信息内容较老，建议看 [[progress-before-20160115]]

2015年10月---2016年1月（2015秋季学期结束）最终实现目标：
==========
1. Android可以在基于Intel Skylake架构的桌面CPU＋intel h110/h170主板上正常运行
1. Android/windows双系统启动
1. Android-x86提供windows系统恢复功能


人员安排（3学生+3工程师）：
=========
  - 预计开发人员（3工程师，1人主要负责测试+配置，1人开发linux+android-x86,1人开发android应用；3学生，负责技术攻关）
  - 前期需求分析，设计分析（1人+0.5月）
  - uefi+grub+refind配置（1人+0.5月）
  - 使用wimlib来实现系统还原的linux+android主要功能(1人+2月)
  - 生成并运行在现有真机（Thinkpad笔记本）的linux+android主要功能（1人+2月）
  - 生成并运行在Intel Skylake架构的桌面CPU＋intel h110/h170主板的linux+android主要功能（1人+2月）  
  - 测试分析（2人+1月）
  
风险评估：
===========
  - Linux驱动等对Intel Skylake架构的桌面CPU＋intel h110/h170主板支持不足。预案：请Intel工程师协助解决
  - android-x86 kernel中出现无法克服的bug。预案：请Android-x86 hacker+Intel工程师协助解决
  - android UI方面出现无法克服的问题。预案：请Android-x86 hacker和remix工程师协助解决。
  
初步安排：
==========
 1.将windows+linux通过uefi的形式可以正常双系统启动
 
 - 了解uefi引导启动的相关设置配置需要
   
   - 找一个已经编译好的efi的镜像（暂时不需自己从源码编译）
　    - 可以参考的开源的efi的网络列表？
   - 用qemu运行， qemu [-kvm-enable] -bios=编译好的efi的镜像 ....
      - 会用 （暂时不需自己从源码编译）refind：　http://www.rodsbooks.com/refind/index.html 
   - 在qemu上用efi.img, refind bootloader manager, linux/windows 可以正常运行
   - 在真机上重复qemu的工作
 
 - 相关分区的设置（esp分区中的文件内容,data分区共享等）
    
 - 对refind进行实际使用，在linux看恢复功能是否可以实现
 
 - 在qemu上进行实验，然后在真机上进行实验
  

 2.将android-x86进行配置修改等内容，并且使其可以与windows通过uefi引导启动
 
 - 理解android-x86 对android 4.4 and x86-64的修改，先从kernel开始分析　
    - 形成文档， 有三种文档， android-x86<-->android AOSP, android-x86<-->linux kernel, android AOSP <--> linux kernel
    - 理解修改的内容
    
 - 对android-x86源码进行编译，完成相关内核的配置
    - 源码编译，在qemu上运行android-x86 4.4
    - 源码编译，在真机上运行android-x86 4.4
    - 源码编译，在qemu上运行android-x86 5.1    
    - 源码编译，在真机上运行android-x86 5.1
    
 - 实现从uefi进行引导双系统正常启动
 
 - 完成相关数据共享区等的分配实现

 3.recovery app对恢复功能的实现（android-x86中）
 
 - 学习wimlib的设计实现和使用，来实现系统还原的主要功能
 - 在linux上实现一个命令行程序，通过mount-nfsd-fuse配合脚本, 使用wimlib来实现系统还原的主要功能，同时recovery　app要实现从OEM网站（URL可配置）下载系统wim文件并展开的功能
 - 在android上实现一个app通过mount-nfsd-fuse配合脚本实现自动mount ntfs分区的功能
 - 写一个简单的windows脚本/程序，在用户把bootmanager设定覆盖的情况下（一般是重装了windows）通知uefi缺省仍然启动refind，使用windows bcdedit命令或者api
 
 4. 阶段性系统测试
 - 对1进行测试
 - 对1,2进行测试
 - 对1,2,3进行联调测试
