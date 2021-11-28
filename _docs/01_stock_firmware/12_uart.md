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

As an example, using the sun8i-a33-sinlinx-sina33.dts definition from u-boot (see below for compilation), you can apply this patch:

```diff
diff --git a/arch/arm/dts/sun8i-a33-sinlinx-sina33.dts b/arch/arm/dts/sun8i-a33-sinlinx-sina33.dts
index 541acb4d2b..f019fa2a61 100644
--- a/arch/arm/dts/sun8i-a33-sinlinx-sina33.dts
+++ b/arch/arm/dts/sun8i-a33-sinlinx-sina33.dts
@@ -54,7 +54,7 @@
        compatible = "sinlinx,sina33", "allwinner,sun8i-a33";

        aliases {
-               serial0 = &uart0;
+               serial0 = &uart1;
        };

        chosen {
@@ -276,9 +276,9 @@
        };
 };

-&uart0 {
+&uart1 {
        pinctrl-names = "default";
-       pinctrl-0 = <&uart0_pins_b>;
+       pinctrl-0 = <&uart1_pins_a>;
        status = "okay";
 };
```
