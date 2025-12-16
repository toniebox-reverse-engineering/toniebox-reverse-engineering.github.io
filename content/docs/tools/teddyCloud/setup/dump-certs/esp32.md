---
title: "ESP32"
description: ""
---

# ESP32
You can extract the flash memory via the debug port of the box and the esptool. Keep your backup! Please use a recent version of [esptool](https://github.com/espressif/esptool). (>v4.4)
Please connect the jumper J100 (Boot) and reset the box to put it into the required UARTmode. Connect your 3.3V UART to J103 (TxD, RxD, GND). Please avoid USB-C UARTs as they were recently reported as not working as expected.

If you're unsure in which mode the ESP32 is starting:

#### Normal startup mode

LED blinking green, startup sound is played, Serial output:

    ESP-ROM:esp32s3-20210327
    Build:Mar 27 2021
    rst:0x1 (POWERON),boot:0x8 (SPI_FAST_FLASH_BOOT)
    SPIWP:0xee
    mode:DIO, clock div:1
    load:0x3fcd0108,len:0x118
    load:0x403b6000,len:0xb90
    load:0x403ba000,len:0x27f4
    entry 0x403b61c4
    + gibberish


#### Download mode

LED is off, no sound, Serial output:

    ESP-ROM:esp32s3-20210327
    Build:Mar 27 2021
    rst:0x1 (POWERON),boot:0x0 (DOWNLOAD(USB/UART0))
    waiting for download


![J103 Pinout](/img/tb-esp32-uart.jpg)

Beware, if the serial monitor is open it will block esptool.py from accessing the esp. If you get a "BROWNOUT_RST" check your power supply / battery. "SPI_FAST_FLASH_BOOT" indicates a boot without the J100 jumper. 

## Browser based
You can use the build in ESP32 box flashing tool in the webinterface of teddyCloud to backup your box with "Read ESP32".

![Initial Screen ESP32 flash](/img/esp32_gui_flashing_00_initial.png)

Click on [Read Flash]

![Read flash](/img/esp32_gui_flashing_01_readflash.png)

If the flash is read sucessfully, you can download the unpatched firmware.

![Flash successfully read](/img/esp32_gui_flashing_02_patchflash.png)


_(Optionally as you can do it automatically at the end of the flashing process!)_

After that you can manually extract the certificates. You can either do that with the teddycloud executable on your computer or you may do it via the docker shell `docker exec -it <container-name> bash`.
```
# Please check the filename of your backup
# Be sure you are in the TeddyCloud directory
# cd /teddycloud/ # just for docker
mkdir certs/client/<mac>
teddycloud --esp32-extract data/firmware/ESP32_<mac>.bin --destination certs/client/<mac>
```
Please check the filename of the extracted certs, especially the case! Change them to lowercase if they are uppercase.
```
mv certs/client/<mac>/CLIENT.DER certs/client/<mac>/client.der
mv certs/client/<mac>/PRIVATE.DER certs/client/<mac>/private.der
mv certs/client/<mac>/CA.DER certs/client/<mac>/ca.der
```
For your first Toniebox setup with TeddyCloud, copy the certificates into the base client certificates directory. TeddyCloud uses these certificates to authenticate with the official Tonies Cloud, allowing content to be downloaded without Toniebox interaction (e.g., when you click 'Download' on a Tonie in the GUI).
```
cp certs/client/<mac>/client.der certs/client/client.der
cp certs/client/<mac>/private.der certs/client/private.der
cp certs/client/<mac>/ca.der certs/client/ca.der
```

Be sure, that the dump is okay and you are able to extract the certificates. 

[Please continue with flash CA step for the ESP32](../../flash-ca/esp32)

## Legacy
```
# extract firmware
esptool.py -b 921600 read_flash 0x0 0x800000 tb.esp32.bin
# extract certficates from firmware
mkdir certs/client/esp32
mkdir certs/client/<mac>
teddycloud --esp32-extract tb.esp32.bin --destination certs/client/esp32

# Copy box certificates to teddyCloud
cp certs/client/esp32/CLIENT.DER certs/client/<mac>/client.der
cp certs/client/esp32/PRIVATE.DER certs/client/<mac>/private.der
cp certs/client/esp32/CA.DER certs/client/<mac>/ca.der

# In case of first Toniebox setup for TeddyCloud
cp certs/client/<mac>/client.der certs/client/client.der
cp certs/client/<mac>/private.der certs/client/private.der
cp certs/client/<mac>/ca.der certs/client/ca.der

# Copy certificates to temporary dir
mkdir certs/client/esp32-fakeca
cp certs/client/esp32/CLIENT.DER certs/client/esp32-fakeca/
cp certs/client/esp32/PRIVATE.DER certs/client/esp32-fakeca/
cp certs/server/ca.der certs/client/esp32-fakeca/CA.DER
```

Be sure, that the dump is okay and you are able to extract the certificates. 

[Please continue with flash CA step for the ESP32](../../flash-ca/esp32)
