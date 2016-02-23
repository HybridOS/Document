
##源码版本

本文档基于对陈老师下载的Android-x86-4.4版本及内核进行编译的过程撰写。其余版本不保证效果。


##环境搭建

Android-x86的编译在Android编译的基础少还需要一些其余工具。编译过程中若缺少工具会得到错误提示，根据提示对工具进行补全即可。另外Android需要Oracle或者Sun版本的JDK1.6进行编译。其他版本的JDK均不可用。


##事前准备

将源码进行解压。不知是否源码问题，我初次编译系统在提示工具不全的同时也提示了对在kernel目录下执行make mrproper指令进行目录清空。之后的具体步骤可以参考http://www.android-x86.org


##编译

和官方android编译类似，首先执行
source build/envsetup.sh

之后执行lunch命令选择编译的目标版本。

之后执行编译指令
make iso_img -jX
*根据对bootable/newinstall目录下Android.mk文件的考察，理论上可以通过使用efi_img替换iso_img使得生成的镜像具备efi启动支持。这点还没有进行尝试。
*如果已经有编译好的内核或者想要对内核编译的config进行更改则可以使用不同的指令来规定这些内容。具体可以参考Android-x86网站上kernel custom部分

##制作U盘启动
可以使用Android-x86上提供的工具。也可以使用工具rufus，两者相比前者烧录速度更快，后者提供烧录DD镜像的选项。另外按照Android-x86官网的叙述，使用Linux中的DD工具直接烧录镜像也是可以的，可以酌情选择方法。


##遇到的问题

-编译内核

编译内核的时候遇到了以下问题：

1. 编译 drivers/gpio/gpio-valleyview2.c　出错
解决方法：
#include <gpiolib.h>
修改为
//#include <gpiolib.h>
#include "gpiolib.h"

2. drivers/net/wireless/brcm80211/brcmfmac/内多个文件的编译错误
解决方法：错误发生原因在于sdio.h这个文件与其他文件版本不符。其他文件使用的均为Linux4.1.x版本的定义内容，但该文件仍保持Linux4.0.8版本的内容。通过查阅源码，将该文件与Linux4.1版本的文件进行对比，将内容进行修改使之与4.1版本保持一致即可编译通过。

-生成ISO

安装 squashfs-tools可以减少iso size
sudo apt-get install squashfs-tools

我一共进行了三次ISO的编译最初一次使用了编译好的kernel并使用prebuilt命令对kernel进行指定并编译。生成的ISO无法进入live系统，也无法进行安装。初步判断是kernel的问题。

第二次编译使用了android-x86_usr版本作为编译目标，生成的ISO镜像可以进入系统和进行安装。但不管是live系统还是安装好的系统均在进入系统后即呈现无响应状态，任何按键均无反应。初步判断为编译目标选择上出了问题。

第三次在对编译系统进行彻底初始化（在主目录使用make clean，在kernel目录使用make mrproper）后通过一次make完成对系统和kernel的编译并以android-x86_eng为目标生成iso。这次生成的iso在安装和live系统上都没有问题，安装完成的系统除去部分部件无法驱动大多数功能都能够实现，今后会对make成功和失败的原因中再作探讨。
