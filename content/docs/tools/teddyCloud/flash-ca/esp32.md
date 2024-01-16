---
title: "ESP32"
description: ""
---

# ESP32
## Browser based
With teddyCloud you can also write a new image with your custom CA and a DNS/IP so the box connects to teddyCloud.
If you have a Fritzbox you can set it to tc.fritz.box (see CC3200 how to configure the hostname on your Fritzbox), if not set it to the IP of teddyCloud.

## Legacy
Replace the original CA within your flash dump with esptool.

```
# copy firmware backup
cp tb.esp32.bin tb.esp32.fakeca.bin
# inject new CA into firmware
teddycloud ESP32CERT inject tb.esp32.fakeca.bin certs/client/esp32-fakeca
# flash firmware with new CA
esptool.py -b 921600 write_flash 0x0 tb.esp32.fakeca.bin
```

Flash ESP32 Image
![Screenshot 2024-01-15 085240](https://github.com/toniebox-reverse-engineering/toniebox-reverse-engineering.github.io/assets/5244525/e7a9d38a-b779-40e1-84fb-bf13f75ac718)


[Please continue with DNS step for the ESP32](../../dns/esp32)
