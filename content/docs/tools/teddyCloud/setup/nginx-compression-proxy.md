---
title: "nginx Compression Proxy"
description: "Speed up teddyCloud web interface loading by adding nginx as a compression proxy."
weight: 40
---
# nginx Compression Proxy for teddyCloud

teddyCloud's web interface transfers a significant amount of data on initial page load (over 5MB uncompressed). When accessing teddyCloud remotely over slower connections, this can result in noticeable loading times.

Adding nginx as a compression proxy between clients and teddyCloud significantly reduces transfer sizes and improves loading performance.

## How it works

```
Client ◄──gzip compressed──► nginx ◄──uncompressed──► teddyCloud:80
```

nginx intercepts responses from teddyCloud and compresses them before sending to the client. This is transparent to both the client and teddyCloud.

## Configuration

### docker-compose.yml

Add the nginx service to your teddyCloud stack:

```yaml
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

  nginx:
    image: nginx:alpine
    container_name: teddycloud-nginx
    restart: unless-stopped
    networks:
      - app
    ports:
      - "8080:80"  # Adjust port as needed
    volumes:
      - ./config/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      teddycloud:
        condition: service_healthy

networks:
  app:
    driver: bridge
```

### nginx.conf

Create `config/nginx/nginx.conf`:

```nginx
events {
    worker_connections 1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    # Enable gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_comp_level 6;
    gzip_types text/plain text/css application/json application/javascript
               text/xml application/xml application/xml+rss text/javascript
               image/svg+xml application/wasm;

    upstream teddycloud {
        server teddycloud:80;
    }

    server {
        listen 80;
        server_name _;

        location / {
            proxy_pass http://teddycloud;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # WebSocket support for live updates
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }
}
```

## Usage

After adding nginx, access the teddyCloud web interface through nginx instead of directly:

| Access | URL |
|--------|-----|
| Direct (uncompressed) | `http://teddycloud-host:80` |
| Via nginx (compressed) | `http://teddycloud-host:8080` |

When using a reverse proxy like Traefik or Pangolin, point it to the nginx container instead of teddyCloud directly.

## Compression Results

Typical compression ratios for teddyCloud web assets:

| Content Type | Compression |
|--------------|-------------|
| JavaScript | ~70-80% reduction |
| CSS | ~80-85% reduction |
| JSON | ~85-90% reduction |
| HTML | ~70-75% reduction |

The initial 5MB+ page load can be reduced to under 1.5MB with gzip compression.
