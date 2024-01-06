---
title: "Dump certificates"
description: "Start and read!"
weight: 10
bookCollapseSection: true
---

# Dump Certificates

## Generate certificates
On first run teddyCloud will generate the CA and certificates with the starting date 2015-11-03: ```./gencerts.sh``` Those will be placed in ```/certs/server/```.
This also generates the replacement CA for the toniebox ```certs/server/ca.der```.

## Dump certificates of your toniebox
You'll need the ```flash:/cert/ca.der``` (Boxine CA), ```flash:/cert/client.der``` (Client Cert) and ```flash:/cert/private.der``` (Client private key). Place those files under ```/certs/client/*```. You can either power the box with the battery (be sure it is not empty) or with the power supply. (recommended)

## Device specific
Please continue with the device specific step for your device: 
* [CC3200](cc3200)
* [CC3235](cc3235)
* [ESP32](esp32)
