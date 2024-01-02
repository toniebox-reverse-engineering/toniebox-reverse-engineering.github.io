---
title: "Frequently Asked Questions"
description: "You are always asking the same things. Thats why we started this page so you can find the answer yourself =)"
weight: 15
---
# FAQ
## Is there a way to find out which kind of box I have...
### ... on the outside of the packaging (by serial)?
No, the packaging provides no information about that. The serial number just indictates when (year + week) and on which production line it was produced. As there a still boxes produces with [CC3200](/docs/box-variants/cc3200/) and [CC3235](/docs/box-variants/cc3235/) chips, you may also get them, beside a [ESP32](/docs/box-variants/esp32/) based one, if you buy a recent one. Tonies even reuses old pcbs for new boxes. 
### ... on the outside of the toniebox itself (by mac)?
If the box has a MAC on its cap, you may check if its a [ESP32](/docs/box-variants/esp32/) based one or not. You may use [macvendors](https://macvendors.com/) for that. Boxes without a MAC are most likely a [CC3200](/docs/box-variants/cc3200/) based one, as the MAC wasn't printed onto the CAP at the beginning.

## What kind of box should I get?
There are three different MCUs used. The [CC3200](/docs/box-variants/cc3200/) (v1/v2), the [CC3235](/docs/box-variants/cc3235/) (v3) and the [ESP32](/docs/box-variants/esp32/) (v4).
The [CC3200](/docs/box-variants/cc3200/) based box has the custom bootloader [HackieboxNG](/docs/custom-firmware/cc3200/hackieboxng-bl/) and the custom PoC firmware [Hackiebox](/docs/custom-firmware/cc3200/hackiebox-cfw) available, but for the future we recommend the [ESP32](/docs/box-variants/esp32/) based box.