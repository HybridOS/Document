# Establishing a Build Environment

This section describes how to set up your local work environment to build the Android-x86 source files. You will need to use linux. We haven't tried to build it in MacOS and Windows is not currently supported.

## Setting up a build environment

These instructions apply to **LOLLIPOP-X86**

The Android-x86 build is routinely tested in house on recent versions of Ubuntu, but most distributions should have the required build tools available. Reports of successes or failures on other distributions are welcome.

### Installing required packages (Ubuntu 15.10)

 You will need a 64-bit version of Ubuntu. Ubuntu 15.10 is recommended.

 ```
sudo apt-get install -y git-core gnupg flex bison gperf build-essential zip curl libc6-dev libncurses5-dev x11proto-core-dev libx11-dev libreadline6-dev libgl1-mesa-dev g++-multilib schedtool tofrodos python-markdown pngquant libxml2-utils xsltproc zlib1g-dev libxext-dev openjdk-7-jdk gettext bc mtools lib32z1 lib32ncurses5 python-pip libyaml-dev python-dev squashfs-tools
sudo pip install prettytable Mako pyaml dateutils --upgrade
 ```

### Installing required packages (Ubuntu 14.04)

 You will need a 64-bit version of Ubuntu. Ubuntu 14.04 is recommended.

 ```
sudo apt-get install -y git-core gnupg flex bison gperf build-essential zip curl libc6-dev libncurses5-dev x11proto-core-dev libx11-dev libreadline6-dev libgl1-mesa-dev g++-multilib mingw32 schedtool tofrodos python-markdown pngquant libxml2-utils xsltproc zlib1g-dev libxext-dev openjdk-7-jdk gettext bc mtools lib32z1 lib32ncurses5 lib32bz2-1.0 python-pip libyaml-dev python-dev squashfs-tools
sudo pip install prettytable Mako pyaml dateutils --upgrade
 ```

