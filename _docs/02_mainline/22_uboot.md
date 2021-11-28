---
title: Mainline U-BOOT
category: Mainline U-Boot and Kernel
order: 2
---

Clone the branch `pocketgo-s30` of our [u-boot repo](https://github.com/PocketGo-S30-CFW/u-boot/tree/pocketgo-s30): 
```console
$ git clone -b pocketgo-s30 https://github.com/PocketGo-S30-CFW/u-boot.git
$ cd u-boot
$ make PocketGo_s30_a33_defconfig
$ make CROSS_COMPILE=arm-linux-gnueabihf- -j17
```
Note: change the -j17 to your number of cores+1 (e.g. 8 cores -> -j9)

The build should generate a ``u-boot-sunxi-with-spl.bin`` u-boot file that can be used via USB with sunxi-fel or flashed to a SDCard

Once u-boot is compiled, you can either boot via tethered USB or flashed SD card.

### USB (sunxi-fel) BOOT

Sunxi-fel is part of the sunxi-tools package, you can install it by checking out their repo: ``https://github.com/linux-sunxi/sunxi-tools``.

After ``sunxi-fel`` is installed you need to put the console in FEL mode. The Pocket-Go S30 does not have a dedicated recovery/reset button, so you can use the [fel-sdboot.sunxi](https://github.com/linux-sunxi/sunxi-tools/blob/master/bin/fel-sdboot.sunxi) image and flash it to a SDcard with:

```console
$ get https://github.com/linux-sunxi/sunxi-tools/raw/master/bin/fel-sdboot.sunxi
$ sudo dd if=fel-sdboot.sunxi of=/dev/sdX bs=1024 seek=8
```
Note: replace sdX with your sdcard device (you can find it with fdisk -l)

Power down the Pocket-Go S30, insert the new SDCARD that you flashed with ``fel-sdboot.sunxi`` in it, and connect your UART to your compture.

Then you can boot the console via USB:

```console
sunxi-fel -v -p uboot u-boot-sunxi-with-spl.bin
```

If you have UART connected, you will see the following output:

```console
U-Boot SPL 2022.01-rc2-dirty (Nov 23 2021 - 16:07:52 +0000)
DRAM: 512 MiB
Trying to boot from FEL


U-Boot 2022.01-rc2-dirty (Nov 23 2021 - 16:07:52 +0000) Allwinner Technology

CPU:   Allwinner A33 (SUN8I 1667)
Model: PocketGo S30
DRAM:  512 MiB
WDT:   Not starting watchdog@1c20ca0
MMC:   mmc@1c0f000: 0, mmc@1c11000: 1
Loading Environment from FAT... Unable to use mmc 0:2... Setting up a 320x480 lcd console (overscan 0x0)
In:    serial
Out:   vidconsole
Err:   vidconsole
Allwinner mUSB OTG (Peripheral)
Net:   eth0: usb_ether
starting USB...
Bus usb@1c1a000: USB EHCI 1.00
Bus usb@1c1a400: USB OHCI 1.0
scanning bus usb@1c1a000 for devices... 1 USB Device(s) found
scanning bus usb@1c1a400 for devices... 1 USB Device(s) found
       scanning usb for storage devices... 0 Storage Device(s) found
Hit any key to stop autoboot:  0
```

### SDCARD BOOT

Another option is to flash the compiled u-boot (``u-boot-sunxi-with-spl.bin``) to the sdcard with:

```console
sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdX bs=1024 seek=8
```
Note: replace sdX with your sdcard device (you can find it with fdisk -l)

Power down the Pocket-Go S30, insert the new SDCARD that you flashed with ``u-boot-sunxi-with-spl.bin``. Connect your UART and turn on your console. You will see something like:

<details>
  <summary>PocketGo S30 CFW Boot - Click to expand!</summary>

```console
U-Boot SPL 2022.01-rc2-00024-g3144ba23bf-dirty (Nov 20 2021 - 04:03:00 +0000)
DRAM: 512 MiB
Trying to boot from MMC1


U-Boot 2022.01-rc2-00024-g3144ba23bf-dirty (Nov 20 2021 - 04:03:00 +0000) Allwinner Technology

CPU:   Allwinner A33 (SUN8I 1667)
Model: PocketGo S30
DRAM:  512 MiB
WDT:   Not starting watchdog@1c20ca0
MMC:   mmc@1c0f000: 0, mmc@1c11000: 1
Loading Environment from FAT... *** Warning - bad CRC, using default environment

Setting up a 1024x600 lcd console (overscan 0x0)
In:    serial
Out:   vidconsole
Err:   vidconsole
Allwinner mUSB OTG (Peripheral)
Net:   eth0: usb_ether
starting USB...
Bus usb@1c1a000: USB EHCI 1.00
Bus usb@1c1a400: USB OHCI 1.0
scanning bus usb@1c1a000 for devices... 1 USB Device(s) found
scanning bus usb@1c1a400 for devices... 1 USB Device(s) found
       scanning usb for storage devices... 0 Storage Device(s) found
Hit any key to stop autoboot:  0
=> setenv bootargs console=ttyS0,115200 earlyprintk=serial,ttyS1,115200 debug loglevel=7 rootwait root=/dev/mmcblk0p1
=> load mmc 0 0x43000000 boot/sun8i-a33-olinuxino.dtb
21702 bytes read in 3 ms (6.9 MiB/s)
=> load mmc 0 0x42000000 boot/zImage                      
4087568 bytes read in 172 ms (22.7 MiB/s)
=> bootz 0x42000000 - 0x43000000
Kernel image @ 0x42000000 [ 0x000000 - 0x3e5f10 ]
## Flattened Device Tree blob at 43000000
   Booting using the fdt blob at 0x43000000
   Using Device Tree in place at 43000000, end 430084c5


Starting kernel ...

[    0.000000] Booting Linux on physical CPU 0x0
[    0.000000] Linux version 5.0.0 (acmeplus@endymion) (gcc version 6.3.1 20170404 (Linaro GCC 6.3-2017.05)) #3 SMP Fri Nov 26 1
[    0.000000] CPU: ARMv7 Processor [410fc075] revision 5 (ARMv7), cr=10c5387d
[    0.000000] CPU: div instructions available: patching division code
[    0.000000] CPU: PIPT / VIPT nonaliasing data cache, VIPT aliasing instruction cache
[    0.000000] OF: fdt: Machine model: Olimex A33-OLinuXino
[    0.000000] printk: bootconsole [earlycon0] enabled
[    0.000000] Memory policy: Data cache writealloc
[    0.000000] cma: Reserved 16 MiB at 0x5d000000
[    0.000000] psci: probing for conduit method from DT.
[    0.982066] hub 1-0:1.0: 1 port detected
[    0.986741] ohci-platform 1c1a400.usb: Generic Platform OHCI controller
[    0.993386] ohci-platform 1c1a400.usb: new USB bus registered, assigned bus number 2
[    1.001344] ohci-platform 1c1a400.usb: irq 27, io mem 0x01c1a400
[    1.076061] hub 2-0:1.0: USB hub found
[    1.079842] hub 2-0:1.0: 1 port detected
[    1.084569] usb_phy_generic usb_phy_generic.0.auto: usb_phy_generic.0.auto supply vcc not found, using dummy regulator
[    1.095330] usb_phy_generic usb_phy_generic.0.auto: Linked as a consumer to regulator.0
[    1.103660] musb-hdrc musb-hdrc.1.auto: MUSB HDRC host driver
[    1.109404] musb-hdrc musb-hdrc.1.auto: new USB bus registered, assigned bus number 3
[    1.118413] hub 3-0:1.0: USB hub found
[    1.122213] hub 3-0:1.0: 1 port detected
[    1.127172] sun8i-a33-pinctrl 1c20800.pinctrl: 1c20800.pinctrl supply vcc-pf not found, using dummy regulator
[    1.137291] sunxi-mmc 1c0f000.mmc: Linked as a consumer to regulator.1
[    1.144328] sunxi-mmc 1c0f000.mmc: Got CD GPIO
[    1.174253] sunxi-mmc 1c0f000.mmc: initialized, max. request size: 16384 KB
[    1.181508] simple-framebuffer 5e000000.framebuffer: Linked as a consumer to regulator.6
[    1.189675] simple-framebuffer 5e000000.framebuffer: framebuffer at 0x5e000000, 0x96000 bytes, mapped to 0x(ptrval)
[    1.200136] simple-framebuffer 5e000000.framebuffer: format=x8r8g8b8, mode=320x480x32, linelength=1280
[    1.212411] Console: switching to colour frame buffer device 40x30
[    1.221094] simple-framebuffer 5e000000.framebuffer: fb0: simplefb registered!
[    1.228459] sun6i-rtc 1f00000.rtc: setting system clock to 1970-01-01T00:00:14 UTC (14)
[    1.236793] ALSA device list:
[    1.239759]   No soundcards found.
[    1.244068] Waiting for root device /dev/mmcblk0p1...
[    1.278480] mmc0: host does not support reading read-only switch, assuming write-enable
[    1.288323] mmc0: new high speed SDXC card at address 59b4
[    1.295033] mmcblk0: mmc0:59b4       58.2 GiB
[    1.301732]  mmcblk0: p1
[    1.336164] EXT4-fs (mmcblk0p1): mounted filesystem with ordered data mode. Opts: (null)
[    1.344364] VFS: Mounted root (ext4 filesystem) readonly on device 179:1.
[    1.351730] devtmpfs: mounted
[    1.355798] Freeing unused kernel memory: 1024K
[    1.381592] Run /sbin/init as init process
[    1.455036] random: fast init done
[    1.458519] EXT4-fs (mmcblk0p1): re-mounted. Opts: (null)
Starting syslogd: OK
Starting klogd: OK
Running sysctl: OK
Initializing random number generator: OK
Saving random seed: [    1.566874] random: dd: uninitialized urandom read (512 bytes read)
OK
Starting network: OK

Welcome to A33 OLinuXino!
A33-olinuxino login: root
```
       
</details>
