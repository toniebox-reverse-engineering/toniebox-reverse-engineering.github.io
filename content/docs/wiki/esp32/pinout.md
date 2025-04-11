---
title: "Pinout"
description: ""
---
| Pin ID  | Pin Name | Toniebox usage | Target | Notes |
| -- | -- | -- | -- | -- |
| 01 | LNA_IN | | | |
| 02 | VDD3P3 | | | |
| 03 | VDD3P3 | | | |
| 04 | CHIP_PU | | | |
| 05 | GPIO0 | Boot | | J100 |
| 06 | GPIO1 | SS | TRF7962A | |
| 07 | GPIO2 | MOSI | TRF7962A | |
| 08 | GPIO3 | MISO | TRF7962A | |
| 09 | GPIO4 | SCLK | TRF7962A |
| 10 | GPIO5 | I2C_SDA | LIS + DAC |
| 11 | GPIO6 | I2C_SCL | LIS + DAC | |
| 12 | GPIO7 | button pressed or charger present |  | wake up (1=no button/no charger) |
| 13 | GPIO8 | ADC_charg | R75/R72, 100kΩ/100kΩ divider (div 2) U300 LM3485, 5V buck, charger | |
| 14 | GPIO9 | ADC_Vbatt | R57/R58, 100kΩ/33kΩ divider (div 4) right before U320 "BW93" boost? | |
| -- | -- | -- | -- | -- |
| 15 | GPIO10 | DIN | DAC3100 | |
| 16 | GPIO11 | BCLK | DAC3100 | |
| 17 | GPIO12 | WCLK | DAC3100 | |
| 18 | GPIO13 | IRQ | TRF7962A | |
| 19 | GPIO14 | IRQ | LIS3DH | |
| 20 | VDD3P3_RTC | | | |
| 21 | XTAL_32K_P | | | |
| 22 | XTAL_32K_N | | | |
| 23 | GPIO17 | Blue-LED | LED | |
| 24 | GPIO18 | Green-LED | LED | |
| 25 | GPIO19 | Red-LED | LED | |
| 26 | GPIO20 | Ear left, big | | active low |
| 27 | GPIO21 | Ear right, small | | active low |
| 28 | GPIO26 / SPICS1 | RESET (active high) | DAC3100 | |
| -- | -- | -- | -- | -- |
| 29 | VDD_SPI | | | |
| 30 | SPIHD | RS | SPI flash | via 100Ω |
| 31 | SPIWP | WP | SPI flash | via 100Ω |
| 32 | SPICS0 | SCS | SPI flash | via 22Ω |
| 33 | SPIHCLK | SCK | SPI flash | via 22Ω |
| 34 | SPIQ | SO | SPI flash | via 22Ω |
| 35 | SPID | SI | SPI flash | via 22Ω |
| 36 | GPIO48 / SPICLK_N | GPIO1 | DAC3100 | |
| 37 | GPIO47 / SPICLK_P | Power | SD/TRF7962A | Low = Power on |
| 38 | GPIO33 | DAT2 | SD | |
| 39 | GPIO34 | DAT3 | SD | |
| 40 | GPIO35 | CLK | SD | |
| 41 | GPIO36 | DAT0 | SD | |
| 42 | GPIO37 | DAT1 | SD | |
| -- | -- | -- | -- | -- |
| 43 | GPIO38 | CMD | SD | J102 |
| 44 | MTCK | TCK | JTAG | J102 |
| 45 | MTDO | TD0 | JTAG | J102 |
| 46 | VDD3P3_CPU | | | |
| 47 | MTDI | TDI | JTAG | J102 |
| 48 | MTMS | TMS | JTAG | J102 |
| 49 | U0TXD | TX | UART | J103 |
| 50 | U0RXD | RX | UART | J103 |
| 51 | GPIO45 | Power | LIS + DAC + Blue-LED | High = Power on |
| 52 | GPIO46 | | | J101 |
| 53 | XTAL_N | | | |
| 54 | XTAL_P | | | |
| 55 | VDDA | | | |
| 56 | VDDA | | | |
