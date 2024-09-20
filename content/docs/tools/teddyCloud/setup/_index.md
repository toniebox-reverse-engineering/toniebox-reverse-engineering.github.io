---
title: "Setup"
description: "Start and read to setup teddyCloud!"
bookCollapseSection: true
headless: true
---
# teddyCloud Setup

Minimal teddyCloud version for this docu is release v0.6.0! Please ensure you are using that or a newer release.

## Docker hints
The docker container automatically generates the server certificates on first run. You can extract the ```certs/server/ca.der``` for your box after that. 

An example [docker-compose.yaml can be found within the docker subdir.](https://github.com/toniebox-reverse-engineering/teddycloud/blob/master/docker/docker-compose.yaml)
Please beware that port 443 cannot be remapped and you cannot use a reverse proxy like nginx or traefik without passing through the TLS (complex, not recommended). The client certificate authentication needs to be done by teddyCloud. Also, there is no SNI. If you are using docker, you can use macvlan to give the teddyCloud container a dedicated IP address (recommended).

## Preparation
Please connect your Toniebox to your Wi-Fi and update its firmware. Many boxes are shipped with a production firmware that needs to be updated. Otherwise the box won't work as it should. It is not necessary to connect the box to the mytonies app/account. [Connect the box without the setup assistant.](https://support.tonies.com/hc/en-us/articles/4415294030482-How-do-I-set-up-a-Wi-Fi-connection-without-the-setup-assistant)

## Device specific preparation
Please check wether you got a Toniebox with a CC3200, CC3235 or ESP32 and continue with the following steps:
* [Dump CA and client certificates](dump-certs)
* [Flash the replacement CA](flash-ca)
* [DNS](dns)

## Test & Troubleshooting
If you have problems with teddyCloud or just want to be sure everything works as it should, please check our [Test & Troubleshooting site](test-troubleshooting).

## Additional

### WebGui
Currently the teddyCloud webinterface is reachable through the IP of the docker container at port 80 (depending on your ```docker-compose.yaml```). To reach the Webinterface with https the port 8443 (depending on your ```docker-compose.yaml```) is used (for example if you want to flash your box). The default https port 443 is usally exclusive for the toniebox connection. 

![teddyCloud Webinterface](/img/teddyCloudWebinterface.png)

Changes affecting the toniebox (volume, LED) which are made through this interface will only be reflected onto the toniebox after pressing the big ear for a few seconds until a beep occurs (freshnessCheck).

### Content (Legacy)
Please put your content into the ```/data/content/default/``` in the same structure as on your toniebox. You can edit ```500304E0.json``` file beside the content files to mark them as live or you can prevent the usage of the Boxine cloud for that tag with the nocloud parameter. By setting a source teddyCloud can stream any content that ffmpeg can decode (urls and files). Please check that the file and folders in upper case. If not rename them, otherwise teddyCloud won't serve them to the box!
