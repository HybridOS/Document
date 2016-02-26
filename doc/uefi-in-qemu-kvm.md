UEFI in qemu/kvm
================
From 
https://wiki.ubuntu.com/UEFI/EDK2
and 
https://wiki.ubuntu.com/UEFI/OVMF
--------------------------------

Install required packages
-------------------------
sudo apt-get install build-essential git uuid-dev iasl nasm


Get the latest source for EDKII
-------------------------------
$ mkdir ~/src
$ cd ~/src
$ git clone git://github.com/tianocore/edk2.git
$ cd edk2


Compile base tools
------------------
$ make -C BaseTools


Set up build environment
------------------------
$ . edksetup.sh



Set up build target
-------------------

$ vi Conf/target.txt

Find

 ACTIVE_PLATFORM       = Nt32Pkg/Nt32Pkg.dsc 

and replace it with (for MdeModulePkg module package)
1) 第一次
 ACTIVE_PLATFORM       = MdeModulePkg/MdeModulePkg.dsc 
2）第二次
then for full system firmware image (OVMF)
 ACTIVE_PLATFORM       = OvmfPkg/OvmfPkgX64.dsc 

Find

 TOOL_CHAIN_TAG        = MYTOOLS 

and replace it with your version on GCC here for example GCC 4.6 will be used.

 TOOL_CHAIN_TAG        = GCC49

Find

 TARGET_ARCH           = IA32 

and replace it with 'X64' for 64bit or 'IA32 X64' to build both architectures.

 TARGET_ARCH           = X64 
 
Building MdeModulePkg module package
----------------------------------- 
1) 用第一次 配置
$ build

Build a full system firmware image (OVMF)
--------------------------------------
2) 用第二次 配置
$ build

If you'd like debug output on the serial console, use the DEBUG_ON_SERIAL_PORT option:

$ build -DDEBUG_ON_SERIAL_PORT=TRUE



running uefi
---------------------
get qemu-system-x86_64， then
qemu-system-x86_64 -bios .edk2/Build/OvmfX64/DEBUG_GCC46/FV/OVMF.fd
就可以看到是用uefi启动的了。
