---
title: "Hackiebox API"
description: "Start and read!"
type: page
---

This entry contains the api documentation. 

# Endpoint /api/ajax
GET Parameter "cmd" as command name
## get-config - Sends the current config as JSON
### Result
```
{"version":2,"battery":{"voltageFactor":67690,"voltageChargerFactor":71907,"minimalAdc":2400,"sleepMinutes":15},"buttonEars":{"longPressMs":1000,"veryLongPressMs":10000},"wifi":{"ssid":"","password":""}}
```
## get-dir Directory file listing
### Parameters
#### dir
Contains the target directory path
### Result
```
{"files":[{"name":"CONTENT","size":0,"time":11942,"date":20662,"dir":true},{"name":"revvox","size":0,"time":39661,"date":20661,"dir":true},{"name":"initsd.crc","size":5123,"time":0,"date":33,"dir":false},{"name":"hackiebox.config.json","size":228,"time":23491,"date":20665,"dir":false}]}
```
## get-file - Sends the specified file
### Parameters
#### filepath
Contains the target file path
#### start (optional)
Ignore bytes until [start]
#### length (optional)
Read max [length] bytes
### Result
Raw filestream

## get-flash-file - Sends the specified file from flash
### Parameters
#### filepath
Contains the target file path
#### start (optional)
Ignore bytes until [start]
#### length (optional)
Read max [length] bytes
### Result
Raw filestream

## copy-file - Copy file to a different location (TBD)
### Parameters
#### source
Full [source] file path
#### target
Full [target] file path
### Result
404 or { "success": true }
## move-file - Move file to a different location
### Parameters
#### source
Full [source] file path
#### target
Full [target] file path
### Result
404 or { "success": true }
## delete-file - Delete file
### Parameters
#### filepath
Full [filepath]
### Result
404 or { "success": true }
## delete-flash-file - Delete flash file
### Parameters
#### filepath
Full [filepath]
### Result
404 or { "success": true }

## create-dir - Create dir
### Parameters
#### dir
Full [dir] file path
### Result
404 or { "success": true }
## copy-dir - Copy dir to a different location (TBD)
### Parameters
#### source
Full [source] file path
#### target
Full [target] file path
### Result
404 or { "success": true }
## move-dir - Move dir to a different location
### Parameters
#### source
Full [source] dir path
#### target
Full [target] dir path
### Result
404 or { "success": true }
## delete-dir - Delete directory
Removes an empty directory
### Parameters
#### dir
Full [dir]
### Result
404 or { "success": true }

## box-power power related commands
### Parameters
#### sub
Subcommand
### Subcommands
#### reset
Restarts the device
#### hibernate
Goes into hibernation
### Result
404 or { "success": true }

## box-battery battery related commands
### Parameters
#### sub
Subcommand
### Subcommands
#### start-test
Starts a battery test (/revvox/batteryTest.csv)
### Result
404 or { "success": true }
#### stats
Prints current battery stats
### Result
{"charging":true,"low":false,"critical":false,"adcRaw":3286,"voltage":456,"testActive":false,"testActiveMinutes":0}

## box-rfid RFID related commands
### Parameters
#### sub
Subcommand
### Subcommands
#### uid
Sends uid information
##### Result
```
{"active":true,"uid":"13:37:13:37:13:37:13:37","uidRaw":[55,19,55,19,55,19,55,19]}
```
#### memory
extracts tags memory (8 blocks a 4 bytes, SLIX-L)
##### Result
```
[15,65,137,198,147,188,12,253,23,109,30,171,162,104,124,119,133,224,173,140,118,106,154,234,71,28,135,255,157,122,36,136]
```

## cli - Command line interface
### Parameters
#### cli
Command, type "help" for more details.
### Result
Raw command line result.
Example for help
```
# help
Help:

i2c [-read] [-write] -a/ddress <value> -r/egister <value> [-v/alue <value>] [-l/ength <1>] [-o/utput <b>]
 Access I2C

beep [-m/idi-id <60>] [-l/ength <200>]
 Beep with build-in DAC synthesizer

help
 Show this screen
```

# Endpoint /api/upload/*
Standard POST file upload. Attention, only one upload at once supported or you may get corrupt files!
## Parameters
### filepath
Specifies the target file path.
### filesize
(Flash file upload only)
### overwrite
Enable to allow overwriting files
### start (optional)
Skip bytes until [start]

### /api/upload/file
Upload file to sd card.
### /api/upload/flash-file
Upload file to flash