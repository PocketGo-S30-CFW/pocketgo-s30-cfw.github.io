---
title: Intro
category: Buildroot and Batocera
order: 1
---

We can generate a full SDCard distribution using buildroot or batocera.

## Buildroot

Clone the branch `pocketgo-s30` of our fork of the [buildroot repo](https:github.com/PocketGo-S30-CFW/buildroot/tree/pocketgo-s30):
```console
$ git clone -b pocketgo-s30 https://github.com/PocketGo-S30-CFW/buildroot.git
$ cd buildroot
$ make pocketgo_s30_defconfig
$ make
```

Once the build is finished, you will see something like this:

```console
# ls output/images/
boot.scr  rootfs.ext2  rootfs.ext4  rootfs.tar  sdcard.img  sun8i-a33-pocketgo-s30.dtb  u-boot.bin  u-boot-sunxi-with-spl.bin  zImage
```

The `sdcard.img` contains the full system bootable image ready to be flashed into an SDCard.

Flash the image with:
```console
$ sudo dd if=sdcard.img of=/dev/sdX bs=1024 status=progress
```

Once the card has been flashed, insert it in the PocketGo-S30, and restart. If you have the UART connected you will see the booot traces. 
