# 总体
===================
## 2016年1月需要完成的工作
### 基本内容
 1. 稳定运行的remix的稳定版android+可在skylake CPU/主板上稳定运行的kernel，都是x86-64版本 
 2. 在1.的基础上的部分常用办公，生活，游戏软件
   - 办公生活类
     - ms office
     - wps
     - skype
     - QQ
     - 微信
     - ES文件管理器
     - 今日头条
     - 豌豆荚
     - 优酷视频
     - QQ音乐
     - 搜狗输入法
     - QQ浏览器
     - chrome浏览器
     - 美图秀秀
     - 百度云
     - seafile
     - remix上能可靠运行的软件
     
   - 游戏类
     
     - 开心消消乐

     - 植物大战僵尸
     - 地铁跑酷
     - 水果忍者
     
   - benchmark类
     - 安兔兔
     
   - 定制类
     - recovery windows
     
  3. 在1.的基础上的特定软件
     - 系统恢复（可在android上恢复windows系统）
### 进展情况
  1. 可在skylake CPU/主板上稳定运行android-x86 5.1的kernel还没有，预计要花1～2周查找和修复问题
  2. 在一般PC上测试了android-x86-5.x (android(32位)，kernel（64位）),wps, skype, QQ，微信..
  3. 功能基本完成，缺界面设计（具体完成人月需要与吴琼老师协商），缺测试（大约3~4周）

# 细节
===================
## android 5.1/6 fork

### 计划
 - 同方版本, 基于remix的稳定版android+可在skylake CPU/主板上稳定运行的kernel
 - openthos, 2015年12月1日前，fork android 5.1，kernel是x86-64版本,android x86-32版本
 - openthos, 2016年1月31日前，fork android 5.1，都是x86-64版本
 - openthos, 2016年1月31日后，fork android 6，都是x86-64版本
  
### 备注
 - 目前的kernel还无法支持android在skylake cpu上正常运行
 - 在2016年6月前，同方版本，将基于remix的稳定版android+可在skylake CPU/主板上稳定运行的kernel(预计linux kernel 4.4), 都是x86-64版本
 - 将来openthos的版本将基于android的新版本后3个月内发布。
 
# 各个方面的改动 
## 桌面化 
 - 窗口管理器：强制横屏，多窗口支持，第一步支持窗口平铺，以及不支持横向使用的app适配 
 - 键盘热键调整（win键映射home，菜单键映射任务管理） 
 - launcher，考虑hybridlauncher (GPL版权) 
 - 任务管理器（与菜单键绑定）（mac osx的expose模式，win10的win+tab模式） 
 - 文件管理器：ms windows style，整合本地+网络文件访问（samba, nfs, fuse）+openthos云服务（见4.b）同步设置，可移动存储介质如U盘/SD CARD的挂载和卸载 
 - 任务管理器：资源监控和任务管理，整合系统优化（自启动，清缓存，清进程等），参考绿色守护和autostarts两个应用 
 - 显示管理器（第一步只支持双显示器mirror，需要一个管理工具） 
 - 系统裁剪：去掉通话记录、拨号器、短信、移动网络、流量管理等手机内容 
 - 网络设置模块：有线网卡+WIFI，更方便的proxy设置 
 - 通知中心，右侧滑入，无需手势，点击处理通知 
 - 隐藏导航条和顶部状态栏，底部改为launcher+通知中心 
 - 双启动：EFI bootmanager，固化启动选项，内置系统镜像和系统修复工具 
 - 实体键盘输入法优化,去除google拼音输入法，改为使用合作方的（搜狗？百度？） 
 - 多用户支持 
 - 多实例支持，例如同时运行多个word程序等 
 - 新安装程序图标默认桌面快捷方式 
 - 和技德合作 remix os 

### 计划
 - 同方版本, 依赖 remix的稳定版android+稳定版kernel的进展情况
 - openthos, 依赖 android-x86-5.1 OR android-x86-6.0,  估计2016年1月31日前，根据人手情况，对于部分基本不用编码的子项目，可完成。 
 - openthos, 依赖 android-x86-5.1 OR android-x86-6.0, 估计2016年1月31日后，fork android 6，支持多窗口
 - openthos, 依赖 android-x86-5.1 OR android-x86-6.0,  估计2016年1月31日后，预计2016年1月后，根据人手情况，涉及小规模编码的，初步估计2人月完成一个子项目。涉及中大规模编码的，初步估计6人月完成一个子项目。
### 备注
 - 多用户支持，已经在android-5.x+中支持
 - 显示管理器，已经在android-5.x+中有初步支持
 - 这都比较工程，部分子项目细节的工作量还不清楚（比如多显，多实例支持，裁剪等），需要有效的工作方式，推动对此的实现
 
## 安全性 
 - 快速SM2/3/4算法实现 
 - 信任链，目前安卓的设计已经相对比较完善了，主要涉及算法替换以及OEM密钥存取方式（改成通过TCM） 
    - SMS4的整盘加密（实际上是/data分区加密） 
    - 使用SM3来验证块设备的HASH 
 - 使用用户数据隔离的方式提供双环境（办公+上网）乃至多环境 
 - 应用程序权限，分类方法从强调手机改为强调桌面：用户信息、公共文件目录、网络、打印、自启动、通知绑定、账户等… 
 - 为每个应用的每个权限提供“许可/禁止/假装许可”模式，在国内的现实情况下，既能保证app的正常运行，又能保护用户隐私 
 - 双环境下可采取不同的缺省权限设置，“办公”为缺省“许可”，“娱乐”缺省为“假装许可” 
 - 应用程序签名，“办公”环境不允许自签名和未知来源的应用安装，“娱乐”环境只给其最低权限（其他权限全部为假装许可，稳妥的最小权限集合研究） 
 - 禁止不安全的ssl相关网络协议回退 
 - 权限编辑器app 

### 计划
 - 同方版本, 依赖 remix的稳定版android+稳定版kernel的进展情况, 估计2016年1月31日后，根据人手，预计12人月，预计涉及UEFI的security boot+ TCM, Android security subsystem-base-on-REMIX + verify disk data+ SElinux
 - openthos, 依赖 android-x86-5.1 OR android-x86-6.0, 估计2016年1月31日后， 根据人手，预计6人月，fork android 6，支持多窗口，采用android 6的security subsystem
### 备注
 - 问题： 目前清华方缺人手熟悉TPM/TCM, 同方的人对android security还不够了解。但这块很重要
 
## 适配：硬件 
 - 处理器架构： arm64(飞腾） 
    - mips64 (龙芯) 
    - x86-64 (intel) 
        - 集成libhoundini，适配微信等核心应用 
 - 电源管理模块:userspace的电源管理 
 - 显示器分辨率自动适配和手工定义（需要一个配置工具） 
 - AMD显卡+开源驱动，UVD硬件codec支持（好像android x86 5.1中已经实现） 
 - 键盘鼠标，synoptics触摸板的多点触控手势（android-x86中已经实现） 
 - NTFS，extFAT支持 
 - “自主可控”的打印机等外围硬件支持 
 - U盘，读卡器，刻录机等外设支持 
 - 作为USB HOST，对手机的支持，从手机、相机导入照片，通过手机上网 
 - 作为蓝牙SOURCE，支持A2DP，PAN 
 - 移除自身作为adb device的功能 
 - 与windows统一硬件时钟的时区定义 
 
### 计划
 - 同方版本, 龙芯：依赖 remix的稳定版android+稳定版kernel的进展情况，依赖基于龙芯的计算机/开发板，龙芯提供提供kernel源码支持和基本android支持。
 - 同方版本, 飞腾：依赖 remix的稳定版android+稳定版kernel的进展情况，依赖基于国防科大的计算机/开发板，国防科大提供提供kernel源码支持和基本android支持。 
 - 同方版本, 其他外设，根据人手情况，初步估计2人月完成一个子项目
 - openthos， android-x86-5.1 OR android-x86-6.0, 龙芯，飞腾，其他外设：与同方版本类似
### 备注
 - extFAT，已经在android-5.x+-x86中支持
 - NTFS，mount有bug
 - AMD显卡/显示相关，根据资料，是支持的，但有bug或不足的地方，比如不支持32bpp，只支持16bpp等
 - 电源管理，有bug，会无法唤醒
 - USB HOST,没有试过
 - 蓝牙，有基本支持
 - 打印机，应该需要写HAL
 - synoptics触摸板的多点触控手势，没有试过
 - 键盘鼠标，有基本支持
 - 基本思路，一个一个啃吧，但需要了解哪些是优先的，比如打印机/U盘？
 
### 适配：软件 
 - 系统级的云服务及其管理app，基于Seafile协议，实现用户指定目录的云端同步，系统和用户数据（通讯录、设置、app列表、输入法词库、浏览器书签/历史记录/密码、系统主题等等）的自动同步和备份/恢复（参考win8和icloud的功能） 
 - 作为发行版的软件环境预制和配置 
     - 图标库，来自总体VI 
     - 办公软件：ms office+onenote和wps 
     - 浏览器：firefox+adblock，OEM厂商书签预设，浏览器theme（总体VI） 
     - 电子邮件：考虑outlook.com 
     - 通信：微信/QQ 
     - 软件商店：play store是优先选择 
     - 地图：百度、高德等选一 
     - 娱乐和多媒体：媒体播放器（待选型），图片编辑器（待选型），照相机（AOSP） 
     - 工具：
        - 笔记（evernote）
        - 任务管理器（含绿色守护和自启动管理功能）
        - 权限管理器
        - console终端
        - busybox命令库
        - 系统还原
        - 文件管理器（remix）
        - 系统设置（remix）
        - 日历/计算器/时钟/通讯录（AOSP）
        - RDP远程桌面（microsoft）
        - VNC远程桌面（待选型） 
     - 壁纸和窗口装饰，来自总体VI 
 - Linux子系统，使用linux deploy部署一套标准的debian，使用android版SDL XServer，提供经裁剪的LINUX桌面，用于运行遗留业务和应用开发环境如：eclipse,libreoffice, firefox, chrome, php, mysql… 
 - Google框架，作为测试功能，先不对用户开放 

### 计划
 - 同方版本, 2016年1月前，依赖remix的稳定版android+稳定版kernel的进展情况
 - 同方版本, 2016年6月前，依赖 remix的稳定版android OR android-x86-6.0 +稳定版kernel的进展情况 
 - 同方版本, 其他工具/软件，2016年1月前，根据人手情况，涉及不用自己编码的软件，初步估计0.25人月完成一个子项目；需要简单修改的，初步估计0.5人月完成一个子项目；需要大改的，初步估计2人月完成一个子项目；
 - openthos，依赖  android-x86-5.1 OR android-x86-6.0, 其他工具/软件：与同方版本类似
 - 同方版本&openthos， 系统还原完成60%，缺UI和大量的部署测试。
### 备注
 - 图标库，来自总体VI，估计请美院吴琼老师，具体花费时间和人手需要与吴老师协商。
 
### 产品VI相关 
 - 产品的中英文名称、颜色、字体、LOGO设计 
 - 商标检索和注册 
 - 产品的整体外观风格设计：EFI启动管理器、启动画面、全局配色、第一方和第三方应用图标、桌面壁纸、主题、窗口管理器装饰、应用程序风格等 
 - 产品的文案和网站设计 

### 计划 
 - 估计请美院吴琼老师，具体花费时间和人手需要与吴老师协商。
