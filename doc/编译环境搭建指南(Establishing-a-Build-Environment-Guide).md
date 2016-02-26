### 编译AOSP

- AOSP的编译请查阅AOSP官方的[编译指南](http://source.android.com/source/initializing.html)。


### 编译Android-x86

- 编译前请参考Android-x86项目主页中有关[源码下载部分](http://www.android-x86.org/getsourcecode)的相关内容来获取所需要的源码。建议在获取对应版本AOSP源码的基础上进行进一步源码的获取，AOSP源码获取详见编译指南。

- Ubuntu 14.04环境下可通过安装下述软件包来构建编译环境（替换AOSP中Installing required packages步骤所提供的软件包）。

```
sudo apt-get install -y git-core gnupg flex bison gperf build-essential zip curl libc6-dev libncurses5-dev x11proto-core-dev libx11-dev libreadline6-dev libgl1-mesa-dev g++-multilib mingw32 schedtool tofrodos python-markdown pngquant libxml2-utils xsltproc zlib1g-dev libxext-dev openjdk-7-jdk gettext bc mtools lib32z1 lib32ncurses5 lib32bz2-1.0 python-pip libyaml-dev python-dev squashfs-tools
sudo pip install prettytable Mako pyaml dateutils --upgrade
```

- 另外编译Android-x86的编译命令语句，输出目标，输出结果等都与AOSP有着相当的不同，请查阅wiki中的一些Android-x86编译记录来获取更详细的编译指南。

**更多关于编译方面的问题欢迎向我们提问~！**