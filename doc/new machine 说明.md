机器ssh 连接方式：

机器1：os@192.168.0.157

源码路径：~/Anroid-x86/

机器2：os@192.168.0.185

源码路径：~/android-x86-project/android-x86-5.1/

机器3：os@192.168.0.141

源码路径：~/android-related/android-source/

编译命令：

. /build/envsetup.sh

lunch

选择android-x86 or android-x86-64

make -j16 efi_img

镜像.img生成后，做启动u盘，直接插入u盘

umount /dev/sdb1(u盘的)

sudo if=/your/img/path/android_x86.img of=/dev/sdb