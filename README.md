Advantech RK3399 bsp Repo Manifest README
This repo is used to download manifests for Advantech RK3399 bsp releases.
## **Supported boards**
RK3399 Series
## Host PC Requirements
To use this manifest repo, the 'repo' tool must be installed first.
```
$ mkdir ~/bin
$ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo  > ~/bin/repo
$ chmod a+x ~/bin/repo
$ PATH=${PATH}:~/bin
```
To execute
```
$: mkdir <release>
$: cd <release>
$: repo init -u https://github.com/edgehook/rk-linux-manifests.git -b <branch name> [ -m <release manifest>]
$: repo sync
```
Examples:To download the RK3399 itb201 release.
```
$repo init -u https://github.com/edgehook/rk-linux-manifests.git  -b master -m adv-rk3399-itb201a1-1.0.0.xml
$repo sync
```
## Build Method
### Build u-boot
```
$:linux_sdk_510/BSP# ls
app     buildroot  debian  device.tar.bz2  external          kernel    mk-debian.sh   mk-ubuntu.sh  prebuilts.tar.bz2  rkbin    tools   ubuntu
br.log  build.sh   device  envsetup.sh     external.tar.bz2  Makefile  mkfirmware.sh  prebuilts     recovery.img       rockdev  u-boot
$:linux_sdk_510/BSP# ./build.sh uboot
```
The generated file is located in the U-boot directory，include rk3399_loader_v1.27.126.bin、trust.img and uboot.img

### Build kernel

```
$:linux_sdk_510/BSP# ./build.sh kernel
```
The generated file is located in the kernel directory，is boot.img

### Build recovery

```
$:linux_sdk_510/BSP# rm -rf buildroot/output/rockchip_rk3399_recovery/
$:linux_sdk_510/BSP# source envsetup.sh rockchip_rk3399_recovery
$:linux_sdk_510/BSP# ./build.sh recovery
```
The generated file is located in the buildroot/output/rockchip_rk3399_recovery/images directory，is recovery.img

### Build Debian rootfs

Use the linaro-bullseye-alip-whole-tar.gz file in the Debian directory to compile the file as follows
```
$:linux_sdk_510/BSP# rm -rf buildroot/output/rockchip_rk3399/
$:linux_sdk_510/BSP# source envsetup.sh rockchip_rk3399
$:linux_sdk_510/BSP# ./mk-debian.sh
```
Do not use the linaro-bullseye-alip-whole-tar. gz file in the debian directory to compile the file as follows
```
$:linux_sdk_510/BSP# rm -rf buildroot/output/rockchip_rk3399/
$:linux_sdk_510/BSP# source envsetup.sh rockchip_rk3399
$:linux_sdk_510/BSP# ./mk-debian.sh new`
```
Whichever way the above is compiled, the resulting file is located in the debian directory, is linaro-rootfs.img

### Build Ubuntu rootfs

Use the ubuntu20.04-whole.tar.gz file in the ubuntu directory to compile the file as follows
```
$:linux_sdk_510/BSP# rm -rf buildroot/output/rockchip_rk3399/
$:linux_sdk_510/BSP# ./mk-ubuntu.sh
```
Do not use the ubuntu20.04-whole.tar.gz file in the ubuntu directory to compile the file as follows
```
$:linux_sdk_510/BSP# rm -rf buildroot/output/rockchip_rk3399/
$:linux_sdk_510/BSP# ./mk-ubuntu.sh new
```
Whichever way the above is compiled, the resulting file is located in the ubuntu directory, is ubuntu-focal.img

## Push all image to rockdev folder

```
$:linux_sdk_510/BSP# ./mkfirmware.sh
```
All image in rockdev/ ./mkfirmware.sh at previous step will repack boot.img and rootfs.img, and copy other related image files to the rockdev/ directory. The common image files are listed below: 

```
board.img
boot.img -> /home/ning/workspace/rk3399/linux_sdk_510/BSP/kernel/boot.img
MiniLoaderAll.bin -> ../u-boot/rk3399_loader_v1.27.126.bin
misc.img -> ../device/rockchip/rockimg/wipe_all-misc.img
oem.img
parameter.txt -> ../device/rockchip/rk3399/parameter.txt
recovery.img -> ../buildroot/output/rockchip_rk3399_recovery/images/recovery.img
rootfs.img -> ../ubuntu/ubuntu-focal.img
trust.img -> ../u-boot/trust.img
uboot.img -> ../u-boot/uboot.img
update.img
userdata.img
```
## Make update.img

```
$:linux_sdk_510/BSP# ./build.sh updateimg
```
The generated file is located in the rockdev directory.