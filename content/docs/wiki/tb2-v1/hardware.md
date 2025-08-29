---
title: "Hardware"
description: ""
---
# Pictures (r1)
## Board


# Parts

## TAS2X63 TI 458 AXGJ G3
## NXP IW416HNA1C KJ1GC.01 JEAD2509
## NXP 823-A0 452n02
## MIMXRT1061 DVL6B 1N00X CTQG2502B
## Winbond W956A8MBYA5I 6331A5100ZV2 510PCA TWN
## Rayson RS70B32G4 S15G F3010B AF14
## Winbond 250128JYSQ 2518

# Table

| Chip Designation | Manufacturer | Function (Assumed) | Datasheet (PDF) |
| :-- | :-- | :-- | :-- |
| **TAS2X63 TI 458 AXGJ G3** | Texas Instruments | Audio amplifier (TAS2x63 series, very likely TAS2563) | [TI TAS2563 PDF] |
| **IW416HNA1C KJ1GC.01 JEAD2509** | NXP | WiFi/Bluetooth transceiver (IW416 series) | [NXP IW416 PDF] |
| **823-A0 452n02 (NXP)** | NXP | Unclear, possibly PMIC or sub-chip, no clear assignment | No public datasheet |
| **MIMXRT1061 DVL6B 1N00X CTQG2502B** | NXP | ARM Cortex-M7 MCU (RT1060 family) | [NXP MIMXRT1061 PDF] |
| **Winbond W956A8MBYA5I 6331A5100ZV2 510PCA TWN** | Winbond | 512Mb DDR3 SDRAM | [Winbond W956A8MBYA5I PDF] |
| **Rayson RS70B32G4 S15G F3010B AF14** | Rayson | High-speed relay/IC (barely public, possibly special chip) | No public datasheet |
| **Winbond 250128JYSQ 2518** | Winbond | 128Mb SPI NOR Flash | [Winbond W25Q128JVSQ PDF] |

## Notes \& Corrections

- For the **823-A0 452n02** chip from NXP, it is most likely a custom part or a sub-module with no public datasheet available.
- **Rayson RS70B32G4** is very likely a relay or interface chip from Rayson, but no public datasheet can be found.
- All other chips are in the correct manufacturer families and the part numbers match the visible markings on the board.
- The Winbond number **250128JYSQ** should correctly read as **W25Q128JVSQ**, which is the common Winbond 128Mb SPI Flash IC.
- For the TI audio chip, **TAS2563** is very likely the correct assignment, as the ending numbers/appearance match. Datasheets for TAS2x63 and TAS2563 are relevant.

1. **TI TAS2563**: https://www.ti.com/product/TAS2563[^1]
2. **NXP IW416**: https://www.nxp.com/docs/en/data-sheet/IW416.pdf[^2]
3. **NXP MIMXRT1061**: https://www.nxp.com/docs/en/data-sheet/IMXRT1060CEC.pdf[^3]
4. **Winbond W956A8MBYA5I**: https://www.winbond.com/resource-files/w956a8mbya5i.pdf[^4]
5. **Winbond W25Q128JVSQ**: https://www.winbond.com/resource-files/w25q128jv spi revc 02122019.pdf
