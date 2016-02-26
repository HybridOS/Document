# openthos进度表（2015108-20160115）

##总体目标
20160116之前，在skylake CPU/主板上稳定运行的kernel（x86-64），支持remix稳定版（x86-64），支持部分常用办公，生活，游戏软件和定制软件。

##任务分解

### recovery for windows APP
- [ ] 缺扩展界面设计实现，文档完善，测试等

#### owner:包钧圳，陈一璋，吴琼老师

#### 周进度
 - 20151224-20151231: ***
 - 20160101-20160107: ***
 - 20160108-20160114: *****

#### 细节
 - [x] libwim的android支持
 - [x] 功能设计实现
 - [x] 基本界面设计
 - [x] 测试文档
 - [x] 用户使用文档
 - [x] 设计文档
 - [x] win8功能测试 in android-x86-32+kernel 4.0.9 in old pc
 - [x] win8功能测试 in android-x86-64+kernel 4.0.9 in old pc
 - [x] win7/win8.1/win10功能测试 in android-x86-64+kernel 4.4-rc4 in skylake-cpu PC
 - [x] win7/win8.1/win10功能测试 in android-x86-64+kernel 4.4-rc6 in skylake-cpu PC
 - [x] win7/win8.1/win10功能测试 in android-x86-64+kernel 4.4-rc7 in skylake-cpu PC
 - [x] win7/win8.1/win10功能测试 in android-x86-64+kernel 4.4-final in skylake-cpu PC
 - [ ] 基于remix稳定版+kernel-4.4+进行测试
 - [x] 扩展界面设计实现(bootloader启动界面，openthos启动界面，openthos背景和图标)：吴琼老师


### APPs testing
- [ ] 缺remix+kernel 4.4测试,CTS测试等

#### owner: 刘明明

#### 周进度
 - 20151224-20151231: ****
 - 20160101-20160107: *****
 - 20160108-20160114: *****

#### 细节
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc4
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc6
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc7
 - [x] testing APPs on android-x86-64 lollipop+kernel 4.4-rc8
 - [ ] testing APPs on android-x86-64 lollipop+kernel 4.4-final
 - [x] testing APPs on remix-x86-64 lollipop+kernel 4.4-rc7+
 - [ ] testing APPs on remix-x86-64 + kernel 4.4-final
 - [x] testing/分类/分析 CTS on android-x86-64 lollipop+kernel 4.4-rc6+
 - [ ] testing/分类/分析 CTS on android-x86-64 lollipop+kernel 4.4-final
 - [ ] testing/分类/分析 CTS on remix-x86-64 lollipop+kernel 4.4-rc6+
 - [ ] testing/分类/分析 CTS on remix-x86-64 lollipop+kernel 4.4-final

### Android+kernel系统定制
- [ ] 缺remix-稳定版 + kernel 4.4 final，部分app无法运行，测试等

#### owner:罗浩 陈一璋

#### 周进度
 - 20151224-20151231: *****
 - 20160101-20160107: ****
 - 20160108-20160114: *****

#### 细节
 - [x] 配置/安装hodili
 - [x] 编译/定制/安装/运行rEfind
 - [X] 编译/定制/安装/运行uefi-supported/BIOS bootloader+linux kernel
 - [x] 编译/定制/安装/运行android-x86-32 lollipop+kernel 4.0.8
 - [x] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.0.8
 - [x] 编译/定制/安装/运行android-x86-32 lollipop+kernel 4.0.9
 - [x] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.0.9
 - [x] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.4-rc4
 - [x] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.4-rc4
 - [x] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.4-rc6
 - [x] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.4-rc7
 - [x] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.4-rc8
 - [ ] 编译/定制/安装/运行android-x86-64 lollipop+kernel 4.4-final
 - [x] 安装/运行/初步测试phoenixos，对比phoenixos/android-x86/remix
 - [ ] 安装/配置　gapps in 运行android-x86-64 lollipop+kernel 4.4-rc8+(？？缺proxy代理配置)
 - [ ] 安装/运行remix-x86-64 + kernel 4.4-final
 - [x] 撰写安装定制img说明文档
 - [x] 分析/fix（??还缺gapps或proxy代理) 部分APP（ ms-office(work),baiduyun(net)，youku(video),）无法运行

### kernel升级/维护
- [ ] 缺remix-稳定版 + kernel 4.4 final，部分app无法运行，测试等

#### owner:陈渝老师

#### 周进度
 - 20151224-20151231: *****
 - 20160101-20160107: ****
 - 20160108-20160114: *****

#### 细节
 - [x] 编译/定制/测试 kernel 4.0.8
 - [x] 编译/定制/测试 kernel 4.0.9
 - [x] 编译/定制/测试 kernel 4.1.0
 - [x] 编译/定制/测试 kernel 4.4-rc3
 - [x] 编译/定制/测试 kernel 4.4-rc4
 - [x] 编译/定制/测试 kernel 4.4-rc6
 - [x] 编译/定制/测试 kernel 4.4-rc7
 - [x] 编译/定制/测试 kernel 4.4-rc8
 - [x] 编译/定制/测试 kernel 4.4-final
 - [ ] 添加Broadcom 802.11a/b/g/n hybrid device driver
 - [x] 配置通用的config for kernel 4.4-final(基本通用)

### remix稳定版
- [ ] 缺remix-稳定版

#### owner:技德-战总

#### 周进度
 - 20151224-20151231: *
 - 20160101-20160107: ****
 - 20160108-20160114: ***

#### 细节
 - [x] 编译/定制/安装/运行/测试remix-x86-32 + kernel 4.0.9(without eng ver)
 - [x] 编译/定制/安装/运行/测试remix-x86-64 + kernel 4.0.9(without eng ver)
 - [x] 提供mesa lib相关的修改过的可执行动态链接库，帮助openthos解决了一个mesa lib相关的bug
 - [x] 编译/定制/安装/运行/测试remix-x86-64 + kernel 4.4-rc6
 - [x] 在20160111借了同方PC一台用于测试
 - [ ] 编译/定制/安装/运行/测试remix-x86-64稳定版+recovery win APP + kernel 4.4-final
 - [ ] fix bug for remix-x86-64稳定版+recovery win APP + kernel 4.4-final
 - [ ] 集成openthos界面(bootloader启动界面，openthos启动界面，openthos背景和图标)

### secure boot

#### owner:冯金阳

#### 周进度
 - 20151224-20151231: ***
 - 20160101-20160107: ****
 - 20160108-20160114: ****

#### 细节
 - [x] uefi secure boot分析
 - [x] dm-verity分析
 - [x] 设计实现方案(有一定进展)
 - [ ] 设计实现功能(有一定进展)
 - [ ] 功能测试
 - [ ] 使用说明文档

### 基于xposed的虚拟权限管理

#### owner:付贵龙

#### 周进度
 - 20151224-20151231: ***
 - 20160101-20160107: ***
 - 20160108-20160114: *****

#### 细节
 - [x] 编译/安装xposed
 - [x] 在模拟器中运行xposed
 - [ ] 分析xposed，写出分析文档
 - [ ] 在android-x86-5.1中跑起来xposed
 - [ ] 设计基于xposed的虚拟权限管理的实现方案
 - [ ] 实现功能
 - [ ] 功能测试
 - [ ] 在android-x86-6.0中跑起来xposed

### multi-win支持

#### owner:谢仲天，包钧圳

#### 周进度
 - 20160108-20160114: *****

#### 细节
 - [x] 分析现有multi-win设计实现
 - [x] 在物理机中运行打开并运行android-marshmallow的支持multi-win的功能
 - [x] 在模拟器中运行支持multi-win的android-4.4
 - [ ] 分析现有multi-win设计实现细节，写出分析文档
 - [ ] 在模拟器/物理机中运行支持multi-win的android-x86-4.4
 - [ ] 在模拟器/物理机中运行支持multi-win的android-x86-5.1
 - [ ] 在模拟器/物理机中运行支持multi-win的android-x86-6.0