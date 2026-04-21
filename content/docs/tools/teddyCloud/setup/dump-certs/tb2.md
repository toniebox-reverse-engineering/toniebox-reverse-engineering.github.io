---
title: "Toniebox 2"
description: ""
---

# Toniebox 2
You'll have to manually extract it from the flash of the box with a SOP8 clamp directly from the memory or by desoldering it. Reading in-circuit can be tricky, but is possible. I recommend flashrom as tool for that.

## Reading the flash with Pico
As a probe you may use a raspberry pi pico with the serprog protocol firmware for it. It may be downloaded from the [libreboot project](https://rsync.libreboot.org/stable/26.01rev1/roms/libreboot-26.01rev1_serprog_pico.tar.xz)
```
flashrom -p serprog:dev=/dev/ttyACM0:921600 -r tb2-flash.bin --progress
```
You may do multiple reads and compare the result
```
flashrom -p serprog:dev=/dev/ttyACM0:921600 -r tb2-flash.2.bin --progress
diff tb2-flash.bin tb2-flash.2.bin #no output = equal
```

## TBD
The certificates in the flash are encrypted. We are working on a solution to decrypt them.
