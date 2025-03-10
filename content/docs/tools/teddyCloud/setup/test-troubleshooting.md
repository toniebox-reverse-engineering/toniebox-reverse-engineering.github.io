---
title: "Test & Troubleshooting"
description: "This site shows how to test if teddyCloud is running fine and shows steps to solve occuring problems."
bookCollapseSection: true
---
# teddyCloud - Test & Troubleshooting
## Connection toniebox to teddyCloud
First of all please be sure the cloud is not enabled in the webinterface. Then close all browser windows to teddyClouds webinterface and open the logs of teddyCloud. If you are using docker you can access them via: `docker logs teddycloud`.

To be sure your toniebox can connect to teddyCloud we do a so called "freshnessCheck". This can be initiated by pressing one ear of the box until the LED is pulsing blue.

### Error: Codeword turtle (Schildkröte)
If the box is flashing red and shouts the codeword turtle, be sure teddyCloud is running and the box can connect to your cloud. Check the [DNS step](../dns).

### Error: Codeword owl (Eule)
If the box is flashing red and shouts the codeword `owl`, there might be several different problems:

__1. docker-compose.yaml faulty__

Problem:

The ports in your `docker-compose.yaml` are still marked as comment (with a `#` in front of each line). The consequence: all 3 ports are not accessible outside the container, the teddyCloud GUI is not reachable with the local IP address of your host (192.168.x.x) and https communication between the box and teddyCloud is not possible. 

Solution:

- Remove the `#` in front of each line in your `docker-compose.yaml`:
```
ports:
  - 80:80 #optional (for the webinterface)
  - 8443:8443 #optional (for the webinterface)
  - 443:443 #Port is needed for the connection for the box, must not be changed!
```
- Restart Teddycloud: `docker-compose up -d`

- Verify open ports: `docker container port teddycloud`
```
80/tcp -> 0.0.0.0:80
443/tcp -> 0.0.0.0:443
8443/tcp -> 0.0.0.0:8443
```

__2. Wrong teddyCloud host ip address__

Problem:

The Docker daemon uses its own virtual ip address range for networking. By default, it starts with `172.*.*.*` (`172.16.*.*`, `172.17.*.*` etc.). Please **don't** use these ip addresses. They are only reachable on your Docker host. 

Solution:

When applying a [DNS patch](https://tonies-wiki.revvox.de/docs/tools/teddycloud/setup/dns) for your box, always use the local ip address of your teddyCloud host machine, normally starting with `192.168.*.*`. Find the correct ip address in the network settings of your Router or run `hostname -I | awk '{print $1}'` on your teddyCloud host (Linux).


__3. Wrong certificates__

Problem:

The Teddycloud server CA certificate (automatically generated upon start) is not the same as on the box.

Solution:

Verify that teddyClouds `certs/server/ca.der` is identical to the one on box. Please check the [Flash replacement CA step](../flash-ca).

Sometimes you'll need to regenerate teddyClouds certificates as it may be defective. For that delete all files in `certs/server/ca.der` and restart teddyCloud. We had the case that an esp32 based box worked with the certificate, but the cc3200 based one had trouble. After regenerating the certificates it was fine.

__4. Wrong DNS resolution__

Problem:

The Box can't reach Boxine cloud (e.g. when using an original Tonie for the first time)

Solution:

- Check the [DNS step](../dns). 

- If you use a reverse proxy like `nginx` or `traefik` between teddyCloud and your box: this is not supported, teddyCloud needs its own dedicated IP address.

Example log output:
```
INFO |server.c:0574:server_init()| 1 open HTTPS connections
INFO |server.c:0574:server_init()| 0 open HTTPS connections
```

## Connection teddyCloud to boxine cloud
Please visit `http://<teddyCloud>/v1/time` and check the logs. You should see a number which is the current unixtime. The logs should look like that:
```
INFO |handler_cloud.c:0038:handleCloudTime()|  >> respond with current time
INFO |mqtt.c:0684:mqtt_init_box()| Skipping client 'Toniebox' (cn: 'default')
```
Please visit the webinterface and enable `Cloud enabled` and `Forward 'time`. Then visit `http://<teddyCloud>/v1/time` again and check the logs. You should see the time again and following log:
### Log is fine
```
INFO |mqtt.c:0684:mqtt_init_box()| Skipping client 'Toniebox' (cn: 'default')
INFO |cloud_request.c:0158:web_request()| Connecting to HTTP server prod.de.tbs.toys:443...
INFO |cloud_request.c:0208:web_request()|   trying IP: 18.156.186.144
INFO |cloud_request.c:0036:httpClientTlsInitCallbackBase()| Initializing TLS...
INFO |cloud_request.c:0071:httpClientTlsInitCallbackBase()| Initializing TLS done
INFO |cloud_request.c:0308:web_request()| HTTP code: 200
INFO |handler.c:0056:cbrCloudHeaderPassthrough()| >> cbrCloudHeaderPassthrough: Server = openresty
INFO |handler.c:0056:cbrCloudHeaderPassthrough()| >> cbrCloudHeaderPassthrough: Date = Sat, 26 Jan 2024 18:48:23 GMT
INFO |handler.c:0056:cbrCloudHeaderPassthrough()| >> cbrCloudHeaderPassthrough: Content-Type = text/plain; charset=utf-8
INFO |cloud_request.c:0339:web_request()| Content-Type is text/plain; charset=utf-8
INFO |handler.c:0056:cbrCloudHeaderPassthrough()| >> cbrCloudHeaderPassthrough: Content-Length = 10
[...]
INFO |handler.c:0061:cbrCloudHeaderPassthrough()| >> cbrCloudHeaderPassthrough: NULL
INFO |cloud_request.c:0386:web_request()| Response: '1706384903'
INFO |handler.c:0225:cbrCloudServerDiscoPassthrough()| >> cbrCloudServerDiscoPassthrough
INFO |cloud_request.c:0417:web_request()| Connection closed
```
### Error case - Missing client certificates
```
INFO |handler_cloud.c:0038:handleCloudTime()|  >> respond with current time
INFO |mqtt.c:0684:mqtt_init_box()| Skipping client 'Toniebox' (cn: 'default')
INFO |cloud_request.c:0158:web_request()| Connecting to HTTP server prod.de.tbs.toys:443...
INFO |cloud_request.c:0208:web_request()|   trying IP: 3.69.182.181
INFO |cloud_request.c:0036:httpClientTlsInitCallbackBase()| Initializing TLS...
ERROR|cloud_request.c:0218:web_request()| Failed to connect to HTTP server! Error=2
```
Please check your client certificates from the [dump CA and client certificates step](../dump-certs).
### Error case - prod.de.tbs.toys is resolved to teddyCloud
```
INFO |cloud_request.c:0158:web_request| Connecting to HTTP server prod.de.tbs.toys:443...
INFO |cloud_request.c:0208:web_request|   trying IP: 192.168.xxx.xxx
INFO |cloud_request.c:0036:httpClientTlsInitCallbackBase| Initializing TLS...
INFO |cloud_request.c:0071:httpClientTlsInitCallbackBase| Initializing TLS done
ERROR|cloud_request.c:0218:web_request| Failed to connect to HTTP server! Error=537
INFO |handler_cloud.c:0692:handleCloudFreshnessCheck| Freshness check response: size=14, content=
WARN |tls_server_fsm.c:0260:tlsPerformServerHandshake| TLS handshake failure!
```
Please check if `prod.de.tbs.toys` and `rtnl.bxcl.de` are resolved to your teddyCloud instance within its own container. Please either give your Toniebox a custom DNS that resolved them to teddyCloud or the teddyCloud instance a DNS that does not tamper the urls.
