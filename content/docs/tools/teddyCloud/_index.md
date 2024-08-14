---
title: "teddyCloud"
description: "teddyCloud is an open source replacement server for the tonies cloud."
bookCollapseSection: true
---
# teddyCloud

## Features
teddyCloud is an alternative server for your Toniebox, allowing you to host the cloud services locally.
This gives you the control about which data is sent to the original manufacturer's cloud and allows you
to host your own figurine audio files on e.g. your [NAS or any other server](setup/supported-server-hardware). 
You can also use teddyCloud on the command line to manipulate esp32 firmware dumps or encode Tonie Audio Files (TAFs). See ```toniecloud --help```.

## Docker hints
The docker container automatically generates the server certificates on first run. You can extract the ```certs/server/ca.der``` for your box after that. 

An example [docker-compose.yaml can be found within the docker subdir.](https://github.com/toniebox-reverse-engineering/teddycloud/blob/master/docker/docker-compose.yaml)
Please beware that port 443 cannot be remapped and you cannot use a reverse proxy like nginx or traefik without passing through the TLS (complex, not recommended). The client certificate authentication needs to be done by teddyCloud. Also, there is no SNI. If you are using docker, you can use macvlan to give the teddyCloud container a dedicated IP address (recommended).

## Preparation
Please connect your Toniebox to your Wi-Fi and update its firmware. Many boxes are shipped with a production firmware that needs to be updated. Otherwise the box won't work as it should. It is not necessary to connect the box to the mytonies app/account. [Connect the box without the setup assistant.](https://support.tonies.com/hc/en-us/articles/4415294030482-How-do-I-set-up-a-Wi-Fi-connection-without-the-setup-assistant)

## Device specific preparation
Please check wether you got a Toniebox with a CC3200, CC3235 or ESP32 and continue with the following steps:
* [Dump CA and client certificates](setup/dump-certs)
* [Flash the replacement CA](setup/flash-ca)
* [DNS](setup/dns)

## Test & Troubleshooting
If you have problems with teddyCloud or just want to be sure everything works as it should, please check our [Test & Troubleshooting site](setup/test-troubleshooting).

## Additional

### Content
Please put your content into the ```/data/content/default/``` in the same structure as on your toniebox. You can edit ```500304E0.json``` file beside the content files to mark them as live or you can prevent the usage of the Boxine cloud for that tag with the nocloud parameter. By setting a source teddyCloud can stream any content that ffmpeg can decode (urls and files). Please check that the file and folders in upper case. If not rename them, otherwise teddyCloud won't serve them to the box!

### Webinterface
Currently the teddyCloud webinterface is reachable through the IP of the docker container at port 80 (depending on your ```docker-compose.yaml```). The 443 port is usally exclusive for the toniebox connection. Changes affecting the toniebox (volume, LED) which are made through this interface will only be reflected onto the toniebox after pressing the big ear for a few seconds until a beep occurs (freshnessCheck).

![teddyCloud Webinterface](/img/teddyCloudWebinterface.png)


As an additional frontend is still being developed, you can reach a second frontend at ```xxx.xxx.xxx/web```. Changes made here are instantly live on the box.



## Links
* [GitHub](https://github.com/toniebox-reverse-engineering/teddycloud)
* [Releases](https://github.com/toniebox-reverse-engineering/teddycloud/releases)
