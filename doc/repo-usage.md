
##　repo 命令汇总
```
repo help --all
usage: repo COMMAND [ARGS]
The complete list of recognized repo commands are:
  abandon        Permanently abandon a development branch
  branch         View current topic branches
  branches       View current topic branches //查看分支
  checkout       Checkout a branch for development  //切换分支
  cherry-pick    Cherry-pick a change.
  diff           Show changes between commit and working tree
  diffmanifests  Manifest diff utility
  download       Download and checkout a change
  forall         Run a shell command in each project
  gitc-init      Initialize a GITC Client.
  grep           Print lines matching a pattern
  help           Display detailed help on a command
  info           Get info on the manifest branch, current branch or unmerged branches
  init           Initialize repo in the current directory
  list           List projects and their associated directories
  manifest       Manifest inspection utility
  overview       Display overview of unmerged project branches
  prune          Prune (delete) already merged topics
  rebase         Rebase local branches on upstream branch
  selfupdate     Update repo to the latest version
  smartsync      Update working tree to the latest known good revision
  stage          Stage file(s) for commit
  start          Start a new branch for development // 创建分支
  status         Show the working tree status
  sync           Update working tree to the latest revision  //查看文件状态
  upload         Upload changes for code review
  version        Display the version of repo
See 'repo help <command>' for more information on a specific command.

```

## manifest仓库
因为Android源码引用了很多开源项目，每一个子项目都是一个Git仓库，每个Git仓库都有很多分支版本，
为了方便统一管理各个子项目的Git仓库，repo也会建立一个Git仓库，用来记录当前Android版本下各个
子项目的Git仓库分别处于哪一个分支，这个仓库通常叫做：manifest仓库。

## 安装repo

官方的repo脚本下载方法：
```
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo >  ./repo
OR
git clone git://git.omapzoom.org/git-repo.git
REPO_URL = 'git://git.omapzoom.org/git-repo.git'
REPO_REV = 'stable'
OR
git clone git://aosp.tuna.tsinghua.edu.cn/android/git-repo.git
REPO_URL = 'git://aosp.tuna.tsinghua.edu.cn/android/git-repo'
REPO_REV = 'stable'
```

## 通过repo下载源码
```
repo init -u https://android.googlesource.com/platform/manifest
OR
repo init -u git://git.omapzoom.org/platform/manifest.git
repo init -u git://git.omapzoom.org/platform/manifest.git -b android-6.0.0_r1
OR
repo init -u git://aosp.tuna.tsinghua.edu.cn/android/platform/manifest
repo init -u git://aosp.tuna.tsinghua.edu.cn/android/platform/manifest -b android-6.0.0_r1

然后
repo sync 
```

## 本地切换分支
```
repo init -b android-6.0.0_r1
repo sync -l
```

##查看当前可用的Android源码分支和版本
```
git --git-dir .repo/manifests/.git/ branch -a
OR
cd .repo/manifests
git branch -a | cut -d / -f 3
```

## 查看android各个版本的不同
```
repo forall -pc 'git log --no-merges --oneline branch-1..branch-2'
OR
repo forall -pc 'git log --no-merges --oneline android-4.4.2_r2..android-4.4.2_r1'
OR
repo forall -pc 'git log --no-merges --oneline android-4.4.2_r2..android-4.4.2_r1' > /tmp/android-4.4.2_r2-android-4.4.2_r1-diff.txt
```


## android的分支
FROM 
http://source.android.com/source/build-numbers.html#source-code-tags-and-builds

```
Code name	Version	API level
Marshmallow	6.0	API level 23
Lollipop	5.1	API level 22
Lollipop	5.0	API level 21
KitKat	4.4 - 4.4.4	API level 19
Jelly Bean	4.3.x	API level 18
Jelly Bean	4.2.x	API level 17
Jelly Bean	4.1.x	API level 16
Ice Cream Sandwich	4.0.3 - 4.0.4	API level 15, NDK 8
Ice Cream Sandwich	4.0.1 - 4.0.2	API level 14, NDK 7
Honeycomb	3.2.x	API level 13
Honeycomb	3.1	API level 12, NDK 6
Honeycomb	3.0	API level 11
Gingerbread	2.3.3 - 2.3.7	API level 10
Gingerbread	2.3 - 2.3.2	API level 9, NDK 5
Froyo	2.2.x	API level 8, NDK 4
Eclair	2.1	API level 7, NDK 3
Eclair	2.0.1	API level 6
Eclair	2.0	API level 5
Donut	1.6	API level 4, NDK 2
Cupcake	1.5	API level 3, NDK 1
(no code name)	1.1	API level 2
(no code name)	1.0	API level 1
```


## android-x86 分支
ls -la android-x86-repo/.repo/manifests.git/refs/remotes/origin/

android-5.1.1_r19
android-5.1.1_r20
android-5.1.1_r22
android-5.1.1_r23
android-5.1.1_r24
android-6.0.0_r1
android-cts-5.1_r3
android-cts-6.0_r1
cupcake-x86
donut-x86
eclair-x86
froyo-x86
gingerbread-x86
honeycomb-x86
ics-x86
jb-x86
kitkat-x86
lollipop-x86
marshmallow-cts-dev
marshmallow-dev
marshmallow-x86


### Source Code Tags and Builds
```
uild	Branch	Version	Supported devices   
MRA58K	android-6.0.0_r1	Marshmallow	Nexus 5, Nexus 6, Nexus 7 (flo/deb), Nexus 9 (volantis/volantisg), Nexus Player
LMY48W	android-5.1.1_r24	Lollipop	Nexus 6
LVY48H	android-5.1.1_r23	Lollipop	Nexus 6 (For Project Fi ONLY)
LYZ28M	android-5.1.1_r22	Lollipop	Nexus 6 (For T-Mobile ONLY)
LMY48U	android-5.1.1_r20	Lollipop	Nexus 7 (deb)
LMY48T	android-5.1.1_r19	Lollipop	Nexus 4, Nexus 6, Nexus 9 (volantis/volantisg), Nexus 10
LVY48F	android-5.1.1_r18	Lollipop	Nexus 6 (For Project Fi ONLY)
LYZ28K	android-5.1.1_r17	Lollipop	Nexus 6 (For T-Mobile ONLY)
LMY48P	android-5.1.1_r16	Lollipop	Nexus 7 (deb)
LMY48N	android-5.1.1_r15	Lollipop	Nexus Player
LMY48M	android-5.1.1_r14	Lollipop	Nexus 4, Nexus 5, Nexus 6, Nexus 7 (flo), Nexus 9 (volantis/volantisg), Nexus 10
LVY48E	android-5.1.1_r13	Lollipop	Nexus 6 (For Project Fi ONLY)
LYZ28J	android-5.1.1_r12	Lollipop	Nexus 6 (For T-Mobile ONLY)
LMY48J	android-5.1.1_r10	Lollipop	Nexus Player
LMY48I	android-5.1.1_r9	Lollipop	Nexus 4, Nexus 5, Nexus 6, Nexus 7 (flo), Nexus 9 (volantis/volantisg), Nexus 10
LVY48C	android-5.1.1_r8	Lollipop	Nexus 6 (For Project Fi ONLY)
LMY48G	android-5.1.1_r6	Lollipop	Nexus 7 (flo)
LYZ28E	android-5.1.1_r5	Lollipop	Nexus 6 (For T-Mobile ONLY)
LMY47Z	android-5.1.1_r4	Lollipop	Nexus 6 (All carriers except T-Mobile US)
LMY48B	android-5.1.1_r3	Lollipop	Nexus 5
LMY47X	android-5.1.1_r2	Lollipop	Nexus 9 (volantis)
LMY47V	android-5.1.1_r1	Lollipop	Nexus 7 (flo/grouper), Nexus 10, Nexus Player
LMY47O	android-5.1.0_r5	Lollipop	Nexus 4, Nexus 7 (flo/deb)
LMY47M	android-5.1.0_r4	Lollipop	Nexus 6 (For T-Mobile ONLY)
LMY47I	android-5.1.0_r3	Lollipop	Nexus 5, Nexus 6
LMY47E	android-5.1.0_r2	Lollipop	Nexus 6
LMY47D	android-5.1.0_r1	Lollipop	Nexus 5, Nexus 6, Nexus 7 (grouper/tilapia), Nexus 10, Nexus Player
LRX22L	android-5.0.2_r3	Lollipop	Nexus 9 (volantis/volantisg)
LRX22G	android-5.0.2_r1	Lollipop	Nexus 7 (flo/deb/grouper/tilapia), Nexus 10
LRX22C	android-5.0.1_r1	Lollipop	Nexus 4, Nexus 5, Nexus 6 (shamu), Nexus 7 (flo), Nexus 9 (volantis/volantisg), Nexus 10
LRX21V	android-5.0.0_r7.0.1	Lollipop	Nexus Player (fugu)
LRX21T	android-5.0.0_r6.0.1	Lollipop	Nexus 4
LRX21R	android-5.0.0_r5.1.0.1	Lollipop	Nexus 9 (volantis)
LRX21Q	android-5.0.0_r5.0.1	Lollipop	Nexus 9 (volantis)
LRX21P	android-5.0.0_r4.0.1	Lollipop	Nexus 7 (flo/grouper), Nexus 10
LRX21O	android-5.0.0_r3.0.1	Lollipop	Nexus 5 (hammerhead), Nexus 6 (shamu)
LRX21M	android-5.0.0_r2.0.1	Lollipop	Nexus Player (fugu)
LRX21L	android-5.0.0_r1.0.1	Lollipop	Nexus 9 (volantis)
KTU84Q	android-4.4.4_r2	KitKat	Nexus 5 (hammerhead) (For 2Degrees/NZ, Telstra/AUS and India ONLY)
KTU84P	android-4.4.4_r1	KitKat	Nexus 5, Nexus 7 (flo/deb/grouper/tilapia), Nexus 4, Nexus 10
KTU84M	android-4.4.3_r1.1	KitKat	Nexus 5 (hammerhead)
KTU84L	android-4.4.3_r1	KitKat	Nexus 7 (flo/deb/grouper/tilapia), Nexus 4, Nexus 10
KVT49L	android-4.4.2_r2	KitKat	Nexus 7 (deb Verizon)
KOT49H	android-4.4.2_r1	KitKat	Nexus 5, Nexus 7 (flo/deb/grouper/tilapia), Nexus 4, Nexus 10
KOT49E	android-4.4.1_r1	KitKat	Nexus 5, Nexus 7 (flo/deb/grouper/tilapia), Nexus 4, Nexus 10
KRT16S	android-4.4_r1.2	KitKat	Nexus 7 (flo/deb/grouper/tilapia), Nexus 4, Nexus 10
KRT16M	android-4.4_r1	KitKat	Nexus 5 (hammerhead)
JLS36I	android-4.3.1_r1	Jelly Bean	Nexus 7 (deb)
JLS36C	android-4.3_r3	Jelly Bean	Nexus 7 (deb)
JSS15R	android-4.3_r2.3	Jelly Bean	Nexus 7 (flo)
JSS15Q	android-4.3_r2.2	Jelly Bean	Nexus 7 (flo)
JSS15J	android-4.3_r2.1	Jelly Bean	Nexus 7 (flo/deb)
JSR78D	android-4.3_r2	Jelly Bean	Nexus 7 (deb)
JWR66Y	android-4.3_r1.1	Jelly Bean	Galaxy Nexus, Nexus 7 (grouper/tilapia), Nexus 4, Nexus 10
JWR66V	android-4.3_r1	Jelly Bean	Galaxy Nexus, Nexus 7 (grouper/tilapia), Nexus 4, Nexus 10
JWR66N	android-4.3_r0.9.1	Jelly Bean	Galaxy Nexus, Nexus 7 (grouper/tilapia/flo), Nexus 4, Nexus 10
JWR66L	android-4.3_r0.9	Jelly Bean	Nexus 7
JDQ39E	android-4.2.2_r1.2	Jelly Bean	Nexus 4
JDQ39B	android-4.2.2_r1.1	Jelly Bean	Nexus 7
JDQ39	android-4.2.2_r1	Jelly Bean	Galaxy Nexus, Nexus 7, Nexus 4, Nexus 10
JOP40G	android-4.2.1_r1.2	Jelly Bean	Nexus 4
JOP40F	android-4.2.1_r1.1	Jelly Bean	Nexus 10
JOP40D	android-4.2.1_r1	Jelly Bean	Galaxy Nexus, Nexus 7, Nexus 4, Nexus 10
JOP40C	android-4.2_r1	Jelly Bean	Galaxy Nexus, Nexus 7, Nexus 4, Nexus 10
JZO54M	android-4.1.2_r2.1	Jelly Bean	
JZO54L	android-4.1.2_r2	Jelly Bean	
JZO54K	android-4.1.2_r1	Jelly Bean	Nexus S, Galaxy Nexus, Nexus 7
JRO03S	android-4.1.1_r6.1	Jelly Bean	Nexus 7
JRO03R	android-4.1.1_r6	Jelly Bean	Nexus S 4G
JRO03O	android-4.1.1_r5	Jelly Bean	Galaxy Nexus
JRO03L	android-4.1.1_r4	Jelly Bean	Nexus S
JRO03H	android-4.1.1_r3	Jelly Bean	
JRO03E	android-4.1.1_r2	Jelly Bean	Nexus S
JRO03D	android-4.1.1_r1.1	Jelly Bean	Nexus 7
JRO03C	android-4.1.1_r1	Jelly Bean	Galaxy Nexus
IMM76L	android-4.0.4_r2.1	Ice Cream Sandwich	 
IMM76K	android-4.0.4_r2	Ice Cream Sandwich	Galaxy Nexus
IMM76I	android-4.0.4_r1.2	Ice Cream Sandwich	Galaxy Nexus
IMM76D	android-4.0.4_r1.1	Ice Cream Sandwich	Nexus S, Nexus S 4G, Galaxy Nexus
IMM76	android-4.0.4_r1	Ice Cream Sandwich	
IML77	android-4.0.3_r1.1	Ice Cream Sandwich	
IML74K	android-4.0.3_r1	Ice Cream Sandwich	Nexus S
ICL53F	android-4.0.2_r1	Ice Cream Sandwich	Galaxy Nexus
ITL41F	android-4.0.1_r1.2	Ice Cream Sandwich	Galaxy Nexus
ITL41D	android-4.0.1_r1.1	Ice Cream Sandwich	Galaxy Nexus
ITL41D	android-4.0.1_r1	Ice Cream Sandwich	Galaxy Nexus
GWK74	android-2.3.7_r1	Gingerbread	Nexus S 4G
GRK39F	android-2.3.6_r1	Gingerbread	Nexus One, Nexus S
GRK39C	android-2.3.6_r0.9	Gingerbread	Nexus S
GRJ90	android-2.3.5_r1	Gingerbread	Nexus S 4G
GRJ22	android-2.3.4_r1	Gingerbread	Nexus One, Nexus S, Nexus S 4G
GRJ06D	android-2.3.4_r0.9	Gingerbread	Nexus S 4G
GRI54	android-2.3.3_r1.1	Gingerbread	Nexus S
GRI40	android-2.3.3_r1	Gingerbread	Nexus One, Nexus S
GRH78C	android-2.3.2_r1	Gingerbread	Nexus S
GRH78	android-2.3.1_r1	Gingerbread	Nexus S
GRH55	android-2.3_r1	Gingerbread	earliest Gingerbread version, Nexus S
FRK76C	android-2.2.3_r2	Froyo	 
FRK76	android-2.2.3_r1	Froyo	
FRG83G	android-2.2.2_r1	Froyo	Nexus One
FRG83D	android-2.2.1_r2	Froyo	Nexus One
FRG83	android-2.2.1_r1	Froyo	Nexus One
FRG22D	android-2.2_r1.3	Froyo	
FRG01B	android-2.2_r1.2	Froyo	
FRF91	android-2.2_r1.1	Froyo	Nexus One
FRF85B	android-2.2_r1	Froyo	Nexus One
EPF21B	android-2.1_r2.1p2	Eclair	 
ESE81	android-2.1_r2.1s	Eclair	
EPE54B	android-2.1_r2.1p	Eclair	Nexus One
ERE27	android-2.1_r2	Eclair	Nexus One
ERD79	android-2.1_r1	Eclair	Nexus One
ESD56	android-2.0.1_r1	Eclair	
ESD20	android-2.0_r1	Eclair	 
DMD64	android-1.6_r1.5	Donut	 
DRD20	android-1.6_r1.4		
DRD08	android-1.6_r1.3		
DRC92	android-1.6_r1.2		
```

### Figuring out which kernel to build 
FROM 
http://source.android.com/source/building-kernels.html

```
Device	Binary location	Source location	Build configuration
shamu	device/moto/shamu-kernel	kernel/msm	shamu_defconfig
fugu	device/asus/fugu-kernel	kernel/x86_64	fugu_defconfig
volantis	device/htc/flounder-kernel	kernel/tegra	flounder_defconfig
hammerhead	device/lge/hammerhead-kernel	kernel/msm	hammerhead_defconfig
flo	device/asus/flo-kernel/kernel	kernel/msm	flo_defconfig
deb	device/asus/flo-kernel/kernel	kernel/msm	flo_defconfig
manta	device/samsung/manta/kernel	kernel/exynos	manta_defconfig
mako	device/lge/mako-kernel/kernel	kernel/msm	mako_defconfig
grouper	device/asus/grouper/kernel	kernel/tegra	tegra3_android_defconfig
tilapia	device/asus/grouper/kernel	kernel/tegra	tegra3_android_defconfig
maguro	device/samsung/tuna/kernel	kernel/omap	tuna_defconfig
toro	device/samsung/tuna/kernel	kernel/omap	tuna_defconfig
panda	device/ti/panda/kernel	kernel/omap	panda_defconfig
stingray	device/moto/wingray/kernel	kernel/tegra	stingray_defconfig
wingray	device/moto/wingray/kernel	kernel/tegra	stingray_defconfig
crespo	device/samsung/crespo/kernel	kernel/samsung	herring_defconfig
crespo4g	device/samsung/crespo/kernel	kernel/samsung	herring_defconfig
```

Device projects are of the form device/<vendor>/<name>.
```
$ git clone https://android.googlesource.com/device/ti/panda
$ cd panda
$ git log --max-count=1 kernel
```
### Identifying kernel version in a particular system image
```
dd if=kernel bs=1 skip=$(LC_ALL=C grep -a -b -o $'\x1f\x8b\x08\x00\x00\x00\x00\x00' kernel | cut -d ':' -f 1) | zgrep -a 'Linux version'

For Nexus 5 (hammerhead), this can be accomplished with:
dd if=zImage-dtb bs=1 skip=$(LC_ALL=C od -Ad -x -w2 zImage-dtb | grep 8b1f | cut -d ' ' -f1 | head -1) | zgrep -a 'Linux version'

```

### download kernel source
Depending on which kernel you want,
```
$ git clone https://android.googlesource.com/kernel/common.git
$ git clone https://android.googlesource.com/kernel/x86_64.git
$ git clone https://android.googlesource.com/kernel/exynos.git
$ git clone https://android.googlesource.com/kernel/goldfish.git
$ git clone https://android.googlesource.com/kernel/msm.git
$ git clone https://android.googlesource.com/kernel/omap.git
$ git clone https://android.googlesource.com/kernel/samsung.git
$ git clone https://android.googlesource.com/kernel/tegra.git
```


 - The goldfish project contains the kernel sources for the emulated platforms.
 - The msm project has the sources for ADP1, ADP2, Nexus One, Nexus 4, Nexus 5, Nexus 6, and can be used as a starting point for work on Qualcomm MSM chipsets.
 - The omap project is used for PandaBoard and Galaxy Nexus, and can be used as a starting point for work on TI OMAP chipsets.
 - The samsung project is used for Nexus S, and can be used as a starting point for work on Samsung Hummingbird chipsets.
 - The tegra project is for Xoom, Nexus 7, Nexus 9, and can be used as a starting point for work on NVIDIA Tegra chipsets.
 - The exynos project has the kernel sources for Nexus 10, and can be used as a starting point for work on Samsung Exynos chipsets.
 - The x86_64 project has the kernel sources for Nexus Player, and can be used as a starting point for work on Intel x86_64 chipsets.
 
### Downloading a prebuilt gcc for kernel build
Ensure that the prebuilt toolchain is in your path.
```
$ export PATH=$(pwd)/prebuilts/gcc/linux-x86/arm/arm-eabi-4.6/bin:$PATH
OR
$ export PATH=$(pwd)/prebuilts/gcc/darwin-x86/arm/arm-eabi-4.6/bin:$PATH

git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.6
```
 
### Build kernel
As an example, we would build the panda kernel using the following commands:
```
$ export ARCH=arm
$ export SUBARCH=arm
$ export CROSS_COMPILE=arm-eabi-
$ cd omap
$ git checkout <commit_from_first_step>
$ make panda_defconfig
$ make

```

Or you can include the TARGET_PREBUILT_KERNEL variable while using make bootimage or any other make command line that builds a boot image.

```
$ export TARGET_PREBUILT_KERNEL=$your_kernel_path/arch/arm/boot/zImage

``` 

That variable is supported by all devices as it is set up via device/common/populate-new-device.sh
