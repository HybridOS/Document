#windows8.1+ubuntu15.04+android-x86(4.4-r2)

准备工作:

在bios中开启uefi

简介一下UEFI+GPT模式的启动原理，首先我们来回顾下BIOS引导MBR分区的流程，BIOS开机自检——》读取硬盘MBR分区的主引导记录—》控制权交给引导程序-》引导程序根据安装时候的配置读取各分区记录—》根据各分区已经有的系统情况，列出启动目录—》根据用户选择，启动选择的引导文件启动用户选择的系统。
现在我们来说说UEFI的情况，主板上的UEFI模块—》硬盘内的第一个fat分区，如果分区内有EFI这个文件目录，就根据EFI文件目录的引导文件加载各类型的驱动和引导文件，启动系统同时完成自检。（如果第一个fat分区没有EFI目录则选择第二个，如果第一块硬盘没有，择选择第二块，或者U盘以此类推），当然，UEFI的实际启动过程，并不像我说的这么简单，我这里也仅仅是简单描述下跟传统BISO引导的不同地点而已。


####1. 安装windows8.1

	u盘启动安装windows8.1（使用UltraISO做启动盘），在开始点击安装之前按shift+f10

	输入如下：
	diskpart
	list DISK       ##列出当前计算机上的所有物理磁盘
	select disk x   ##选择你要操作的物理磁盘。其中“X”代表磁盘的编号。
	clean           ##清除物理磁盘所有信息
	convert gpt     ##将磁盘转换为GPT格式
	create partition efi size=500    ##创建efi分区，size是分区大小（这里假定500m）
	create partition msr size=128    ##创建MSR分区，微软默认大小是128M
	list partition  ##列出磁盘上的分区
	exit            ##退出

继续安装win8，之后在创建分区的时候可以看到已经按照我们的要求建立好efi以及msr分区，之后继续安装，注意留下空间来安装ubuntu以及android-x86

####2. 安装ubuntu15.04
	
Unbuntu15.04的发行版ISO，已经支持UEFI模式安装了，所以，我们还是选择最简单的办法就是写到u盘或者是光盘安装.开始一路NEXT，到选择安装的时候仍然选择其他选项。

注意接下来出现自定义安装界面，分区安排还是跟ubuntu正常安装的时候差不多，只是多了两个分区而已（编号方法仍旧是/dev/sda12345），值得注意的是，我们现在在选安装启动器的设备位置时，选择/dev/sda1（我们之前创建的efi分区） 不要遗漏数字1，这样就是把Ubuntu的EFI启动信息安装到我们的ESP启动分区上了

安装结束后，重启默认会优先启动ubuntu中的grub，可以通过它来选择进入ubuntu或者win8系统

####3. 安装android-x86(4.4-r2)

因为目前android-x86还不支持uefi直接启动安装，在这里我们采用的方法是先将bios修改为传统的模式，将android-x86安装之后，再bios改回uefi，之后通过修改ubuntu中grub的方式进行android-x86的启动。

首先先将bios改回传统bios模式，之后使用u盘启动安装（注意这里做启动盘使用PowerISO来进行制作，使用UltraISO制作可能会出现安装问题）。

进入安装界面，选择安装的磁盘的位置（正常可以创建指定大小的磁盘来进行安装），注意由于目前android-x86并不支持在gpt的磁盘下进行安装，在这里我们插入另一个u盘，将android-x86安装到u盘中，其他安装过程基本不用改变，注意在最后会询问是否安装grub，在这里我们选择no。安装好之后我们可以正常启动android-x86。

之后我们修改bios变回之前uefi启动，这时无法通过之前的方式启动android-x86，只能启动win8或者ubuntu,所以我们需要修改ubuntu中的grub来进行引导，并且将我们的u盘内容放入到指定的磁盘中，从而避免需要u盘进行android的启动（之后要改写启动菜单）。

我们在这里的思路是是安装android时不安装grub，然后在grub添加对安卓系统的引导。


#####3.2 4.4-r3UFI安装

根据Android-x86官网提供的镜像下载列表我们可以找到支持UEFI安装方式的android-4.4-r3.img版本镜像。该镜像为DD镜像，在Windows使用UltraISO是无法烧制DD镜像的。在此我使用的是一款名为rufus（https://rufus.akeo.ie/）的镜像制作软件。该软件会根据所选镜像文件的内容进行不同的烧制方式，能够很简单的制作DD镜像。镜像制作完毕后无需修改启动方式，在UEFI模式下即可进行安装。




在grub添加安卓系统的启动项：

这个需要在linux系统下操作。下面以ubuntu为例。打开终端，输入sudo gedit /etc/grub.d/40_custom
40_custom是允许用户自由添加启动项的文档。
文档开头已经有这样的内容

	#!/bin/sh
	exec tail -n +3 $0
	# This file provides an easy way to add custom menu entries. Simply type the
	# menu entries you want to add after this comment. Be careful not to change
	# the 'exec tail' line above.

我们要在下面添加

	menuentry "Android-x86 4.4.2 RC2" {
	set root=(hd0,7)
	linux /android-4.4-RC2/kernel quiet root=/dev/sda7 androidboot.hardware=android_x86 video=-16 SRC=/android-4.4-RC2
	initrd /android-4.4-RC2/initrd.img
	}
不要照抄，上面有几个项目需要根据你的情况改动。

第一行：“Android-x86 4.4.2 RC2”，这个随便写，你想在grub显示什么就输入什么。

第二行：“hd0,5“，这个是你安装安卓系统的分区。举例：该分区在linux系统下识别为sda”x“（x是数字），那么你就在这里写上hd0,x（网上有的教程说grub识别分区是从hd0,0开始的，所以对应的要减一，我第一次照做，结果grub提示我分区不正确。。）。该分区若在linux下识别为sdb”x“，那么你就在这里写上hd1,x，以此类推。

第三行：”sda7“，同样的，是你安装安卓系统的分区。

第三四五行：android-4.4-RC2，这个是你安卓系统安装目录的文件夹名。请找到你对应的安卓系统的目录名，将其代替。
编辑完保存，回到终端。输入下面的内容更改grub设置：sudo grub-mkconfig

待终端处理完毕，接着输入：sudo update-grub 完工。重启电脑，就能看到安卓的启动项出现。

在这里我简单将u盘中的内容放到/boot目录（也可以新建分区进行放置，原理相同）

我在修改后的内容如下：


	#!/bin/sh
	exec tail -n +3 $0
	# This file provides an easy way to add custom menu entries.  Simply type the
	# menu entries you want to add after this comment.  Be careful not to change
	# the 'exec tail' line above.

	menuentry "Android-x86 4.4.2 rc2"{
	set root='hd0,gpt4'
	linux /boot/android-x86/android-4.4-r2/kernel quiet root=/dev/sda4 androidboot.hardware=android_x86 
	video=-16 SRC=/boot/android-x86/android-4.4-r2
	initrd /boot/android-x86/android-4.4-r2/initrd.img
	}

