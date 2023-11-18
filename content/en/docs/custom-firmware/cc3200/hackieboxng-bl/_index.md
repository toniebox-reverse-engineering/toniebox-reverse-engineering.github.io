---
title: "HackieboxNG Custom Bootloader"
description: "Start and read!"
weight: 10
bookCollapseSection: true
---

# HackieboxNG Custom Bootloader

## Introduction
HackieboxNG is the next generation bootloader for your cc3200 based toniebox!

## HackieboxNG SD bootloader
The HackieboxNG SD bootloader consists of two stages. The first stage is a sd bootloader which is called preload. This preloader then runs the stage two bootloader from microSD. This is the bootloader itself that allows selecting + running different firmwares.

### Features
* Nine firmware slots
* Loading any CC3200 standard firmware
* Loading the original firmware directly
* Simulate the OFW bootloaders behaviour
* Patching binaries in memory for ex. domain name changes for teddyCloud, enable SLIX tags or disable charger wakeup ([more](ofw-patches))
* Highly configurable via json files

### Installation
Please take a look into the [wiki](install)
