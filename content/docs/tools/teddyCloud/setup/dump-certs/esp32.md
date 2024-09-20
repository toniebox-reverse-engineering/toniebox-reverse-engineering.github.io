---
title: "ESP32"
description: ""
---

# ESP32
You can extract the flash memory via the debug port of the box and the esptool. Keep your backup! Please use a recent version of [esptool](https://github.com/espressif/esptool). (>v4.4)
Please connect the jumper J100 (Boot) and reset the box to put it into the required UARTmode. Connect your 3.3V UART to J103 (TxD, RxD, GND).
![J103 Pinout](/img/tb-esp32-uart.jpg)

If connected with the Boot jumper, the box just start in "DOWNLOAD (USB/UART0)" mode (Check with a serial monitor) and the LED will be off. Beware, if the serial monitor is open it will block esptool.py from accessing the esp. If you get a "BROWNOUT_RST" check your power supply / battery. "SPI_FAST_FLASH_BOOT" indicates a boot without the J100 jumper. 

## Browser based
You can use the build in ESP32 box flashing tool in the webinterface of teddyCloud to backup your box with "Read ESP32".

![Initial Screen ESP32 flash](/img/esp32_gui_flashing_00_initial.png)

Click on [Read Flash]

![Read flash](/img/esp32_gui_flashing_01_readflash.png)

If the flash is read sucessfully, you can download the unpatched firmware.

![Flash successfully read](/img/esp32_gui_flashing_02_patchflash.png)

After that you can manually extract the certificates into the ```/certs/client/``` directory. You can either do that with the teddycloud executable on your computer or you may do it via the docker shell `docker exec -it <container-name> bash`.
```
# Please check the filename of your backup
# Be sure you are in the teddycloud directory
# cd /teddycloud/ # just for docker
teddycloud --esp32-extract data/firmware/ESP32_<mac>.bin --destination certs/client
```
Please check the filename of the extracted certs, especially the case! Change them to lowercase if they are uppercase.
```
mv certs/client/CLIENT.DER certs/client/client.der
mv certs/client/PRIVATE.DER certs/client/private.der
mv certs/client/CA.DER certs/client/ca.der
```

Be sure, that the dump is okay and you are able to extract the certificates. 

[Please continue with flash CA step for the ESP32](../../flash-ca/esp32)

## Legacy
```
# extract firmware
esptool.py -b 921600 read_flash 0x0 0x800000 tb.esp32.bin
# extract certficates from firmware
mkdir certs/client/esp32
teddycloud --esp32-extract tb.esp32.bin --destination certs/client/esp32
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

Be sure, that the dump is okay and you are able to extract the certificates. 

[Please continue with flash CA step for the ESP32](../../flash-ca/esp32)
