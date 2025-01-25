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
If the box is flashing red and shouts the codeword owl, be sure teddyClouds `certs/server/ca.der` is identical to the one on box. Please check the [Flash replacement CA step](../flash-ca).

Sometimes you'll need to regenerate teddyClouds certificates as it may be defective. For that delete all files in `certs/server/ca.der` and restart teddyCloud. We had the case that an esp32 based box worked with the certificate, but the cc3200 based one had trouble. After regenerating the certificates it was fine.

This error can also happen if the box tries to reach the boxine cloud. Check the [DNS step](../dns).

Alternativly you may use a reverse proxy like nginx or traefik between teddyCloud and your box. This is not supported, teddyCloud needs its own dedicated IP address.

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

## Flashing back the previous image
If you somehow fail to setup everything but keep your previous image. For an esp32 this is the way to reflash back the image:

    python -m esptool -b 921600 write_flash 0x0 .\Downloads\tb.esp32.bin --flash_size 8MB

Resulting in the following example output
    
    esptool.py v4.8.1
    Found 1 serial ports
    Serial port COM5
    Connecting....
    Detecting chip type... ESP32-S3
    Chip is ESP32-S3 (QFN56) (revision v0.2)
    Features: WiFi, BLE
    Crystal is 40MHz
    MAC: 48:ca:43:48:fd:ac
    Uploading stub...
    Running stub...
    Stub running...
    Changing baud rate to 921600
    Changed.
    Configuring flash size...
    Flash will be erased from 0x00000000 to 0x007fffff...
    SHA digest in image updated
    Compressed 8388608 bytes to 2391441...
    Wrote 8388608 bytes (2391441 compressed) at 0x00000000 in 45.3 seconds (effective 1481.6 kbit/s)...
    Hash of data verified.
    
    Leaving...
    Hard resetting via RTS pin...

