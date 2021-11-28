---
title: UART
category: Stock Firmware (OFW)
order: 2
---

The only UART connected on the S30 is the UART1 (ttyS1). In order to get the uart to produce any output you need to change the U-boot configuration to redirect the output to that UART. In order to do so you need to change the following:
- Modify the DTS to replace any uart0 mentions to uart1, also change the pins to uart1_pins_a
- Modify the u-boot config:
  - CONFIG_CONS_INDEX = 2 (switches to UART1)
  - CONFIG_MMC0_CD_PIN="" (enables booting via SDcard)

You can find an initial version of the PocketGo-S30 DTS [here](https://github.com/PocketGo-S30-CFW/u-boot/blob/pocketgo-s30/arch/arm/dts/sun8i-a33-pocketgo-s30.dts).
