# Please use the new HackieboxNG bootloader
[You can find the new Hackiebox bootloader here](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng)


# Description - ATTENTION DEPRECATED!
**ATTENTION - This following text is about the deprecated Hackiebox SD Bootloader based on micropython!**
The custom bootloader is a pre-bootloader that is placed before the original bootloader and can load images from the built in microSD. Usally it just starts the original bootloader.

# Known problems
## micropython
If you are loading micropython, it may replace the current wifi configuration. So you may need to put the box into wifi ap mode (press both ears) and reenter your wifi credentials. The AP may be named "mpy-toniebox-RevvoX-XXXX". If you have used older versions of micropython, the password may be "TeamRevvoX" or "www.wipy.io". To remove the password just install and run the latest version of the toniebox micropython.

# Boot selection
If you press and hold the big ear while starting the toniesbox (or while restarting it) the box now should blink green very short once a while (boots from pre_img.bin). _Keep the big ear pressed all the time or the box will leave the bootloader!_ If you press the small ear it should now blink twice (boots from pre_img1.bin), if you press it again it blinks thrice (boot from pre_img2.bin). Again and it starts over. 
**Keep in mind if you boot a defective image, the box may needs to be restarted by replugging the battery. If you try to boot a non existing image it will boot the first one.**

# Installation
## Backup
Please make a **full file based backup** of your toniebox's flash with [cc3200tool](https://github.com/toniebox-reverse-engineering/cc3200tool).

```
python3 cc.py -p /dev/ttyUSB0 read_all_files targetdir/
```

**Attention: Writing files to flash via micropython creates clutter files on the flash. You may have problems to read it with [cc3200tool](https://github.com/toniebox-reverse-engineering/cc3200tool).** This is just a problem with the cc3200 tool!

## Install bootloader
### Get the bootloader
Download the latest version from [here](https://github.com/toniebox-reverse-engineering/micropython/releases). Search for _prebootmgr_
### Move original bootloader
First of all you need to copy you just backuped original mcuimg.bin from your toniebox to a different location where the custom bootloader can run it.
```
python3 cc.py -p /dev/ttyUSB0 read_file /sys/mcuimg.bin pre-img.bin
python3 cc.py -p /dev/ttyUSB0 write_file pre-img.bin /sys/pre-img.bin
```
### Install bootloader
```
python3 cc.py -p /dev/ttyUSB0 write_file pre_bootloader.bin /sys/mcuimg.bin
```
### Or as oneliner
```
python3 cc.py -p /dev/ttyUSB0 read_file /sys/mcuimg.bin mcuimg.bin write_file mcuimg.bin /sys/pre-img.bin write_file pre_bootloader.bin /sys/mcuimg.bin

```
## SD 
### Directory structure
Create following directory structure on the sd card: "/revvox/boot/"
### File structure
Place following files into the the created "/revvox/boot/" directory. 
Only the "pre-img.bin" is mandatory.

1. pre-img.bin # You should place the original bootloader here
2. pre-img1.bin # Usally for the custom firmware (in progress) - optional
3. pre-img2.bin # Recommened for backup CFW image - optional
4. pre-img3.bin # Used for micropython in the past - optional

# Other
## Error codes
If the bootloader detects a problem, it blinks green in a defined pattern:
### SD related codes
If a sd related problem occurs, the box combines two patterns. The first one indicates where the problem roughly occured. The second one gives you more information about it.
#### First pattern
##### SD not found - 2x500ms, wait 500ms
Please check if the sd is placed in the holder correctly and the sd is okay. The OFW will blink in red and shut off.
##### File could not be opened - 3x500ms, wait 500ms
Problem opening the firmware file
##### File could not be read - 4x500ms, wait 500ms
Problem reading the firmware file
#### Second pattern (X times 1000ms)
1. FR_DISK_ERR, /* (1) A hard error occurred in the low level disk I/O layer */
2. FR_INT_ERR, /* (2) Assertion failed */
3. FR_NOT_READY, /* (3) The physical drive cannot work */
4. FR_NO_FILE, /* (4) Could not find the file */
5. FR_NO_PATH, /* (5) Could not find the path */
6. FR_INVALID_NAME, /* (6) The path name format is invalid */
7. FR_DENIED, /* (7) Access denied due to prohibited access or directory full */
8. FR_EXIST, /* (8) Access denied due to prohibited access */
9. FR_INVALID_OBJECT, /* (9) The file/directory object is invalid */
10. FR_WRITE_PROTECTED, /* (10) The physical drive is write protected */
11. FR_INVALID_DRIVE, /* (11) The logical drive number is invalid */
12. FR_NOT_ENABLED, /* (12) The volume has no work area */
13. FR_NO_FILESYSTEM, /* (13) There is no valid FAT volume */
14. FR_MKFS_ABORTED, /* (14) The f_mkfs() aborted due to any problem */
15. FR_TIMEOUT, /* (15) Could not get a grant to access the volume within defined period */
16. FR_LOCKED, /* (16) The operation is rejected according to the file sharing policy */
17. FR_NOT_ENOUGH_CORE, /* (17) LFN working buffer could not be allocated */
18. FR_TOO_MANY_OPEN_FILES, /* (18) Number of open files > _FS_LOCK */
19. FR_INVALID_PARAMETER /* (19) Given parameter is invalid */
### Other
#### SD Image could not be loaded - 10x33ms
The box will now fallback to "/revvox/boot/pre-img.bin" image on the sd card.
#### SD fallback image could not be loaded - 20x33ms
The box will now fallback to "/sys/pre-img.bin" image on the flash memory.
#### Application error - 3x33ms, 3x66ms, 3x33ms
Application error. This shouldn't happen. Update the bootloader to the most recent one.

## Updating the bootloader
You may connect via ftp to the box if micropython is loaded. Copy over the firmware files to /flash/sys/*.bin. It is normal that you don't see any files. You may replace the bootloader, the firmware or the bootfiles this way.

## Remove micropython files
```
python3 cc.py -p /dev/ttyUSB0 erase_file __000__.fsb erase_file __001__.fsb erase_file __002__.fsb erase_file __003__.fsb erase_file __004__.fsb erase_file __005__.fsb erase_file __006__.fsb erase_file __007__.fsb erase_file __008__.fsb erase_file __009__.fsb erase_file __010__.fsb erase_file __011__.fsb erase_file __012__.fsb erase_file __013__.fsb erase_file __014__.fsb erase_file __015__.fsb erase_file __016__.fsb erase_file __017__.fsb erase_file __018__.fsb erase_file __019__.fsb erase_file __020__.fsb erase_file __021__.fsb erase_file __022__.fsb erase_file __023__.fsb erase_file __024__.fsb erase_file __025__.fsb erase_file __026__.fsb erase_file __027__.fsb erase_file __028__.fsb erase_file __029__.fsb erase_file __030__.fsb erase_file __031__.fsb
```