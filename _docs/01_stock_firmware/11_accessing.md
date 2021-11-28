---
title: Accessing the console with the original firmware
category: Stock Firmware (OFW)
order: 1
---

The OFW has adb daemon enabled, so you can use it to open a terminal session on the console. From an ubuntu linux pc:

```console
$ sudo udevadm trigger
$ adb shell
* daemon not running; starting now at tcp:5037
* daemon started successfully
zkswe@flythings:/ # ls
bin       dev       init      mnt       sbin      system    userdata
data      etc       lib       proc      sys       tmp       usr
zkswe@flythings:/ #
```

You can find a full output of the dmesg output at [the bottom of this page](#OFW-dmesg-output)

The original kernel:
```console
uname -a
Linux localhost 3.4.39 #154 SMP PREEMPT Mon Jan 25 07:18:08 UTC 2021 armv7l GNU/Linux
```

Partitions:
```console
zkswe@flythings # cat /proc/partitions                         
major minor  #blocks  name

  31        0        384 mtdblock0
  31        1        128 mtdblock1
  31        2       7296 mtdblock2
  31        3        128 mtdblock3
  31        4        256 mtdblock4
 179        0   30910464 mmcblk0 <- SDCARD device
 179        1   30909440 mmcblk0p1 <- SDCard partition 1
 ```
The partitions can also be seen via dmesg:
```console
[    0.409669] m25p_probe()1167 - Use the Dual Mode Read.
[    0.409826] m25p80 spi0.0: found zb25q64, expected w25q128
[    0.409841] m25p80 spi0.0: zb25q64 (8192 Kbytes)
[    0.410680] Creating 6 MTD partitions on "spi0.0":
[    0.410699] 0x000000000000-0x000000060000 : "uboot"
[    0.411552] 0x000000060000-0x000000080000 : "env"
[    0.412317] 0x000000080000-0x0000007a0000 : "boot"
[    0.412996] 0x0000007a0000-0x0000007c0000 : "res"
[    0.413661] 0x0000007c0000-0x000000800000 : "boot_logo"
[    0.414339] 0x000000800000-0x000000800000 : "UDISK"
[    0.414350] mtd: partition "UDISK" is out of reach -- disabled
```
