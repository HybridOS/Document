# 在ubuntu 15.04 x86-64上实验 android-x86-5.1.1+kernel-4.0.9
## 源码是从sf上取得
```
mkdir android-repo
cd android-repo
repo init -u git://gitscm.sf.net/gitroot/android-x86/manifest -b lollipop-x86
repo sync
```

## 开始配置
自己的配置
```
cd ..
. ./chysetenv.sh

export PATH=/backup/android-related/git-repo:$PATH
export USE_CCACHE=1
export CCACHE_DIR=/backup/android-related/ccache
/backup/android-related/android-repo/prebuilts/misc/linux-x86/ccache/ccache -M 30G

export ANDROID_PRODUCT_OUT=/backup/android-related/android-repo/out/target/product/generic
export EMULATOR_X86=/backup/android-related/android-repo/prebuilts/android-emulator/linux-x86_64
ANDROID_PRODUCT_OUT_BIN=/backup/android-related/android-repo/out/host/linux-x86/bin
export PATH=$PATH:$ANDROID_PRODUCT_OUT_BIN:$ANDROID_PRODUCT_OUT:$EMULATOR_X86
```

这是为之前AOSP准备的，现在部分内容其实没用，比如EMULATOR_X86，也许就前面4行有用

## 开始编译
```
cd android-repo
source build/envsetup.sh
lunch android-x86-eng #选择10 x86-32 系统 or 7 for x86-64系统
make -j16 iso_img
```

就完成了！ 可以直接用virtualbox使用生成的img，在不安装模式下是可以基本正常工作的。

## 定制内核(optional)

```
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 android-x86_defconfig #选择缺省android-x86配置
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 android-x86_defconfig
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 menuconfig #可以自己做些定制
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 -j16  #编译成功内核
```

注意：
```
DO NOT make in kernel/ directly. If you do so, try
$ make -C kernel distclean
$ rm -rf $OUT/obj/kernel 
```

## 用qemu-system-i386测试
```
1 qemu-system-i386 -m 2G -cdrom /backup/android-related/android-repo/out/target/product/x86/android_x86.iso -serial stdio >/backup/temp/logq

2 qemu-system-i386 -kernel kernel -cdrom android_x86.iso  -append "initrd=/initrd.img root=/dev/ram0  console=ttyS0 androidboot.hardware=android_x86 DEBUG=2 SRC= DATA=" -no-reboot  -serial stdio
kernel是编译出来的内核

3 qemu-system-i386 -kernel kernel -cdrom android_x86.iso -initrd  /home/chyyuu/develop/xly_qemu_debug_linux/rootfs_32.img.gz -append "root=/dev/ram rdinit=/sbin/init console=ttyS0" -no-reboot  -serial stdio
```

对kernel-4.3的测试，在qemu中，没有看到QEMU-CDROM, 所以没法找到cdrom, 之后参考
https://wiki.gentoo.org/wiki/CDROM
https://bbs.archlinux.org/viewtopic.php?id=99127
https://www.suse.com/documentation/sles-12/book_virt/data/cha_qemu_running_devices.html

增加了config中的选项，具体可以参考4.3的android-x86_defconfig.chy1和chy2的区别。
然后，就可以识别到QEMU CDROM了，就可以采用1的方式到shell了！


生成的iso的grub.cfg（真实名字是isolinux.cfg）的内容
```
default vesamenu.c32
timeout 600

menu background android-x86.png
menu title Android-x86 Live & Installation CD 5.1-rc1
menu color border 0 #ffffffff #00000000
menu color sel 7 #ffffff00 #ff000000
menu color title 0 #ffffffff #00000000
menu color tabmsg 0 #ffffffff #00000000
menu color unsel 0 #ffffffff #00000000
menu color hotsel 0 #ffffff00 #ff000000
menu color hotkey 7 #ffffff00 #00000000

label livem
	menu label Live CD - ^Run Android-x86 without installation
	kernel /kernel
	append initrd=/initrd.img root=/dev/ram0 androidboot.hardware=android_x86 quiet SRC= DATA=

label vesa
	menu label Live CD - ^VESA mode
	kernel /kernel
	append initrd=/initrd.img root=/dev/ram0 androidboot.hardware=android_x86 quiet nomodeset vga=788 SRC= DATA=

label debug
	menu label Live CD - ^Debug mode
	kernel /kernel
	append initrd=/initrd.img root=/dev/ram0 androidboot.hardware=android_x86 vga=788 DEBUG=2 SRC= DATA=

label install
	menu label Installation - ^Install Android-x86 to harddisk
	kernel /kernel
	append initrd=/initrd.img root=/dev/ram0 androidboot.hardware=android_x86 INSTALL=1 DEBUG=
```

## 用qemu-system-x86_64测试

### UEFI 模式
```
qemu-system-x86_64 -enable-kvm -m 4G -bios OVMF.fd -hdb android_x86_64.img -vga std -serial stdio
```

### BIOS 模式
```
qemu-system-x86_64 -enable-kvm -m 4G -cdrom android_x86_64.iso -vga std -serial stdio
```

### VESA mode
```
qemu-system-x86_64 -enable-kvm -m 4G -bios OVMF.fd -hdb android_x86_64.img -vga std -serial stdio
```

### kernel 参数 
```
  nomodeset vga=788   即800x600:16bpp
  console=ttyS0  用串口交互
```