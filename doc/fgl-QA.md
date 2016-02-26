问题列表

# 安装xposed

为了测试x86_32位版本的xposed。编译androidx86_32位版本（32位版本的实际使用的名字就是androidx86，后面不带_32），编译方法和前面的64版本一样，就是lunch时选择不同，这次编译也很顺利，就不详细说了。
在罗浩的提示下（感谢罗浩），编译参数改位efi_img（$make efi_img -j16）。
将编译出来的android_x86.img刻录到u盘：
$umount /dev/sdd1 (卸载u盘）
$dd if=[androidx86]/out/target/product/x86/android_x86.img of=/dev/sdd
用u盘在pc机器上安装，在第一个界面选择live cd启动（不安装，直接‘光盘’启动），报错，说找不到system.sfs，而查看实际的编译输出目录中是system.img。
在陈老师等的帮助下（感谢陈老师等），是因为编译时缺少squashsf-tools。故：
$sudo apt-get install squashfs-tools
$source build/env...
$lunch
$make efi_img -j16
这次生成的文件就变成了system.sfs。
重新制作U盘再试。