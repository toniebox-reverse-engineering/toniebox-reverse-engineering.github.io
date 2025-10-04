---
title: "Hardware"
description: ""
---

# Pictures (r1)
## Main PCB
[![Top view](TB-MAIN_2.0.P_Top_small.png)](TB-MAIN_2.0.P_Top_small.png)
[![Bottom view](TB-MAIN_2.0.P_Bottom_small.png)](TB-MAIN_2.0.P_Bottom_small.png)

## UX PCB
[![Top](TB-UX-PCB_1.0.R_Top_small.png)](TB-UX-PCB_1.0.R_Top_small.png)
[![Bottom](TB-UX-PCB_1.0.R_Bottom_small.png)](TB-UX-PCB_1.0.R_Bottom_small.png)



# Testpoints

| Testpoint | Function |
| :-- | :-- |
| **TP510** | Serial TX |
| **TP511** | Serial RX? (no response) |
| **TP512** | Serial GND |

# Table

| Chip Designation | Manufacturer | Function (Assumed) | Datasheet (PDF) |
| :-- | :-- | :-- | :-- |
| **TAS2X63 TI 458 AXGJ G3** | Texas Instruments | Audio amplifier (likely TAS2563) | [TI TAS2563 PDF][1] |
| **IW418HNA1C** | NXP | Dual-band WiFi 4 + Bluetooth 5.2 | [NXP IW416 PDF][2] |
| **823-A0** | NXP | PMIC – Dual 1.5A Buck + 525 mA LDO | [NXP PM823 PDF][3] |
| **MIMXRT1061 DVL6B** | NXP | ARM Cortex-M7 MCU (RT1060 family) | [NXP MIMXRT1061 PDF][4] |
| **W956A8MBYA5I** | Winbond | 64Mbit HyperRAM 200 MHz 35 ns | [Winbond W956A8MBYA5I PDF][5] |
| **RS70B32G4S15G** | Rayson | 32GB eMMC 5.1 TFBGA-153 | [Rayson RS70B32G4S15G PDF][6] |
| **W25Q128JVSIQ** | Winbond | 128Mbit SPI NOR Flash SOIC-8 | [Winbond W25Q128JVSIQ PDF][7] |

# Notes & Corrections

- **TAS2X63** marking strongly suggests TI **TAS2563**, used for Class-D audio amplification.
- **823-A0** refers to NXP’s PM823, a power management IC with integrated buck converters and LDO.
- **W956A8MBYA5I** is a **64Mbit HyperRAM** with HyperBus interface.
- **RS70B32G4S15G** is a 32GB eMMC 5.1 module.
- **W25Q128JYSQ** marking corresponds to **W25Q128JVSIQ**, a 128Mbit SPI NOR Flash IC.

[1]: https://www.ti.com/product/TAS2563
[2]: https://www.nxp.com/products/IW416
[3]: https://www.mouser.de/ProductDetail/NXP-Semiconductors/PM823UK-A0CZ?qs=TuK3vfAjtkVJFmQ7TAMyAw%3D%3D
[4]: https://www.nxp.com/docs/en/data-sheet/IMXRT1060CEC.pdf
[5]: https://www.digikey.de/de/products/detail/winbond-electronics/W956A8MBYA5I/15181915
[6]: https://www.lcsc.com/product-detail/C22375657.html?s_z=n_RS70B32G4
[7]: https://www.lcsc.com/product-detail/C97521.html?s_z=n_25Q128JV
