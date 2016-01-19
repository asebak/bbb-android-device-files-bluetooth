# bbb-android-device-files-bluetooth

Beagleboard Black Android Kernel with Bluetooth Support
Android Kernel 4.4.4 
Linux Kernel 3.14

#### Fetch Android Open Source Project (AOSP)
```
$ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo 
$ chmod a+x ~/bin/repo
$ mkdir ~/aosp
$ cd aosp
$ repo init -u https://android.googlesource.com/platform/manifest -b android-4.4.4_r1
$ repo sync
```

#### Get Android BSP for beaglebone black
```
$ cd ~/aosp/device
$ mkdir ti
$ cd ti
$ git clone https://github.com/asebak/bbb-android-device-files-bluetooth.git beagleboneblack
```

#### Setup ARM Compiler
```
$ wget http://releases.linaro.org/14.04/components/toolchain/binaries/gcc-linaro-arm-linux-gnueabihf-4.8-2014.04_linux.tar.xz
$ sudo tar xvf gcc-linaro-arm-linux-gnueabihf-4.8-2014.04_linux.tar.xz -C /opt/
$ cd /opt/
$ sudo mv gcc-linaro-arm-linux-gnueabihf-4.8-2014.04_linux/ gcc-arm-linux
$ export PATH=$PATH:/opt/gcc-arm-linux/bin
```

#### Compile U-Boot (Bootloader)

```
$ cd ~/aosp
$ git clone https://github.com/u-boot/u-boot.git u-boot
$ cd u-boot
$ make CROSS_COMPILE=arm-linux-gnueabihf- am335x_boneblack_defconfig
$ make CROSS_COMPILE=arm-linux-gnueabihf-
```

#### Compile the Linux Kernel

```
$ cd ~/aosp
$ git clone https://github.com/beagleboard/linux.git kernel
$ cd kernel
$ git checkout 3.14
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bb.org_defconfig
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j4 uImage LOADADDR=0x80008000
$ croot
```


#### Compile the Android Kernel

```
$ cd ~/aosp
$ source build/envsetup.sh
$ lunch #Select beagleboneblack-bluetooth-eng
$ make -j8
```

#### Create an SD Card

```
./sdcard.sh
```
