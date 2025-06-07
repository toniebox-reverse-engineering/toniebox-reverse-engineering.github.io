---
title: "Supported NAS/Server hardware"
description: "list of supported NAS/server hardware (incomplete)"
bookCollapseSection: true
---
# Supported NAS/Server hardware

Teddycloud is supported on nearly all servers utilizing the x86_64, arm64, and armhf (linux/arm/v7) architectures, as Docker containers are available for these platforms.
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
| Qnap TS-462 (QTS 5.2.3.3006)    | Works out of the box in Container Station                                                                                                                                                          |
| Proxmox on Dell Wyse 5070       | Works out of the box with Proxmox VE Helper Script. Needs some time till you can access the ip address.                                                                                            |
| ...                             |                                                                                                                                                                                                    |

Consider using Alpine container instead of Ubuntu / Debian container if your server has limited resources.

If you're utilizing alternative NAS/server hardware configurations, your contribution is welcome. Feel free to share your experience and insights with the community.

## Tested Hardware, but not supporting teddyCloud


| NAS / Server hardware | Comment/Discussion/Links |
|-----------------------|--------------------------|
| ...                   |                          |

If you've attempted to deploy teddyCloud on specialized hardware without success, we encourage you to share your experience for community contribution. Conversely, if you've successfully implemented teddyCloud on any of the mentioned hardware platforms, we invite you to document your process on our forum. Additionally, you're welcome to transition your system to one of the officially supported configurations.
