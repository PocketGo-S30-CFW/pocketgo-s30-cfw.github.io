---
title: Kernel
category: Mainline U-Boot and Kernel
order: 3
---

Clone the branch `pocketgo-s30` of our fork of the [linux repo](https://github.com/PocketGo-S30-CFW/linux/tree/pocketgo-s30): 
```console
$ git clone -b pocketgo-s30 https://github.com/PocketGo-S30-CFW/linux.git
$ cd u-boot
$ make sunxi_defconfig
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j17
```
Note: change the -j17 to your number of cores+1 (e.g. 8 cores -> -j9)
