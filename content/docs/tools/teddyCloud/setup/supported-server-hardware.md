---
title: "Supported NAS/Server hardware"
description: "list of supported NAS/server hardware (incomplete)"
bookCollapseSection: true
---
# Supported NAS/Server hardware

TeddyCloud is supported on nearly all servers utilizing the x86_64, arm64, and armhf (linux/arm/v7) architectures, as Docker containers are available for these platforms.
Below is a compilation of NAS/server hardware that has been successfully utilized to host teddyCloud:

| NAS / Server hardware           | Limitations                                                                                                                                                                                        |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Raspberry Pi 5                  | -                                                                                                                                                                                                  |
| Raspberry Pi 4                  | -                                                                                                                                                                                                  |
| Raspberry Pi 3 Model B Rev 1.2. | Having 3 or more docker container runing same time, could lead into crash (depend on containers task load e.g. jellyfin). Having teddycloud container runing alone, never was an issue |
| Raspberry Pi Zero 2             | slow on initial startup (expect 20 minutes and more for initial certificate creation!), encoding also very slow, possible crashes in some situations, but in general usable                        |
| Synology NAS DS923+ (DSM 7.2.1) | -                                                                                                                                                                                                  |
| Raspberry Pi Zero               | Docker installation is a little bit tricky, getting it running could take some extra steps see https://forum.revvox.de/t/hardware-requirements-does-teddycloud-run-on-a-raspberry-zero-1st-gen/309 |
| Raspberry Pi 1 Model A Rev 1.1  | Works with Raspberry Pi OS (32-Bit) and the teddycloud build for Alpine Linux (see https://github.com/toniebox-reverse-engineering/teddycloud/issues/225)                                          |
| ...                             |                                                                                                                                                                                                    |

Consider using Alpine container instead of Ubuntu / Debian container if your server has limited resources.

If you're utilizing alternative NAS/server hardware configurations, your contribution is welcome. Feel free to share your experience and insights with the community.

## Synology NAS Setup Guide

TeddyCloud requires port 443 to be connected from a Tonnie Box.
But Synology NAS already occupies 443 port.
So you cannot map the container port 443 to the host. 

One way is to configure reverse proxy in nginx using SSL passthrough, but that may interrupt with the existing web service you have.
Here we use a different approach that uses MACVLAN to assign a static IP to the container. The detailed steps are captured below.

### Prerequisite
* You have SSH access to your NAS
* Your router supports to customize DHCP IP range
* You have a very basic understanding of networking, e.g. IP address, IP range, subnet

### Steps

Here we assume the router gateway IP is `192.168.1.1` and the subnet size advertised is 24 (thus total 254 addresses to be assigned, from `192.168.1.2` to `192.168.1.254`).
Also we only need one IP for TeddyCloud so we don't need a large IP range reserved for MACVLAN.

1. Figure out what DHCP IP range you want to reserve and set it in your router. For example, `192.168.1.2-192.168.1.247`.
   Thus the remaining available IPs can be used for MACVLAN, i.e. `192.168.1.248-192.168.1.254`.
2. We are going to create a network named `macvlan0` with just 4 IPs: `192.168.1.248/30` (i.e. `192.168.1.248-192.168.1.251`). SSH into your Synology NAS and run the following:
```
# ovs_eth0 is the primary network interface for Synology NAS, you may change to others as needed
sudo ip link add macvlan0 link ovs_eth0 type macvlan mode bridge
# we are connecting with 192.168.1.250 from a docker container to NAS via MACVLAN
sudo ip addr add 192.168.1.250/32 dev macvlan0
sudo ip link set macvlan0 up
sudo ip route add 192.168.1.248/30 dev macvlan0
# create a docker network called macvlan
sudo docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 --ip-range=192.168.1.248/30 --aux-address='host=192.168.1.250' -o parent=ovs_eth0 macvlan0
```
3. Use the following YAML as an example to create a container project in Synology NAS. Here we use `192.168.1.248` as the IP for the TeddyCloud container.
```
version: '3'
networks:
  teddycloud:
    name: macvlan0
    external: true
services:
  teddycloud:
    container_name: teddycloud
    hostname: teddycloud
    image: ghcr.io/toniebox-reverse-engineering/teddycloud:latest
    ports:
      - 80:80 #optional (for the webinterface)
      - 8443:8443 #optional (for the webinterface)
      - 443:443 #Port is needed for the connection for the box, must not be changed!
    volumes:
      - </path/in/your/nas>:/teddycloud/certs:rw
      - </path/in/your/nas>:/teddycloud/config:rw
      - </path/in/your/nas>:/teddycloud/data/content:rw
      - </path/in/your/nas>:/teddycloud/data/library:rw
      - </path/in/your/nas>:/teddycloud/data/firmware:rw
      - </path/in/your/nas>:/teddycloud/data/cache:rw
    restart: unless-stopped
    networks:
      teddycloud:
        ipv4_address: 192.168.1.248
```
4. Visit `https://192.168.1.248:8443` or `http://192.168.1.248`. Profit.

For other NAS systems it should be similar. Feel free to update commands in this guide accordingly.

## Tested Hardware, but not supporting teddyCloud


| NAS / Server hardware | Comment/Discussion/Links |
|-----------------------|--------------------------|
| ...                   |                          |

If you've attempted to deploy teddyCloud on specialized hardware without success, we encourage you to share your experience for community contribution. Conversely, if you've successfully implemented teddyCloud on any of the mentioned hardware platforms, we invite you to document your process on our forum. Additionally, you're welcome to transition your system to one of the officially supported configurations.
