#android-x86 ISO生成原理和步骤初步分析


##生成原理

根据对bootable/newinstaller/目录下的Android.mk文件的阅读，android-x86的ISO镜像主要由以下几部分构成：

- 启动引导文件夹boot（内含efi文件夹和isolinux文件夹）
  - efi文件夹：内含boot文件夹，在boot文件夹内含有一个grub.cfg文件。疑似是efi启动方式的grub引导。其具体内容可能与生成目标的选择（iso_img或efi_img）相关
  - isolinux文件夹：内含安装界面相关文件，用于生成安装界面的图形和逻辑。

- ramdisk.img

- initrd.img

- install.img

- 系统内核kernel：即编译过程中通过源码编译生成的kernel

- 系统镜像system.img(或system.sfs具体扩展名取决于是否使用squashfs）：根据Android.mk文件中的内容，其中规定了一个变量用来标识是否使用了squashfs，若使用了squashfs会将系统img压缩的相当小，其扩展名也会改变为sfs，若未使用则会相对大一些，扩展名为img。


##生成步骤

既然知道了ISO里面的内容，对生成步骤的考量就简单了一些。通过对Android.mk文件的阅读，我们能够发现这样一个大致的ISO生成步骤：
- 生成开始前，这些内容已经通过编译产生
  - 系统镜像（system.img或者system.sfs)
  - 用来封装成initrd.img的全部内容
  - 用来封装成install.img的全部内容
  - ramdisk.img文件
  - boot文件夹内的全部内容
  - 系统内核文件kernel（或是指定好的prebuilt kernel）

- 生成过程
  - 第一步先确定系统镜像的位置
  - 之后将initrd.img压制完成
  - 将install.img压制完成
  - 确定boot文件夹内的内容
  - 将上述内容设置为ISO_IMAGE的依赖文件
  - 确定生成目标的文件名
  - 根据所选目标进行镜像文件的生成（生成iso文件或者支持efi的img文件）

- 生成方法
iso的生成采用了genisoimage指令。由于对该指令的细节尚不是很了解，在这里首先贴上成功编译的记录文件中对该指令的记录
```bash
genisoimage -vJURT -b isolinux/isolinux.bin -c isolinux/boot.cat \
	-no-emul-boot -boot-load-size 4 -boot-info-table \
	-input-charset utf-8 -V "Android-x86 LiveCD" -o out/target/product/x86/android_x86.iso out/target/product/x86/boot out/target/product/x86/ramdisk.img out/target/product/x86/initrd.img out/target/product/x86/install.img out/target/product/x86/system.img out/target/product/x86/kernel
```

根据对该指令的初步查阅，该指令具体内容如下：
+ -vJURT为生成属性设置：
  + -v为打印出生成细节
  + -J为使用Joliet格式的目录与文件名称
  + -U为允许untranslated的文件名
  + -R为使用使用Rock Ridge Extensions
  + -T为生成TRANS.TBL文件

    上述生成设置使用的详细原因还有待查阅。
+ -b为选择isolinux/isolinux.bin作为开机映像的文件
+ -c为选择将全部的eltorito-catalog内容只作为isolinux/boot.cat文件。
+ -no-emul-boot为标识系统非软盘
+ -boot-load-size 4根据官网提示，是用来防止在部分bios系统下启动失效故将此变量设置为4
+ -boot-info-table 对启动文件优化，直接将其复制即可启动
+ -input-charset utf-8 将镜像内文件名的字符编码设置为utf-8
+ -V "Android-x86 LiveCD" 写入镜像卷标为Android-x86 LiveCD
+ -o 设置镜像的输出位置

根据genisoimage的具体语法规则，在设置好镜像输出位置后需要依次输入要写入镜像的文件。因此在这之后的各个文件目录均为镜像内容，这也与我们实际得到的镜像内容相符合（其中TRANS.TBL文件来自genisoimage命令的-T指令）。


##今后工作
由于对Android.mk文件的具体编写细则还不够了解，今后目标为进一步查阅资料并完整理解Android.mk文件编写目的和详细生成过程，并对该文档进行补完的同时进一步分析ISO生成的关键点。
