# bbb-android-device-files-bluetooth

Beagleboard Black Android Kernel with Bluetooth Support
Android 6 Kernel
Linux Kernel 3.14

### Get AOSP (Android 6 Kernel)
```
$ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo 
$ chmod a+x ~/bin/repo
$ mkdir ~/aosp
$ cd aosp
$ repo init -u https://android.googlesource.com/platform/manifest -b android-6.0.0_r7
$ repo sync
```

### Get android BSP for beaglebone black
```
$ cd ~/aosp/device
$ mkdir ti
$ cd ti
$ git clone https://github.com/asebak/bbb-android-device-files-bluetooth.git beagleboneblack
```

### Configure Build for AOSP
```
$ cd ~/aosp
$ . build/envsetup.sh
$ lunch
```

Select "beagleboneblack-bluetooth-eng"

### Setup ARM Compiler
```
$ wget http://releases.linaro.org/14.04/components/toolchain/binaries/gcc-linaro-arm-linux-gnueabihf-4.8-2014.04_linux.tar.xz
$ sudo tar xvf gcc-linaro-arm-linux-gnueabihf-4.8-2014.04_linux.tar.xz -C /opt/
$ cd /opt/
$ sudo mv gcc-linaro-arm-linux-gnueabihf-4.8-2014.04_linux/ gcc-arm-linux
$ export PATH=$PATH:/opt/gcc-arm-linux/bin
```
### Build the Linux Kernel
```
$ cd ~/aosp
$ git clone https://github.com/beagleboard/linux.git kernel
$ cd kernel
$ git checkout 3.14
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bb.org_defconfig
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j4 uImage
$ croot
```
### Build U-Boot
```
$ cd ~/aosp
$ git clone https://github.com/csimmonds/u-boot
$ cd u-boot
$ git checkout am335x-v2013.01.01
$ make CROSS_COMPILE=arm-eabi- am335x_evm_config
$ make CROSS_COMPILE=arm-eabi- 
```
### Build the Android Kernel
```
$ cd ~/aosp
$ make -j8
```

### Create an SD Card
```
.~/aosp/device/ti/beagleboneblack/write_sdcard.sh
```
