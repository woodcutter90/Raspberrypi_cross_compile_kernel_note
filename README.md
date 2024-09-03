# Cross-compile kernel quick start guide
--------------------------------------------

## Download source code
```
$ mkdir rpi && cd rpi
# Install git, if you already have git, please ignore it
$ sudo apt install git
# download source code
$ git clone --depth=1 https://github.com/raspberrypi/linux
```

## Install required dependencies and toolchain
```
$ sudo apt install bc bison flex libssl-dev make libc6-dev libncurses5-dev
# install 32-bits cross-compiler
$ sudo apt install crossbuild-essential-armhf
```

## For Pi 3
```
$ cd linux
$ KERNEL=kernel7
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2709_defconfig
```

## Customise the kernel version using LOCALVERSION
Modify this in .config file
```
CONFIG_LOCALVERSION="-v7l-MMY_CUSTOM"
```

## Build kernel (for 32-bits)
```
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage modules dtbs
```

## Install the kernel
```
$ mkdir mnt
$ mkdir mnt/boot
$ mkdir mnt/root
$ sudo mount /dev/sdb1 mnt/boot
$ sudo mount /dev/sdb2 mnt/root

```
## Install
```
$ sudo env PATH=$PATH make -j12 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=mnt/root modules_install
#  install the 32-bit kernel
$ sudo cp mnt/boot/$KERNEL.img mnt/boot/$KERNEL-backup.img
$ sudo cp arch/arm/boot/zImage mnt/boot/$KERNEL.img
# Depending on your kernel version, run the following command to install Device Tree blobs
# For kernels up to version 6.4:
$ sudo cp arch/arm/boot/dts/*.dtb mnt/boot/
# Finally, install the overlays and README, and unmount the partitions:
$ sudo cp arch/arm/boot/dts/overlays/*.dtb* mnt/boot/overlays/
$ sudo cp arch/arm/boot/dts/overlays/README mnt/boot/overlays/
# umount the SD card
$ sudo umount mnt/boot
$ sudo umount mnt/root

```
## Restart raspberry pi and check 
```
# in raspberry pi console
$ uname -a
```

