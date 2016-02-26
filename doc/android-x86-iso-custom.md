## 镜像定制

对镜像的定制主要是对install.img文件和system.sfs文件的定制。在Windows下获取这两个文件的方法是通过7zip或UltraIso等软件打开编译好的img镜像并提取，linux下可以通过mount编译好的img镜像来提取，下面分别介绍对这两文件的定制方法。
（*本文中有一部分内容涉及源码中的bootable/newinstaller/Android.mk文件，不明之处可以看之前我写过的对该文件的初步分析）

#### system文件定制

--1.system.sfs文件修改方法

在修改system.sfs文件之前我们先说一下sfs文件格式。sfs文件格式是一种高度压缩的文件系统格式，system文件会保存为该种格式的原因是因为android-x86编译系统检测到系统中的/bin文件夹中存在着mksquash命令。该命令是用来生成sfs文件的命令。

通过对源码中/bootable/newinstaller/Android.mk文件的查阅我们发现android-x86编译系统生成的sfs文件是直接将system.img文件压缩而成，因此即便我们将sfs文件进行mount，也只能够看到system.img文件。外加sfs文件是只读文件，要进行修改我们只有一种方法：

	解压.sfs文件为.img文件->mount并修改->umount->重新压缩为sfs文件（若不umount会出现文件系统格式错误）->替换镜像中的system.sfs文件。

下面我们分别对上面提到的步骤进行具体操作的讲解：

---解压.sfs文件

sfs文件系统的全名为squashfs，为了解压该文件我们需要安装对应的工具，在ubuntu中可以通过以下方代码来进行安装
	
	sudo apt-get install squashfs-tools

安装好后系统会增加“mksquashfs”和“unsquashfs”两个可执行文件。通过文件名我们不难看出这两个文件一个是用来进行压缩，另一个是用来进行解压。因此我们只需要使用如下命令解压system.sfs文件杰克

	unsquashfs system.sfs

之后在system.sfs所在文件夹中会出现一个名为squashfs-root的文件夹，该文件夹中便是我们需要的system.img文件。

---mount system.img文件并进行修改

通过ubuntu系统自带的mount功能，我们可以通过一下语句将system.img以rw形式mount到一个我们建立好的文件夹。
	
	mount -rw system.img MOUNTPATH/

之后便可以直接对system.img中的内容进行修改。需要注意的是system.img的文件大小是固定的，增加文件时若超过该大小则会出现问题，修改时请根据需要对system.img内的文件进行适当的操作。

---解除mount

修改完后在重新压缩前我们需要对该镜像进行mount的解除，具体语句如下

	umount MOUNTPATH/

之后便可进行压缩。

---重新压缩为sfs文件

确保mount解除后，进入到存放system.img文件的目录，执行如下语句

	mksquashfs system.img system.sfs -comp xz

这条语句的含义为将system.img文件通过xz工具来压缩为sfs文件system.sfs。执行完毕之后该目录下就会出现我们修改过后的system.sfs文件，通过最初提到的7zip等工具将其替换到编译好的镜像中即可完成定制。


--2.system文件定制内容

---APP定制

目前对APP定制主要通过直接修改编译好的iso文件中的system.sfs文件来进行，对system.sfs文件的修改方式将在前面的部分介绍过了，在此只介绍增减APP的方法。

----增加SystemRecovery.apk到系统app

首先进入mount好的system分区，找到app文件夹，进入该文件夹后可以看到该文件夹中有很多文件夹，内容分别是各种预装APP和执行APP所必须的其他内容。鉴于SystemRecovery并不是一个系统APP，也并不需要lib文件夹对其运行进行支持，因此我们只需要新建一个文件夹，将其改名为任意名称（预装后的APP在APP菜单中显示的名称不由此文件的名字决定，也不由apk文件的文件名决定因此任意命名即可），之后将SystemRecovery.apk文件移动到此文件夹中即可。

----删除无用APP

删除的方法也很简单，只需要直接删除认为无用的APP文件夹即可，需要注意的是某些APP是系统必须的，对不明功能的APP在删除之前最好确认一下其不可或缺性。另外，一些APP之间存在关联（如Email和Exchange2），删除其中一个其余的便会失去工作能力，删除前还请认真确认。

---壁纸定制

包含默认壁纸的文件为framework/framwork-res.apk文件。为了修改我们需要通过7zip工具来打开该文件。打开之后我们能够发现res\drawable-sw720dp-nodpi-v13\和res\drawable-sw600dp-nodpi-v13\这样两个目录。这两个目录中的default_wallpaper.jpg文件便是android-x86的默认壁纸。具体使用的是那张壁纸会根据设备而定，在跑过未修改壁纸的系统之后我们便可以知道自己需要替换的文件，之后将大小合适的文件进行替换即可修改默认壁纸。

---将wimlib执行程序加入/bin文件夹

这里的操作和增加APP时一样，直接将wimlib执行文件置入该文件夹中即可。需要注意的是由于wimlib通过静态库方式编译，该执行文件较大，前面我们提到过system.img镜像的大小是固定的，上限不可超越，因此在新增该执行文件之前，请务必将用不到的一些app进行删减以增加空间。这样可以省去重新对img打包的过程，减少不必要的麻烦。




#### install文件定制

--1.install文件的修改方法

从扩展名上来看，install文件似乎是img文件，但通过file指令来查看则可发现install文件是gzip压缩文件。这一点也通过查看源码中的bootable/newinstaller/Android.mk文件得到了证实。解压后通过file指令可以得知该文件为CPIO压缩文件。由于CPIO压缩文件不是一个文件系统，无法通过mount进行修改，因此我们修改install.img的步骤也就初步定为定位：
	
	修改扩展名为.gz并解压->通过CPIO指令来解压->对需要修改的文件进行修改->通过CPIO指令来压缩->通过gzip压缩为.gz文件->修改扩展名为.img并替换

上述步骤看似完美，但实际上却存在着一个严重的问题。android编译系统中是使用自带的工具来进行CPIO格式文件的压缩的。因此我们要想确保修改后的install文件能够正常工作，也必须要使用android编译系统自带的工具进行压缩。因此上述步骤就变为：

	获取android自带CPIO压缩工具->修改扩展名为.gz并解压->通过CPIO指令来解压->对需要修改的文件进行修改->通过压缩工具来压缩->通过gzip压缩为.gz文件->修改扩展名为.img并替换

下面我们详细讲解上述步骤的每一个过程：


---获取android自带CPIO压缩工具：

android编译系统中用来压缩CPIO文件的工具主要有gen_init_cpio和gen_initramfs_list.sh。其中前者用来压缩CPIO文件，后者用来声称前者压缩需要的文件列表。首先我们来看一下如何获取这两个工具

gen_init_cpio工具的源码位于kernel/usr/目录下，想要获取该工具可以通过局部编译。首先进入kernel文件夹内，执行

	make android-x86_defconfig

来确定.config文件并使得内核可以编译。之后执行

	make usr/

来编译usr目录下的代码。完成后进入usr目录即可看到编译好的可执行文件gen_init_cpio，将其拷贝至工作目录备用即可。而gen_initramfs_list.sh文件位于kernel/scripts/目录内，我们可以通过如下代码

	make scripts/

来局部编译该目录，编译完成后进入该目录即可看到gen_initramfs_list.sh文件，同样将其拷贝到工作目录备用。


---解压install.img文件

由于install.img文件是gzip压缩文件，但扩展名并不为gz使得gzip无法直接解压，因此我们首先将其扩展名改为.gz，之后通过gzip解压即可得到一个CPIO文件。解压CPIO文件只需使用系统自带的cpio工具即可（系统不自带该工具则请安装该工具或类似工具后再继续）。具体代码如下：

	mkdir newinstallimg		#在工作目录下新建文件目录
	cp install.img newinstallimg/	#将install.img文件移动到文件目录中
	mv install.img install.img.gz	#修改文件扩展名
	gzip -d install.img.gz		#gzip解压
	cpio -i < install.img		#cpio解压
	rm install.img			#删除install.img，只在文件目录中保留文件

之后我们便可以看到newinstallimg文件夹内出现了install.img中的所有内容，按照需要进行修改即可。


---重新打包install.img文件

修改过后，我们需要将修改后的文件制作成新的install.img文件来使用。首先，我们需要使用gen_initramfs_list.sh工具获取需要打包的文件列表并保存。之后，通过gen_init_cpio工具将文件列表中的文件打包为CPIO文件。由于镜像中使用的install.img实际上是一个gzip文件，通过对源码中bootable/newinstaller/Android.mk进行查看，我们得知编译系统在制作install.img文件时所采用的压缩语句为gzip -9。因此我们也采用相同的方法来进行压缩，压缩之后别忘了将扩展名改为对应的.img，以便镜像使用。具体代码如下：

	./gen_initramfs_list.sh newinstallimg/ > filelist	#将newinstallimg文件夹中修改后的文件列入filelist
	./gen_init_cpio filelist > newinstall.img		#将filelist中的文件制作为CPIO文件
	gzip -9 newinstall.img					#对新做好的install.img进行压缩
	mv newinstall.img.gz install.img			#修改文件名以便时候使用

这里需要注意的是最后一步之前要确保原本的install.img文件不在工作目录内，否则可能会出现错误。

--2.install.img文件的定制内容

由于需要通过rEFInd启动安装后的android-x86系统，因此我们需要将启动项写入rEFInd的配置文件中。另外，由于只是用UEFI模式启动，我们不需要安装grub，而为了在特殊情况下能够启动，我们需要进行grub2的安装。为了达到这一目的，我们需要修改install.img中的scripts/1-install文件。同时将一个作为rEFInd启动菜单下android-x86标志的图标加入到新增的icon文件夹中。增加icon文件较为简单不再详细阐述，在这里详细说明一下对1-install文件的修改细节。

根据对1-install文件的阅读，发现对android系统进行安装的具体指令和内容全部包含在install_to()函数中。从用户角度考虑，我们希望尽可能少的让用户去进行选择，避免误操作。因此我们删去了对grub和grub安装与否的选择项。并默认加入了对rEFInd配置项写入的代码。具体代码如下（其中hd目录为esp分区的mount目录）：

	if [ -e /hd/EFI/refind/refind.conf ]; then										#判断是否存在rEFInd
        echo -e "menuentry \"Android-x86-Recovery\"{\n\ticon /EFI/refind/icons/os_android-x86.png\n\tvolume \"Android-x86-R\"\n\tloader /$asrc/kernel\n\tinitrd /$asrc/initrd.img\n\toptions \"root=volume androidboot.hardware=android_x86 SRC=/$asrc\"\n}" >> /hd/EFI/refind/refind.conf	#写入rEFInd中android的启动项配置
       	cp /icon/os_android-x86.png /hd/EFI/refind/icons/									#复制icon文件
     	fi

其中rEFInd的配置项文件写入后具体内容如下：
	
	menuentry "Android-x86-Recovery"{
		icon /EFI/refind/icons/os_android-x86.png
		volume "Android-x86-R"
		loader /主文件夹/kernel
		initrd /主文件夹/initrd.img
		options "root=volume androidboot.hardware=android_x86 SRC=/主文件夹
	}
	
修改后的install文件安装中只会询问格式化与否和system目录是否可读写化，安装后重启进入rEFInd启动菜单可以看到grub2启动项和android-x86启动项。可以根据实际情况选择方式启动系统。
