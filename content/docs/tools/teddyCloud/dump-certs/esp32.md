---
title: "ESP32"
description: ""
---

# ESP32
You can extract the flash memory via the debug port of the box and the esptool. Keep your backup! Please use a recent version of esptool. (>v4.4)
Please connect the jumper J100 (Boot) and reset the box to put it into the required UARTmode. Connect your 3.3V UART to J103 (TxD, RxD, GND).
![J103 Pinout](/img/tb-esp32-uart.jpg)

If connected with the Boot jumper, the box just start in "DOWNLOAD (USB/UART0)" mode (Check with a serial monitor) and the LED will be off. Beware, if the serial monitor is open it will block esptool.py from accessing the esp. If you get a "BROWNOUT_RST" check your power supply / battery. "SPI_FAST_FLASH_BOOT" indicates a boot without the J100 jumper. 

## Browser based
You can use the build in ESP32 box flashing tool in the webinterface of teddyCloud to backup your box with "Read ESP32".
After that you can manually extract them into the ```/certs/client/``` directory. You can either do that with the teddycloud executable on your computer or you may do it via the docker shell `docker exec -it <container-name> bash`.
```
# Please check the filename of your backup
# Be sure you are in the teddycloud directory
# cd /teddycloud/ # just for docker
teddycloud ESP32CERT extract data/firmware/ESP32_<mac>.bin certs/client
```
Please check the filename of the extracted certs, especially the case! Change them to lowercase if they are uppercase.
```
mv certs/client/CLIENT.DER certs/client/client.der
mv certs/client/PRIVATE.DER certs/client/private.der
mv certs/client/CA.DER certs/client/ca.der
```

![Read flash](/static/img/esp32_read_flash_webui.png)

![Finished reading the flash](/static/img/esp32_read_flash_finished_webui.png)

![Patchimage](/static/img/esp32_patch_image_webui.png)



## Legacy
```
# extract firmware
esptool.py -b 921600 read_flash 0x0 0x800000 tb.esp32.bin
# extract certficates from firmware
mkdir certs/client/esp32
teddycloud ESP32CERT extract tb.esp32.bin certs/client/esp32
# Copy box certificates to teddyCloud
cp certs/client/esp32/CLIENT.DER  certs/client/client.der
cp certs/client/esp32/PRIVATE.DER  certs/client/private.der
cp certs/client/esp32/CA.DER  certs/client/ca.der

# Copy certificates to temporary dir
mkdir certs/client/esp32-fakeca
cp certs/client/esp32/CLIENT.DER certs/client/esp32-fakeca/
cp certs/client/esp32/PRIVATE.DER certs/client/esp32-fakeca/
cp certs/server/ca.der certs/client/esp32-fakeca/CA.DER
```

[Please continue with flash CA step for the ESP32](../../flash-ca/esp32)
