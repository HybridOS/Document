
## 下载

### 帮助
 - https://wiki.tuna.tsinghua.edu.cn/MirrorUsage/android

###仓库地址：
 - git://aosp.tuna.tsinghua.edu.cn/android/
 
### 下载android 源码

#### 下载 repo
git clone git://aosp.tuna.tsinghua.edu.cn/android/git-repo.git/ 

#### 修改repo
```
google的地址
REPO_URL = 'https://gerrit.googlesource.com/git-repo'
改为清华大学的地址
REPO_URL = 'git://aosp.tuna.tsinghua.edu.cn/android/git-repo'
```

#### 下载 manifest
```
google 的地址
$ repo init -u https://android.googlesource.com/platform/manifest
改为清华大学的地址
$ repo init -u git://aosp.tuna.tsinghua.edu.cn/android/platform/manifest
```

#### 同步源码
 - 还是和以前一样  
`$ repo sync`
 
#### 替换已有的AOSP源代码的remote
```
如果你之前已经通过某种途径获得了AOSP的源码，但是你希望以后通过TUNA同步，只需要将.repo/manifest.xml中的
      <remote  name="aosp"
       fetch=".."
       review="https://android-review.googlesource.com/" />
改为下面的code即可：
      <remote  name="aosp"
       fetch="git://aosp.tuna.tsinghua.edu.cn/android/"
       review="https://android-review.googlesource.com/" />
这个方法也可以用来在同步Cyanogenmod代码的时候从TUNA同步部分代码
```

## 编译
### Initializing a Build Environment

#### Installing the JDK
```
$ sudo apt-get update  
$ sudo apt-get install openjdk-7-jdk  
```

#### Installing required packages
```
sudo apt-get install bison g++-multilib git gperf libxml2-utils make zlib1g-dev:i386 zip
```

### Building the System
#### Initialize
`$ source build/envsetup.sh`  

#### Choose a Target
```
$ lunch 20  
```

#### Update api
`make update-api  `

#### Build the Code
```
make -j16 
```
在TF笔记本上，结果如下　：
```
build mini_emulator_x86-userdebug
#### make completed successfully (01:24:32 (hh:mm:ss)) ####
```

### download and compile the kernel
### download
```
 - git clone https://android.googlesource.com/kernel/goldfish.git
```
### build

 
```
mkdir output-goldfish

cd goldfish
git branch -a

git checkout android-goldfish-3.10　　//这个分支虽然新，但有bug, 用emulator（android-sdk带的）不行，kernel crash

git checkout android-goldfish-3.10-m-dev //可以正常编译并运行！
```
第一种编译方法
```
/backup/android-related/android-repo/prebuilts/qemu-kernel/build-kernel.sh --arch=x86_64 --out=/backup/android-related/kernel-related/output-goldfish
```

第一种编译方法　结果如下：
```
 OBJCOPY arch/x86/boot/setup.bin
  BUILD   arch/x86/boot/bzImage
Setup is 15440 bytes (padded to 15872 bytes).
System is 4284 kB
CRC a721525e
Kernel: arch/x86/boot/bzImage is ready  (#1)
Kernel x86_64_emu prebuilt images (kernel-qemu and vmlinux-qemu) copied to /backup/android-related/kernel-related/output-goldfish successfully !
chyyuu@chyyuu-X599:/backup/android-related/kernel-related/goldfish$ ls ../output-goldfish/
kernel-qemu  LINUX_KERNEL_COPYING  README  vmlinux-qemu
```

第二种编译方法
```
export AOSP_TREE=/backup/android-related/android-repo
export CROSS_COMPILE=$AOSP_TREE/prebuilts/gcc/linux-x86/x86/x86_64-linux-android-4.9/bin/x86_64-linux-android-
make ARCH=x86_64 x86_64_emu_defconfig
make ARCH=x86_64 CC="${CROSS_COMPILE}gcc -mno-android" bzImage  -j16

$ more ustc-aosp-build-kernel.sh
----------------------------------
export AOSP_TREE=/media/chyyuu/opt2/ustc-aosp
export CROSS_COMPILE=$AOSP_TREE/prebuilts/gcc/linux-x86/x86/x86_64-linux-android-4.9/bin/x86_64-linux-android-
make mrproper
make ARCH=x86_64 O=../build-ustc-goldfish-kernel x86_64_emu_defconfig
cd ../build-ustc-goldfish-kernel
make ARCH=x86_64 CC="${CROSS_COMPILE}gcc -mno-android" bzImage  -j16

$ more tuna-aosp-build-kernel.sh
----------------------------------
export AOSP_TREE=/media/chyyuu/opt2/tuna-aosp
export CROSS_COMPILE=$AOSP_TREE/prebuilts/gcc/linux-x86/x86/x86_64-linux-android-4.9/bin/x86_64-linux-android-
make mrproper
make ARCH=x86_64 O=../build-tuna-goldfish-kernel x86_64_emu_defconfig
cd ../build-tuna-goldfish-kernel
make ARCH=x86_64 CC="${CROSS_COMPILE}gcc -mno-android" bzImage  -j16
```


第二种编译方法的运行
//设置　android-sdk的执行路径，这样才能用位于ools/emulator tools/monitor  platform-tools/adb
```
$more chysetupenv-android-sdk.sh  //chysetupenv-android-sdk.s文件内容如下
export PATH=$PATH:/media/chyyuu/opt2/Android/Sdk/tools:/media/chyyuu/opt2/tools/android-studio/bin:/media/chyyuu/opt2/Android/Sdk/platform-tools
```

//option 可以通过tools/monitor 来创建一个avd for x86-64 virtual machine

```
emulator -avd chy4 -kernel ./arch/x86_64/boot/bzImage -show-kernel

serial0 console
emulator: UpdateChecker: skipped version check
[    0.000000] Initializing cgroup subsys cpu
[    0.000000] Initializing cgroup subsys cpuacct
[    0.000000] Linux version 3.10.0+ (chyyuu@chyyuu-X599) (gcc version 4.9 20140827 (prerelease) (GCC) ) #3 PREEMPT Mon Oct 5 16:36:03 CST 2015
[    0.000000] Command line: qemu.gles=0 qemu=1 console=ttyGF0 android.qemud=ttyGF1 androidboot.hardware=goldfish clocksource=pit android.checkjni=1 ndns=1
[    0.000000] e820: BIOS-provided physical RAM map:
...
```
可以正常运行！

第二种运行方法　（用AOSP的emulator）

```
emulator -avd chy4 -kernel kernel-related/goldfish/arch/x86_64/boot/bzImage -show-kernel -system android-repo/out/target/product/generic/system.img -sysdir android-repo/out/target/product/generic -ramdisk android-repo/out/target/product/generic/ramdisk.img -data android-repo/out/target/product/generic/userdata.img 
```

更完整的路径　都在/opt2/中
```
emulator -avd chy4 -kernel /media/chyyuu/opt2/kernel-src/build-ustc-goldfish-kernel/arch/x86_64/boot/bzImage -show-kernel -system /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/system.img -sysdir /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/ -ramdisk /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/ramdisk.img -data /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/userdata.img 


emulator -avd chy4 -kernel /media/chyyuu/opt2/kernel-src/build-tuna-goldfish-kernel/arch/x86_64/boot/bzImage -show-kernel -system /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/system.img -sysdir /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/ -ramdisk /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/ramdisk.img -data /media/chyyuu/opt2/Android/Sdk/system-images/android-23/default/x86_64/userdata.img 

[    0.267224] Failed to execute /init
[    0.267247] Kernel panic - not syncing: No init found.  Try passing init= option to kernel. See Linux Documentation/init.txt for guidance.
[    0.267262] CPU: 0 PID: 1 Comm: swapper Not tainted 3.10.0+ #3
[    0.267273] Hardware name:  , BIOS QEMU 01/01/2007
[    0.267284]  0000000000000000 ffff8800bb033eb8 ffffffff815acb4c ffff8800bb033f38
[    0.267325]  ffffffff815ab90c ffffffff81a161c0 0000000000000008 ffff8800bb033f48
[    0.267365]  ffff8800bb033ee8 ffff880080000000 ffff8800bffd2120 000000000000090a
[    0.267405] Call Trace:
[    0.267419]  [<ffffffff815acb4c>] dump_stack+0x19/0x1b
[    0.267431]  [<ffffffff815ab90c>] panic+0xae/0x19d
[    0.267444]  [<ffffffff815a85ba>] ? rest_init+0x7e/0x7e
[    0.267456]  [<ffffffff815a8687>] kernel_init+0xcd/0xd1
[    0.267468]  [<ffffffff815b1aea>] ret_from_fork+0x7a/0xb0
[    0.267480]  [<ffffffff815a85ba>] ? rest_init+0x7e/0x7e
```
前面正常，最后找不到init

提示　kernel 通过串口输出
```
linux:
 qemu-system-i386 -kernel xxx -initrd xxx -append "root=/dev/ram rdinit=/sbin/init console=ttyS0" -serial stdio  

android：
emulator －show-kernel ＃显示linux内核启动参数
```

## 问题
### 无法用多线程下载tuna mirror的android
一个IP只支持４个下载线程，所以　最多 repo sync -j4

### 在安装zlib1g-dev:i386时，无法安装，待解决。


### 在执行 `make updat-api`时，碰到java的版本问题
```
************************************************************

You are attempting to build with the incorrect version
of java.

Your version is: Picked up JAVA_TOOL_OPTIONS: -javaagent:/usr/share/java/jayatanaag.jar
 java version "1.7.0_79" OpenJDK Runtime Environment (IcedTea 2.5.6)
(7u79-2.5.6-0ubuntu1.15.04.1) OpenJDK 64-Bit Server VM (build 24.79-b02, mixed mode).
The required version is: "1.7.x"

Please follow the machine setup instructions at
 https://source.android.com/source/initializing.html
************************************************************
```

解决办法  http://askubuntu.com/questions/664963/java-7-reporting-not-installed

```
In build/core/main.mk, the java_version_str contains the output of "java -version":

Picked up JAVA_TOOL_OPTIONS: -javaagent:/usr/share/java/jayatanaag.jar 
java version "1.7.0_79"
OpenJDK Runtime Environment (IcedTea 2.5.6) (7u79-2.5.6-0ubuntu1.15.04.1)
OpenJDK 64-Bit Server VM (build 24.79-b02, mixed mode)


The java_version is supposed to extract "1.7.0_79" using grep. The caret at the beginning of the grep regular expression "^java" indicates that the author intended to be able to find a line starting with "java". Unfortunately, GNU Make variables do not store the line endings. So the grep only sees one giant line starting with "Picked".

The grep works accidentally when the "java version" happens to be on the first line. This is probably why "unset _JAVA_OPTIONS" was added in java_version_str, because it was causing similar problems.

The simplest solution is to follow the current band-aid solution by adding "unset JAVA_TOOL_OPTIONS" to java_version_str:
in LINE 139,140
java_version_str := $(shell unset _JAVA_OPTIONS && unset JAVA_TOOL_OPTIONS && java -version 2>&1)
javac_version_str := $(shell unset _JAVA_OPTIONS && unset JAVA_TOOL_OPTIONS && javac -version 2>&1)

The real solution would be to not use an intermediate variable java_version_str and perform the grep directly:

java_version := $(shell java -version 2>&1 | grep '^java .*[ "]1\.7[\. "$$]')
javac_version := $(shell java -version 2>&1 | grep '[ "]1\.7[\. "$$]')

```
