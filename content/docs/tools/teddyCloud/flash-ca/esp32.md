---
title: "ESP32"
description: ""
---

# ESP32
## Browser based
With teddyCloud you can also write a new image with your custom CA and a DNS/IP so the box connects to teddyCloud.
If you have a Fritzbox you can set it to tc.fritz.box (see CC3200 how to configure the hostname on your Fritzbox), if not set it to the IP of teddyCloud.

Check, that your backup of your flash is okay and you were able to extract the certificates. 

![Finished reading the flash/hostname](/img/esp32_read_flash_finished_webui.png)

![Patchimage](/img/esp32_patch_image_webui.png)

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

![Flash ESP32 Image](/img/esp32_write_patched_image_with_esptools.png)


Set the hostname/IP via commandline
```
# copy firmware backup
cp tb.esp32.fakeca.bin tb.esp32.fakeca.IP.bin
# inject new IP into firmware
teddycloud ESP32HOST tb.esp32.fakeca.IP.bin 192.168.178.49
# Fix the firmware, otherwise the TonieBox hang in a reboot loop
teddycloud ESP32FIXUP tb.esp32.fakeca.IP.bin
# flash firmware with new IP
esptool.py -b 921600 write_flash 0x0 tb.esp32.fakeca.IP.bin
```

The output after injection of the hostname/IP should include the following lines
```
INFO |esp32.c:0990:esp32_patch_host()| Patching hostnames in 'tb.esp32.fakeca.IP.bin'
INFO |esp32.c:1038:esp32_patch_host()|  replaced RTNL host 2 times
INFO |esp32.c:1040:esp32_patch_host()|  replaced API host 2 times

```
If it states `0 times` than the hostname/IP is already changed (maybe to another value). ESP32HOST does only works on fwirmware with the original hostnames, as it does some search & replace.


Alternatively, your Fritz.Box can configured to return your TeddyCloud IP during domain name resolution: [Domain Name resolution](../../dns/esp32)
