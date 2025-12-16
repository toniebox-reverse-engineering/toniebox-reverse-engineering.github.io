---
title: "External Access with Pangolin & frp"
description: "Securely expose teddyCloud to the internet using Pangolin for SSO-protected web access and frp for Toniebox TCP passthrough."
weight: 50
---
# External Access: Web Interface via Pangolin + TCP Passthrough via frp

When exposing teddyCloud to the internet, you need to handle two different types of traffic:
- **Web interface** - Can use standard HTTPS with SSO protection
- **Toniebox connections** - Require raw TCP passthrough to preserve mTLS authentication

## The Challenge

Tonieboxes use mutual TLS (mTLS) authentication. Each box has a unique client certificate and expects a direct TLS connection with the server. Standard reverse proxies (Traefik, nginx, Caddy) terminate TLS and break this authentication.

```
Toniebox ──TLS+ClientCert──► Reverse Proxy ──breaks mTLS──► teddyCloud
                                    ↑
                            TLS terminated here
```

## The Solution

Use two separate systems:
- **Pangolin** - For the web interface with SSO protection
- **frp** - For raw TCP passthrough to Tonieboxes

```
Browser    ──► teddy.example.com  ──► Pangolin/SSO ──► teddyCloud:80
Toniebox   ──► tdy.example.net    ──► frp (raw TCP) ──► teddyCloud:443
```

## Prerequisites

- **VPS with 2 public IP addresses** - This is mandatory. Both the web interface (Pangolin) and the Toniebox TCP passthrough (frp) require port 443. Since you cannot bind two different services to the same port on the same IP, you need a dedicated IP address for each:
  - Primary IP (e.g., 203.0.113.10): Pangolin/Traefik for HTTPS web traffic
  - Secondary IP (e.g., 203.0.113.11): frp for raw TCP passthrough
- Home server running Docker
- Domain names with DNS control
- Working Pangolin installation

## teddyCloud Stack Configuration

### Directory Structure
```
/opt/teddy/stack/
├── docker-compose.yml
├── .env
└── config/
    ├── teddycloud/
    ├── nginx/
    │   └── nginx.conf
    ├── newt/
    │   └── blueprints.yml
    └── frpc/
        └── frpc.toml
```

### docker-compose.yml
```yaml
networks:
  app:
    driver: bridge

services:
  teddycloud:
    image: ghcr.io/toniebox-reverse-engineering/teddycloud:latest
    container_name: teddycloud
    restart: unless-stopped
    networks:
      - app
    ports:
      - "443:443"
    volumes:
      - ./config/teddycloud/certs:/teddycloud/certs
      - ./config/teddycloud/config:/teddycloud/config
      - ./config/teddycloud/data/content:/teddycloud/data/content
      - ./config/teddycloud/data/library:/teddycloud/data/library
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:80/web/"]
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 20s

  # nginx compression proxy - see /docs/tools/teddyCloud/setup/nginx-compression-proxy
  nginx:
    image: nginx:alpine
    container_name: teddycloud-nginx
    restart: unless-stopped
    networks:
      - app
    volumes:
      - ./config/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      teddycloud:
        condition: service_healthy

  newt:
    image: fosrl/newt
    container_name: stack-newt
    restart: unless-stopped
    command: ["newt", "--blueprint-file", "/blueprints/blueprints.yml"]
    networks:
      - app
    environment:
      PANGOLIN_ENDPOINT: ${PANGOLIN_ENDPOINT}
      NEWT_ID: ${NEWT_ID}
      NEWT_SECRET: ${NEWT_SECRET}
    volumes:
      - ./config/newt:/blueprints:ro
    depends_on:
      nginx:
        condition: service_started

  frpc:
    image: fatedier/frpc:v0.65.0
    container_name: frpc
    restart: unless-stopped
    networks:
      - app
    volumes:
      - ./config/frpc/frpc.toml:/etc/frp/frpc.toml:ro
    command: ["-c", "/etc/frp/frpc.toml"]
    depends_on:
      teddycloud:
        condition: service_healthy
```

### Environment File (.env)
```bash
PANGOLIN_ENDPOINT=https://admin.example.com
NEWT_ID=your-newt-id
NEWT_SECRET=your-newt-secret
```

### nginx Configuration

The nginx compression proxy significantly reduces transfer sizes for the teddyCloud web interface (from 5MB+ to under 1.5MB). See [nginx Compression Proxy]({{< relref "../setup/nginx-compression-proxy" >}}) for the full configuration.

## Web Interface via Pangolin

### Newt Blueprint (config/newt/blueprints.yml)
```yaml
proxy-resources:
  teddycloud:
    name: "TeddyCloud"
    protocol: http
    full-domain: teddy.example.com
    ssl: true
    auth:
      sso-enabled: true
    targets:
      - hostname: nginx
        method: http
        port: 80
        path: /
        path-match: prefix
    rules:
      - action: allow
        match: path
        value: /*
```

This routes `teddy.example.com` through Pangolin with SSO protection.

## TCP Passthrough via frp

frp provides reliable raw TCP passthrough without inspecting or terminating TLS.

### frps Server (on VPS)

Create `/opt/frps/docker-compose.yml`:
```yaml
services:
  frps:
    image: fatedier/frps:v0.65.0
    container_name: frps
    restart: unless-stopped
    volumes:
      - ./config/frps.toml:/etc/frp/frps.toml:ro
    ports:
      - "7000:7000"
      - "203.0.113.11:443:443"  # Secondary IP for TCP passthrough
    command: ["-c", "/etc/frp/frps.toml"]
```

Create `/opt/frps/config/frps.toml`:
```toml
bindAddr = "0.0.0.0"
bindPort = 7000

auth.method = "token"
auth.token = "your-secure-random-token"

transport.tls.force = true

log.to = "console"
log.level = "info"
```

Generate a secure token:
```
openssl rand -base64 32
```

### frpc Client (config/frpc/frpc.toml)
```toml
serverAddr = "admin.example.net"
serverPort = 7000

auth.method = "token"
auth.token = "your-secure-random-token"

transport.tls.enable = true

[[proxies]]
name = "teddycloud-tcp"
type = "tcp"
localIP = "teddycloud"
localPort = 443
remotePort = 443
```

## DNS Configuration

| Hostname | Type | Value | Purpose |
|----------|------|-------|---------|
| teddy.example.com | A | 203.0.113.10 | Web interface (via Pangolin) |
| tdy.example.net | A | 203.0.113.11 | TCP passthrough (via frp) |

If using Cloudflare, disable the proxy (DNS-only) for `tdy.example.net` to prevent TLS termination.

Configure your Toniebox DNS to redirect `prod.de.tbs.toys` to `tdy.example.net`.

## Deployment

1. Generate a secure frp token
2. Deploy frps on the VPS:
   ```
   cd /opt/frps && docker compose up -d
   ```
3. Deploy the teddyCloud stack:
   ```
   cd /opt/teddy/stack && docker compose up -d
   ```

## Verification

Check frp connection:
```
docker logs frps | grep "proxy"
docker logs frpc | grep "proxy"
```

Test TCP passthrough:
```
nc -zv tdy.example.net 443
```

Test web interface:
```
curl -sI https://teddy.example.com/web/
```

## Troubleshooting

### frpc cannot connect
- Verify frps is running: `docker ps | grep frps`
- Check port 7000 is accessible: `nc -zv admin.example.net 7000`
- Verify tokens match in both configs

### Port already in use
Check what's using port 443:
```
ss -tlnp | grep ':443'
```

### Browser can't connect to tdy.example.net
This is expected - that port serves Toniebox certificates only. Use `teddy.example.com` for browser access.

## References

- [frp GitHub](https://github.com/fatedier/frp)
- [Pangolin Documentation](https://docs.pangolin.net)
- [teddyCloud GitHub](https://github.com/toniebox-reverse-engineering/teddycloud)
