##2016-01-13工作内容
1.找到了编译过程中生成system.img文件的语句位置，进行了修改尝试并成功。方便了对system镜像的扩充。

2.尝试了陈老师说的FlashGordon刷写opengapps，FLashGordon无法安装（卡在install），原因未知。

##2016-01-12工作内容
1.修改了install.img，使之能够正常安装在skylake-based CPU的机器上。

2.探查了refind图标修改的方法和grub2的安装细节。

##2016-01-11工作内容
1.使用4.4-final-openthos作为kernel编译了新的系统并进行了测试，初步测试未发现问题。

2.将吴老师提供的图片整合到对应的apk中去，实测能够正常显示壁纸和新的apk图标。感谢吴老师。

3.找到了一个声称是[x86平台的GMS](http://www.apkmirror.com/apk/google-inc/google-play-services/google-play-services-8-1-15-2250156-876-x86-android-apk-download/),安装后测试无法使用。没有找到对应x86的google play store。目前仍没有一个GMS方面的解决方法。

4.继续分析enable_nativebridge脚本，试图寻找不执行脚本就内置libhoudini的方法。



##2016-01-10工作内容
1.尝试安装GMS，在一个国外网站下载了一个[apk形式的GMS](http://www.androidapksfree.com/apk/google-play-services-8-4-89-2428711-030-android-2-3-apk-download/),并从同一来源下载了[GooglePlay store](http://www.androidapksfree.com/apk/google-play-store-apk-latest-version-download/)。安装之后能够进入GooglePlay store并选择安装app，但无论选择何种app，安装都会卡在正在下载。通过网络查找了一些相关问题，感觉是下载apk的目标地址被墙且hosts无效。另外尝试了android-x86官网提供的镜像的GooglePlay功能，同样无法使用。但根据之前和黄志伟的谈话，他自己说他提供的镜像这部分功能理应是正常的。

2.探索了通过shell安装apk的方法，希望通过此法能够解决一些无法预先植入系统的apk问题

3.继续查找了Microsoft office。通过1中提到的环境运行Microsoft office mobile能够成功通过点击对应项目（word，excel，PowerPoint）进入GooglePlay，但页面显示所在国家或地区无法购买或安装，解决方法未知。




##2016-01-06工作内容
1.尝试将以前修改过的install.img文件进行进一步合理化修正，保证今后的img能够更好地进行安装。

2.阅读了houdini解压的脚本，目前想到的解决方案是添加开机自动启动enable_nativebridge脚本。这样第一次开机的时候便能够自动装载houdini，此后开机由于Houdini已经存在，脚本运行只有一个判断过程也不会太影响开机时间。但此种方法仍需要用户手动在Setting中开启Apps compatibility的支持才能够运行arm应用。

3.目前仍未找到将Apps compatibility默认设为开启的方法。