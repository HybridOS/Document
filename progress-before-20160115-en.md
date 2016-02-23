# progress of openthos（2015108-20160115）

##overall goal
before 2016-01-16,  the kernel (x86-64) steady run on the skylake CPU/motherboard, support Remix stable version (x86-64), support some of the usual office software, life software, game software and custom software.

##Task disassembly

### recovery for windows APP
- [ ] lack of extended UI design & implement document, test

#### owner:包钧圳，陈一璋，吴琼老师

#### weekly progress
 - 20151224-20151231: ***
 - 20160101-20160107: ***
 - 20160108-20160114: *****

#### detail
 - [x] android libwim support
 - [x] functional design and implement
 - [x] UI design
 - [x] document of test
 - [x] user document
 - [x] document of design
 - [x] win8 functional test in android-x86-32+kernel 4.0.9 in old pc
 - [x] win8 functional test in android-x86-64+kernel 4.0.9 in old pc
 - [x] win7/win8.1/win10 functional test in android-x86-64+kernel 4.4-rc4 in skylake-cpu PC
 - [x] win7/win8.1/win10 functional test in android-x86-64+kernel 4.4-rc6 in skylake-cpu PC
 - [x] win7/win8.1/win10 functional test in android-x86-64+kernel 4.4-rc7 in skylake-cpu PC
 - [x] win7/win8.1/win10 functional test in android-x86-64+kernel 4.4-final in skylake-cpu PC
 - [ ] running test base on remix stable version +kernel-4.4+
 - [x] extended ui design & implement(bootloader start ui, openthos startup ui，openthos backgroud and icon)：吴琼老师


### APPs testing
- [ ] lack of remix+kernel 4.4 test, CTS

#### owner: 刘明明

#### weekly progress
 - 20151224-20151231: ****
 - 20160101-20160107: *****
 - 20160108-20160114: *****

#### details
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc4
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc6
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc7
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc8
 - [ ] testing APPs on android-x86-64 lollipop+kernel 4.4-final
 - [x] testing APPs on remix-x86-64 lollipop+kernel 4.4-rc7+
 - [ ] testing APPs on remix-x86-64 + kernel 4.4-final
 - [x] testing/classify/analyse CTS on android-x86-64 lollipop+kernel 4.4-rc6+
 - [ ] testing/classify/analyse CTS on android-x86-64 lollipop+kernel 4.4-final
 - [x] testing/classify/analyse CTS on remix-x86-64 lollipop+kernel 4.4-rc6+
 - [ ] testing/classify/analyse CTS on remix-x86-64 lollipop+kernel 4.4-final

### Android+kernel customization
- [ ] lack of remix stable version + kernel 4.4 final，part of app can't run and test

#### owner:罗浩 陈一璋

#### weekly progress
 - 20151224-20151231: *****
 - 20160101-20160107: ****
 - 20160108-20160114: *****

#### details
 - [x] configure/install hodili
 - [x] compile/customize/install/run rEFInd
 - [X] compile/customize/install/run uefi-supported/BIOS bootloader+linux kernel
 - [x] compile/customize/install/run android-x86-32 lollipop+kernel 4.0.8
 - [x] compile/customize/install/run android-x86-64 lollipop+kernel 4.0.8
 - [x] compile/customize/install/run android-x86-32 lollipop+kernel 4.0.9
 - [x] compile/customize/install/run android-x86-64 lollipop+kernel 4.0.9
 - [x] compile/customize/install/run android-x86-64 lollipop+kernel 4.4-rc4
 - [x] compile/customize/install/run android-x86-64 lollipop+kernel 4.4-rc4
 - [x] compile/customize/install/run android-x86-64 lollipop+kernel 4.4-rc6
 - [x] compile/customize/install/run android-x86-64 lollipop+kernel 4.4-rc7
 - [x] compile/customize/install/run android-x86-64 lollipop+kernel 4.4-rc8
 - [ ] compile/customize/install/run android-x86-64 lollipop+kernel 4.4-final
 - [x] install/run/test phoenixos， compare phoenixos/android-x86/remix different
 - [x] install/configure gapps on android-x86-64 lollipop+kernel 4.4-rc8+
 - [ ] install/run remix-x86-64 + kernel 4.4-final
 - [x] write document of install and customized img
 - [x] Analysis/fix part of APPs, NOW ms-office,baiduyun，youtub are OK!

### kernel upgrade/maintain
- [ ] lack of remix stable version + kernel 4.4 final，part of app can't run and test

#### owner:陈渝老师

#### weekly progress
 - 20151224-20151231: *****
 - 20160101-20160107: ****
 - 20160108-20160114: *****

#### details
 - [x] compile/customize/test kernel 4.0.8
 - [x] compile/customize/test kernel 4.0.9
 - [x] compile/customize/test kernel 4.1.0
 - [x] compile/customize/test kernel 4.4-rc3
 - [x] compile/customize/test kernel 4.4-rc4
 - [x] compile/customize/test kernel 4.4-rc6
 - [x] compile/customize/test kernel 4.4-rc7
 - [x] compile/customize/test kernel 4.4-rc8
 - [x] compile/customize/test kernel 4.4-final
 - [x] add Broadcom 802.11a/b/g/n hybrid device driver
 - [x] configure general config for kernel 4.4-final

### remix stable version
- [ ] lack of remix stable version 

#### owner:技德-战总

#### weekly progress
 - 20151224-20151231: *
 - 20160101-20160107: ****
 - 20160108-20160114: ***

#### details
 - [x] compile/customize/install/running/test remix-x86-32 + kernel 4.0.9(without eng ver)
 - [x] compile/customize/install/running/test remix-x86-64 + kernel 4.0.9(without eng ver)
 - [x] Provide a modified executable dynamic link library of mesa lib, to help openthos solve a lib bug
 - [x] compile/customize/install/running/test remix-x86-64 + kernel 4.4-rc6
 - [x] 在20160111借了同方PC一台用于测试
 - [ ] compile/customize/install/running/test remix-x86-64 stable version +recovery win APP + kernel 4.4-final
 - [ ] fix bug for remix-x86-64稳定版+recovery win APP + kernel 4.4-final
 - [ ] integrate openthos ui(bootloader boot ui，openthos boot ui，openthos backgroud and icon)

### secure boot

#### owner:冯金阳

#### weekly progress
 - 20151224-20151231: ***
 - 20160101-20160107: ****
 - 20160108-20160114: ****

#### details
 - [x] uefi secure boot analysis
 - [x] dm-verity analysis
 - [x] design implement plan(have some progress)
 - [ ] design implement function(have some progress)
 - [ ] function test
 - [ ] user document

### Virtual rights management based on xposed

#### owner:付贵龙

#### weekly progress
 - 20151224-20151231: ***
 - 20160101-20160107: ***
 - 20160108-20160114: *****

#### 细节
 - [x] compile/install xposed
 - [x] run xposed in qemu
 - [ ] analysis xposed，write document of analysis
 - [ ] run xposed on android-x86-5.1
 - [ ] Design the implementation of the virtual rights management based on xposed
 - [ ] implement function
 - [ ] functional test
 - [ ] run xposed on android-x86-6.0

### support of multi-win

#### owner:谢仲天，包钧圳

#### weekly progress
 - 20160108-20160114: *****

#### details
 - [x] Analysis of existing multi-win design and Implementation
 - [x] Run android-marshmallow in the physical machine to open and run multi-win to support the function
 - [x] Running android-4.4 in the simulator to support multi-win
 - [ ] Analysis of the existing multi-win design and implementation details, write analysis of the document
 - [ ] running android-x86-4.4 in the physical machine/simulator to support multi-win
 - [ ] running android-x86-5.1 in the physical machine/simulator to support multi-win
 - [ ] running android-x86-6.0 in the physical machine/simulator to support multi-win