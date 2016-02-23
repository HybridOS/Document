**获取Android-x86的源**
##简介
 本文尽可能多的为大家给x86平台（例如华硕的Eee pc等）构建android系统提供一些信息。编译构建生成的镜像也较好的运行在虚拟机软件中（如qemu 或者virtual box）。
        
  当下从我们的git 仓库获取构建Android-x86已经不是什么难事了，你也不用去apply任何patch，只需要按照下面的介绍来做就ok了（当然了，这里是对有一定基础的同学们来说的，如果你是和我一样还是菜鸟的话，就多费的事儿啦）。
 
##Andorid-x86源码树的分支
 
 由于Android 官方AOSP的发展（更新）比较频繁，所以我们使用不同的分支来维护不同的版本的AOSP。
 
- marshmallow-x86
Based on Android 6.0 release (Marshmallow 棉花糖).
- lollipop-x86
Based on Android 5.0 release (Lollipop).
- kitkat-x86
Based on Android 4.4 release (KitKat).
- jb-x86
Based on Android 4.3 release (Jelly Bean).
- ics-x86
Based on Android 4.0 release (Ice Cream Sandwich).
- honeycomb-x86
Based on Android 3.2 release (Honeycomb).
- gingerbread-x86
Based on Android 2.3 release (Gingerbread).
- froyo-x86
Based on Android 2.2 release (Froyo).
- eclair-x86
Based on Android 2.1 release (Eclair).
- donut-x86
Based on Android 1.6 release (Donut).
- cupcake-x86 (aka android-x86-v0.9)
Based on Android 1.5 release (Cupcake).

##同步Android-x86的源码
首先,按照AOSP页面 [Establishing a Build Environment](http://source.android.com/source/initializing.html) 来配置您的构建环境。然后
```
$ mkdir android-x86
$ cd android-x86
$ repo init -u git://gitscm.sf.net/gitroot/android-x86/manifest -b $branch
$ repo sync
```
这里的 $branch 是指前面章节所描述的分支名称。
**note：由于网络原因国内的访问不了google的仓库，所以上面的repo命令（一个python脚本封装了git的命令）中的
   REPO_URL = 'https://gerrit.googlesource.com/git-repo'改为
REPO_URL = 'git://git.omapzoom.org/git-repo.git'

往后如果你想与android-x86的仓库保持同步，只需要执行repo sync. 而不用再次执行
repo init。然而如果你在repo sync的时候遇到问题，后面的章节（如何解决repo sync 冲突）也许可以帮到你。
**note：** Android-x86 仓库的体积是非常大的（至少在10GB以上）。如果你在同步的过程中遇到问题，很可能是网络
问题或者服务器比较繁忙造成的。再次执行 repo sync直到下载成功。

##**构建安装镜像**
一旦上面的同步完成，就可以着手构建一个cdrom ISO 镜像。构建kikat-x86 之前的分支需要oracle官方的java1.6（openJDK可能会有问题）的支持，请在oracle官网下载安装。
从lollipop-x86分支开始, java 1.7 和 openJDK被支持。
**note：**包括froyo-x86分支之前，32位系统和64位系统环境上构建都支持，从gingerbread-x86分支开始，建议最好在64位系统环境中构建；

###**选择一个目标**

- lollipop-x86 / marshmallow-x86
	- android_x86: for 32-bit x86 platform
	- android_x86_64: for 64-bit x86_64 platform
- jb-x86 / kitkat-x86
	- android_x86: for x86 platform
- honeycomb-x86 / ics-x86
	- generic_x86: for generic x86 PC/notebook
	- amd_brazos: for AMD Brazos platform
	- eeepc: for ASUS EeePC family only
	- asus_laptop: for some ASUS laptops
	- tegav2: for Tegatech Tegav2 (may work with other Atom N45x based tablets)
- froyo-x86 / gingerbread-x86
	- generic_x86: for generic x86 PC/notebook
	- eeepc: for ASUS EeePC family only
	- asus_laptop: for some ASUS laptops
	- tegav2: for Tegatech Tegav2 (may work with other Atom N45x based tablets)
	- sparta: for Dell Inspiron Mini Duo platform
	- vm: for virtual machine (virtual box, qemu, vmware)
	- motion_m1400: for Motion M1400 (Intel Centrino M based with Intel PRO/Wireless)
- eclair-x86
	- generic_x86: for generic x86 PC/notebook
	- eeepc: for ASUS EeePC family only
	- q1u: for Samsung Q1U
	- s5: for Viliv S5
- donut-x86
	- eeepc: for ASUS EeePC family
	- q1u: for Samsung Q1U
	- s5: for Viliv S5

事实上，由于历史原因，对于 generic x86 PC， notebook or netbook设备来说你必须使用eeepc包括donut-x86 
之前的分支。从eclair-x86开始从eeepc切换到提供华硕的EeePC产品系列。所以如果你没有使用EeePC那么不要用前面所说的分支；
简而言之,如果你不知道如何选择,使用eeepc 的donut-x86分支,并使用generic_x86 替代eclair-x86 和ics-x86分支。
但是请注意generic_x86目标只具备基本功能，有一些高级特性像硬件加速不能被支持。

从 jb-x86 分支开始我们尝试使用android_x86 作为对所有x86设备的一个统一的target。然而，他可能没有对
一个特定的设备进行优化。如果你是一个开发者，你可以基于android_x86 来为你自己的设备创建一个目标。
如果你想为你的x86设备添加一个新的target，[参考这篇文章](http://www.android-x86.org/documents/how-to-add-new-x86-platforms)。

###**使用lunch 命令（强烈建议）**
使用如下命令将build/envsetup.sh加到你当前的bash环境中来帮助我们构建：
```
source build/envsetup.sh
```
现在你可以使用lunch命令来选择一个你要构建的目标，命令格式如下：
```
lunch $TARGET_PRODUCT-$TARGET_BUILD_VARIANT
```
这里的 ```$TARGET_PRODUCT``` 是指前面章节描述的任何一个目标，```$TARGET_BUILD_VARIANT```的值可以是这三种```eng / user / userdebug```.
例如
```lunch android_x86-userdebug```
然后你就而已使用m命令来构建你的ISO了
``` m -jN iso_img```
命令```m```等效于```make```，但是你可以在android-x86的任何一个子目录中执行。
从 froyo-x86分支开始加入了``lunch``命令的菜单选择。你只需要输入lunch，在输出的列表中
选择你所要目标序号就可以了，也可以输入```lunch $number``` 直接选择。

###**直接构建**
下面为android_x86构建一个live cdrom ISO 镜像,输入：
```
make iso_img TARGET_PRODUCT=android_x86
```

为tegav2生成一个live cdrom ISO， 输入：
```
make iso_img TARGET_PRODUCT=tegav2
```
如果一切顺利，会在产生```out/target/product/x86/android_x86.iso``` 文件；
当然不用说了，如果你的编译机够牛叉，完全可以任性一点;
```
make -jN iso_img TARGET_PRODUCT=android_x86	
```
###**使用 buildspec.mk**
你可以创建一个 buildspec.mk文件来记录下你需要经常build的target：
```TARGET_PRODUCT:=android_x86
TARGET_BUILD_VARIANT:=userdebug
TARGET_BUILD_TYPE:=release
TARGET_KERNEL_CONFIG:=android-x86_defconfig
```
将文件 buildspec.mk 放在你的android-x86目录下，你只需要输入：
```make -jN iso_img```

###**构建一个小尺寸的镜像**
如果你在编译系统中有安装squashfs-tools 4.0（不能小于这个版本），那么生成的Android core filesystem（Android核心文件系统）
将会被squashfs压缩。所以，生成的iso文件尺寸将会非常小（大约只有原来的30%-40%）。如果你不希望被squashfs压缩，添加```USE_SQUASHFS=0```在make的时候。
可以再你的buildspec.mk 文件中添加：

```USE_SQUASHFS := 0```

包括froyo-x86分支之前，如果你想得到较小的镜像，你可能需要remove掉调试符号：

```TARGET_STRIP := 1```

从 gingerbread-x86 分支开始，调试符号缺省被strip掉了，不需要再使用任何选项；

##**测试镜像**
构建生成的镜像文件位置为：
```
out/target/product/$TARGET_PRODUCT/$TARGET_PRODUCT.iso
```
你可以非常方便的使用 virtual box 或者 qemu来测试。引导阶段，选择VESA或者debug模式引导。当然你也可以
吧ISO刻录到你的光盘并在实际物理机上测试。引导阶段将会自动测检测你的硬件并加载必要的模块。如果你遇到有关默认 frame buffer 驱动的
问题，可以尝试VESA 模式（第二个引导项）。

从honeycomb-x86分支开始，支持混合ISO格式。就是说，ISO文件可以被解进U盘。你可以直接制作一个USB启动盘：
```
dd if=out/target/product/x86/android_x86.iso of=/dev/sdX
```
这里的/dev/sdX是指USB设备的名称（注意不是分区名称）。此功能只用于2011/12/25后发布的iso文件。
注意：```usb_img ```已经被取消不再使用。

有些电脑的BISO（如宏碁 AO）不能引导上面的方式制作的启动盘。如果是这样,你可能需要尝试创建可引导USB驱动器通过unetbootin工具；
linux和windows用户都可以按照下面的指导来（ Gregory Gee 的建议）：
    
   - 1.下载ISO文件。
   - 2.下载UNetbootin 从http://unetbootin.sourceforge.net/  （译者在win7上使用UltraISO）
   - 3.确保你的U盘被格式化。否则会失败；可以格式化我格式化成fat32（译者格式化为ntfs）
   - 4.运行UNetbootin
      - 选择USB设备作为Drive。
	  - 选择android-x86 ISO文件为disk image。
	  - 点击ok并等待。
      - 大约需要一分钟的时间。及得下次版本升级之后的ISO文件需要再次格式化U盘。

还有一种方式是使用 [Linux Live USB Creator ( LiLi ) project](http://www.linuxliveusb.com/)  

##**高级**
本章节为资深用户提供一些有用的信息。可能需要你具备一些linux的专业技能才能完成。

###**安装到USB disk上**
对于资深用户，你也许需要创建一个可以启动的随身USB设备。来看下面的步骤：
1.在你的U盘上安装grub

  - 找一个linux机器安装了最近的grub
  - 使用fdisk或者gpartd给U盘分区，并标记为可引导分区。
  - 格式化分区为ext3（强烈建议）或者vfat格式
  - 挂在你的U盘到```/mnt```上
  - ```cd /mnt```
  -```grub-install --root-directory=. --no-floppy /dev/<your usb device node name>```
  -```cd /boot/grub```
  -按照下面的方式创建你的 ```menu.lst```
2.添加下面的部分到 ```menu.lst```中
```
title Run Android
	kernel /android/kernel root=/dev/ram0 androidboot.hardware=android_x86 acpi_sleep=s3_bios,s3_mode SRC=/android
	initrd /android/initrd.img

title Run Android (VESA mode)
	kernel /android/kernel root=/dev/ram0 androidboot.hardware=android_x86 acpi_sleep=s3_bios,s3_mode vga=788 SRC=/android
	initrd /android/initrd.img

title Run Android (Debug mode)
	kernel /android/kernel root=/dev/ram0 androidboot.hardware=android_x86 acpi_sleep=s3_bios,s3_mode vga=788 SRC=/android DEBUG=1
	initrd /android/initrd.img
```

（从kitkat-x86开始  ```SRC= parameter``` 被省略如果system image 和内核在同一个目录下）

3.在USB 驱动器上创建 ```/android ```目录并copy 这四个文件``` kernel initrd.img ramdisk.img system.sfs``` (或者 ```system.img``` 如果你设置 ```USE_SQUASHFS=0```)	到它下面。

至此，你就可以使用USB disk 启动你的Android系统了。注意啦，这种状态下，所有的数据都保存早ramdisk（也就是内存）当中，所以断电之后一切将不复存在。如果你希望保存数据到实际磁盘，请看下面的片段。

###**安装到物理磁盘**
安装到物理磁盘和安装到USB磁盘是一样的。甚至你不要创建新的分区，只需要将Android文件copy到已经存在的分区，安装grub到物理磁盘（如果之前没有安装过），修改menu.lst.
有人回问了,假设我的硬盘是空的怎么办?如何安装grub和复制文件到它吗?有几种方法可以做到这一点。我提供两个:
 
 - 从任何救援cd引导像systemrescuecd,按照前一节中的说明。
 - 安装你喜爱的linux发行版,然后复制Android文件和修改grub菜单。
   
###**保存数据到 USB/物理 磁盘**
我们支持两种方式的数据保存到磁盘。

- 创建一个名为data的子目录在/android目录下。用户数据将直接保存到该目录。这种方法只适用于ext3分区。
- 创建一个单独的分区并保存数据。你必须添加 DATA= <device_name>启动选项。例如,假设你的数据分区/dev/sda2,然后添加 DATA=sda2启动选项。

###**如何解决repo sync 冲突**
这里可能有几个原因造成repo sync时发生冲突:
 
 - 手动修改了本地目录树
 - upstream发生了变化。因为我们通常与原始Android库保持同步,有时我们需要变基。这样改变了历史记录可能会导致冲突。
 - 在本节中,我们假设冲突是由于上游发生了变化。也就是说你没有本地修改，如果你这样做了,那你必须自己解决冲突。
如果你按照下面方式,你可能会丢失你的本地修改。

这里有一个冲突的例子:
```
$ repo sync
remote: Counting objects: 71, done.
remote: Compressing objects: 100% (41/41), done.
remote: Total 65 (delta 25), reused 28 (delta 9)
Unpacking objects: 100% (65/65), done.
From git://git.tarot.com.tw/android-x86/platform/manifest
   d53e6c1..2de7a11  android-1.5r2 -> origin/android-1.5r2
 * [new branch]      android-1.5r3 -> origin/android-1.5r3
 * [new branch]      android-sdk-1.5_r3 -> origin/android-sdk-1.5_r3
   d53e6c1..c544020  cupcake    -> origin/cupcake
 * [new branch]      cupcake-release -> origin/cupcake-release
   f4d79b1..6f7e0dd  donut      -> origin/donut
 + 7308d31...4a4f936 lan        -> origin/lan  (forced update)
 + b480a6d...d82496e local      -> origin/local  (forced update)
 + 11c9d96...84345fb master     -> origin/master  (forced update)
 + 5bcbf93...66e92cc mirror     -> origin/mirror  (forced update)
 + 9f3092f...665f9e8 ssh        -> origin/ssh  (forced update)
 + c6037be...d70927f ssh-mirror -> origin/ssh-mirror  (forced update)
 + 00a823f...3ddaf66 test       -> origin/test  (forced update)
 * [new tag]         android-1.5r3 -> android-1.5r3
 * [new tag]         android-sdk-1.5_r3 -> android-sdk-1.5_r3
Fetching projects: 100% (128/128), done.  
project .repo/manifests/
First, rewinding head to replay your work on top of it...
Applying: merge donut, change or add the projects to x86 port
error: patch failed: default.xml:3
error: default.xml: patch does not apply
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging default.xml
CONFLICT (content): Merge conflict in default.xml
Failed to merge in the changes.
Patch failed at 0001 merge donut, change or add the projects to x86 port

When you have resolved this problem run "git rebase --continue".
If you would prefer to skip this patch, instead run "git rebase --skip".
To restore the original branch and stop rebasing run "git rebase --abort".

```
```repo sync``` 由于冲突停止。因为我们没有本地修改,就忽略它通过``` git rebase --skip```

``` $cd .repo/manifests
$ git rebase --skip
HEAD is now at 4a4f936 add branch for local lan
Applying: add platform/frameworks/policies/base to x86
error: patch failed: default.xml:18
error: default.xml: patch does not apply
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging default.xml
No changes -- Patch already applied.
Applying: add branch for local lan
error: patch failed: default.xml:1
error: default.xml: patch does not apply
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging default.xml
No changes -- Patch already applied.```

如果抱怨另一个冲突,再次做```git rebase --skip```,直到变基过程完成,通常就足够了.
但如果你希望是绝对干净,你可以忽略这个分支并checkout一个新的分支:

```git checkout -t kitkat-x86 m/kitkat-x86```

这可能不是最好的方法来解决冲突,但对于初学者来说应该是最简单的方式。如果你有更好的建议,就告诉我们。

###**自定义内核**
如果你想自定义指定硬件的内核,参考[这篇文章](http://www.android-x86.org/documents/customizekernel)。

###**本页面完成，有时间在搞其他页面！！！欢迎交流**