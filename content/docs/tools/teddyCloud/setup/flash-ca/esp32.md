---
title: "ESP32"
description: ""
---

# ESP32
## Browser based
With teddyCloud you can also write a new image with your custom CA and a hostname/IP so the box connects to teddyCloud.
If you have a Fritzbox you can set it to tc.fritz.box (see CC3200 how to configure the hostname on your Fritzbox), if not set it to the IP of teddyCloud.

Check, that your backup of your flash is okay and you were able to extract the certificates. 

If you successfully read or load the flash of your Toniebox, the patching step is shown. Enter the hostname/ip of your teddyCloud instance. You can now also enter the WiFi credentials. To proceed click [Patch].

![Patching the flash/hostname/wifi](/img/esp32_newgui_flashing_02_patchflash.png)

After the flash is patched, you can proceed with the actuall flashing. Till this step nothing happens to your Toniebox. 

![Flash you Toniebox](/img/esp32_newgui_flashing_03_initialflash.png)

To flash the patched firmware on your Toniebox, disconnect your Toniebox first from your powersource, click [Flash ESP32], confirm the dialog and immediatly connect to powersource again.

![Confirm flashing](/img/esp32_newgui_flashing_04_confirmflash.png)

![Flashing in progress](/img/esp32_newgui_flashing_05_flashing.png)

If the flashing was successful, you can download the unpatched and patched Firmware for your backup.

![Flashing done](/img/esp32_newgui_flashing_06_flashingdone.png)


## Legacy
Replace the original CA within your flash dump with esptool.

```
# copy firmware backup
cp tb.esp32.bin tb.esp32.fakeca.bin

# inject new CA into firmware
teddycloud --esp32-inject tb.esp32.fakeca.bin --source certs/client/esp32-fakeca
# modify IP/hostname (optional)
teddycloud --esp32-hostpatch tb.esp32.fakeca.bin --hostname <YOUR-IP/HOST>

# flash firmware with new CA
esptool.py -b 921600 write_flash 0x0 tb.esp32.fakeca.bin
```

![Flash ESP32 Image](/img/esp32_write_patched_image_with_esptools.png)


Reassamble your Toniebox again. If you already set the teddyCloud hostname, you can skip the [DNS step for the ESP32](../../dns/esp32), if not [continue with DNS step for the ESP32](../../dns/esp32). 

Your Toniebox should now be able to connect to your teddyCloud. Do a freshnesscheck and check if the box appears in the Toniebox management.