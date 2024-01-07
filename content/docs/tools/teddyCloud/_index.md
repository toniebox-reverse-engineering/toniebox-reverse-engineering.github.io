---
title: "teddyCloud"
description: "teddyCloud is an open source replacement server for the tonies cloud."
bookCollapseSection: true
---
# teddyCloud

## Features
teddyCloud is an alternative server for your Toniebox, allowing you to host the cloud services locally.
This gives you the control about which data is sent to the original manufacturer's cloud and allows you
to host your own figurine audio files on e.g. your NAS or any other server.

## Docker hints
The docker container automatically generates the server certificates on first run. You can extract the ```certs/server/ca.der``` for your box after that. 

An example [docker-compose.yaml can be found within the docker subdir.](https://github.com/toniebox-reverse-engineering/teddycloud/blob/master/docker/docker-compose.yaml)
Please beware that port 443 cannot be remapped. The client certificate authentication needs to be done by teddyCloud. There is no SNI.


## Preparation
You may need to connect your Toniebox to your wifi and update its firmware. Many boxes are shipped with a production firmware only that needs to be updated, before the box works as it should.

## Device specific preparation
Please check wether you got a Toniebox with a CC3200, CC3235 or ESP32 and continue with the following steps:
* [Dump CA and client certificates](dump-certs)
* [Flash the replacement CA](flash-ca)
* [DNS](dns)

## Additional

### Content
Please put your content into the ```/data/content/default/``` in the same structure as on your toniebox. You can edit ```500304E0.json``` file beside the content files to mark them as live or you can prevent the usage of the Boxine cloud for that tag with the nocloud parameter. By setting a source teddyCloud can stream any content that ffmpeg can decode (urls and files).

### Webinterface
Currently the interface to teddycloud is reachable through the IP of the docker container at port 80 or 443 (depending on your ```docker-compose.yaml```). Changes affecting the toniebox (volume, LED) which are made through this interface will only be reflected onto the toniebox after pressing the big ear for a few seconds until a beep occurs.

As an additional frontend is still being developed, you can reach a second frontend at ```xxx.xxx.xxx/web```. Changes made here are instantly live on the box.
