---
title: "Supported NAS/Server hardware"
description: "list of supported NAS/server hardware (incomplete)"
bookCollapseSection: true
---
# Supported NAS/Server hardware

Teddycloud is supported on nearly all servers utilizing the x86_64, arm64, and armhf (linux/arm/v7) architectures, as Docker containers are available for these platforms.
Below is a compilation of NAS/server hardware that has been successfully utilized to host TeddyCloud:

| NAS / Server hardware           | Limitations                                                                                                                                                                 |
|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Raspberry Pi 4                  | -                                                                                                                                                                           |
| Raspberry Pi Zero 2             | slow on initial startup (expect 20 minutes and more for initial certificate creation!), encoding also very slow, possible crashes in some situations, but in general usable |
| Synology NAS DS923+ (DSM 7.2.1) | -                                                                                                                                                                           |
| Raspberry Pi Zero               | Docker installation a little bit tricky, see https://forum.revvox.de/t/hardware-requirements-does-teddycloud-run-on-a-raspberry-zero-1st-gen/309                            |
| ...                             |                                                                                                                                                                             |

Consider using Alpine container instead of Ubuntu / Debian container if your server has limited resources.

If you're utilizing alternative NAS/server hardware configurations, your contribution is welcome. Feel free to share your experience and insights with the community.

## Tested Hardware, but not supporting TeddyCloud


| NAS / Server hardware | Comment/Discussion/Links |
|-----------------------|--------------------------|
| ...                   |                          |

If you've attempted to deploy TeddyCloud on specialized hardware without success, we encourage you to share your experience for community contribution. Conversely, if you've successfully implemented TeddyCloud on any of the mentioned hardware platforms, we invite you to document your process on our forum. Additionally, you're welcome to transition your system to one of the officially supported configurations.
