---
title: "Docker Macvlan"
description: "Use Docker Macvlan for a dedicated IP"
bookCollapseSection: true
headless: true
---
# Docker Macvlan Setup

## Prerequisites

Make sure, you have an IP address in your network, which does not get served by the local DHCP server.

## Assumptions for this example

In this example

* the IP address 192.168.1.3 is reserved for teddyCloud 
* in a network 192.168.0.0/23
* with the router having the address 192.168.0.1


## Create Docker Macvlan Network

You create a Docker Macvlan network with the following command:

```
docker network create \
  --driver macvlan \
  --subnet=192.168.0.0/23 \
  --gateway=192.168.0.1 \
  --ip-range=192.168.1.3/32 \
  -o parent=eth1 \
  teddycloud_macvlan
```

Of course you have to adapt all the parameters to your network.

## Adjust docker-compose.yaml

After the Docker Macvlan network has been created, it can be used in the `docker-compose.yaml`.

### Add Docker Macvlan network

At the end of your `docker-compose.yaml` add the following lines to add the Docker Macvlan network:

```
networks:
  teddycloud_macvlan:
    external: true
```

### Use Docker Macvlan in teddyCloud service

Add the networks secion to your teddyCloud service, which are the last three lines of the following snippet

```
services:
  teddycloud:
    …
    networks:
      teddycloud_macvlan:
        ipv4_address: 192.168.1.3
```

## Done

Save the `docker-compose.yaml` file and start the container.

