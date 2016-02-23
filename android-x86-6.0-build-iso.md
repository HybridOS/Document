# 在ubuntu 15.10 x86-64上实验 android-x86-6.0.1+kernel-4.0.9

## 从sf上取得源码
```bash
mkdir android-repo
cd android-repo
repo init -u git://gitscm.sf.net/gitroot/android-x86/manifest -b marshmallow-x86
repo sync
```

## 开始配置

自己的配置

```bash
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

```bash
cd android-repo
source build/envsetup.sh
lunch android_x86_64-eng  #选择7 x86-64-eng 系统
make -j16 iso_img
```

就完成了！ 可以直接用virtualbox/qemu使用生成的img，在不安装模式下是可以基本正常工作的。

## 定制内核(optional)

```bash
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 android-x86_defconfig #选择缺省android-x86配置
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 android-x86_defconfig
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 menuconfig #可以自己做些定制
make -C kernel O=/backup/android-related/android-repo/out/obj2 ARCH=x86 -j16  #编译成功内核
```

注意：

DO NOT make in kernel/ directly. If you do so, try

```bash
make -C kernel distclean
rm -rf $OUT/obj/kernel 
```

## 用qemu-system-x86_64测试

### BIOS 模式
```bash
qemu-system-x86_64 -enable-kvm -m 4G -cdrom android_x86_64.iso -vga std -serial stdio
```

### UEFI 模式（optional）
```bash
qemu-system-x86_64 -enable-kvm -m 4G -bios OVMF.fd -hdb android_x86_64.img -vga std -serial stdio
```

kernel 参数 
```bash
  nomodeset vga=788   即800x600:16bpp
  console=ttyS0  用串口交互
```
