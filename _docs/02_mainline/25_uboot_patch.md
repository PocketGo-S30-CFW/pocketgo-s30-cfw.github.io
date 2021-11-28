---
title: U-Boot Patch
category: Mainline U-Boot and Kernel
order: 5
---

Mainline U-Boot patch

```diff
diff --git a/arch/arm/dts/Makefile b/arch/arm/dts/Makefile
index cc34da7bd8..cccb4e18e7 100644
--- a/arch/arm/dts/Makefile
+++ b/arch/arm/dts/Makefile
@@ -598,6 +598,7 @@ dtb-$(CONFIG_MACH_SUN8I_A33) += \
        sun8i-a33-olinuxino.dtb \
        sun8i-a33-q8-tablet.dtb \
        sun8i-a33-sinlinx-sina33.dtb \
+       sun8i-a33-pocketgo-s30.dtb \
        sun8i-r16-bananapi-m2m.dtb \
        sun8i-r16-nintendo-nes-classic-edition.dtb \
        sun8i-r16-parrot.dtb
diff --git a/arch/arm/dts/sun8i-a33-olinuxino.dts b/arch/arm/dts/sun8i-a33-olinuxino.dts
index a1a1eb64ca..7ec00a5bea 100644
--- a/arch/arm/dts/sun8i-a33-olinuxino.dts
+++ b/arch/arm/dts/sun8i-a33-olinuxino.dts
@@ -52,7 +52,7 @@
        compatible = "olimex,a33-olinuxino","allwinner,sun8i-a33";

        aliases {
-               serial0 = &uart0;
+               serial0 = &uart1;
        };

        chosen {
@@ -205,9 +205,9 @@
        status = "okay";
 };

-&uart0 {
+&uart1 {
        pinctrl-names = "default";
-       pinctrl-0 = <&uart0_pins_b>;
+       pinctrl-0 = <&uart1_pins_a>;
        status = "okay";
 };

diff --git a/configs/Sinlinx_SinA33_defconfig b/configs/Sinlinx_SinA33_defconfig
index 4eb5300b04..58d80a7f6f 100644
--- a/configs/Sinlinx_SinA33_defconfig
+++ b/configs/Sinlinx_SinA33_defconfig
@@ -5,8 +5,9 @@ CONFIG_SPL=y
 CONFIG_MACH_SUN8I_A33=y
 CONFIG_DRAM_CLK=552
 CONFIG_DRAM_ZQ=15291
-CONFIG_MMC0_CD_PIN="PB4"
+CONFIG_MMC0_CD_PIN=""
 CONFIG_MMC_SUNXI_SLOT_EXTRA=2
+CONFIG_SPL_SPI_SUNXI=y
 CONFIG_USB0_ID_DET="PH8"
 CONFIG_VIDEO_LCD_MODE="x:1024,y:600,depth:18,pclk_khz:66000,le:90,ri:160,up:3,lo:127,hs:70,vs:20,sync:3,vmode:0"
 CONFIG_VIDEO_LCD_DCLK_PHASE=0
@@ -20,3 +21,4 @@ CONFIG_USB_EHCI_HCD=y
 CONFIG_USB_OHCI_HCD=y
 CONFIG_USB_MUSB_GADGET=y
 CONFIG_USB_FUNCTION_MASS_STORAGE=y
+CONFIG_CONS_INDEX=2
```
