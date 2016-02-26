wimlib静态链接生成可执行程序相关文档

生成环境：ubuntu15.04（64位）
源码：wimlib-1.8.2  （http://wimlib.net/downloads/wimlib-1.8.2.tar.gz）
需要下载：libiconv-1.14源码

## 64位可执行文件生成

1.生成64位可使用的libiconv静态库

进入下载好的libiconv-1.14文件夹

./configure --enable-static --prefix=$(pwd)/_INSTALL CFLAGS=-static

make -j8 

可能会出现链接错误，不用管它，直接查看preload/.libs文件夹里是否生成libiconv.a,若生成继续

2.生产64位的可执行文件wimlib-imagex(静态链接)

进入下载好的wimlib-1.8.2文件夹

sudo apt-get install ntfs-3g-dev

sudo apt-get install libfuse-dev

./configure --prefix=$(pwd)/_INSTALL --enable-static --with-libiconv-prefix=/home/chyz/Downloads/libiconv-1.14/preload/.libs CFLAGS=-static 

修改Makefile，将LDFLAGS=-Wl，然后进行编译

make -j8

之后会出现错误，这是正常现象

创建一个test.sh,内容如下：

~~~
#! /bin/bash

g++  -static -std=gnu99 -fvisibility=hidden -v -fno-common -Wmissing-prototypes -Wstrict-prototypes -Wundef -Wno-pointer-sign -Wno-deprecated-declarations -o wimlib-imagex programs/wimlib_imagex-imagex.o  ./.libs/libwim.a /home/chyz/Downloads/libiconv-1.14/preload/.libs/libiconv.a -Wl,--start-group -ldl -lxml2 -lpthread -lntfs-3g -lfuse -lm -lattr -lz -lrt -licule -licutu -licui18n -licuuc -licuio -liculx -licudata  -Wl,--end-group
~~~

运行bash test.sh 可以获得静态链接生成的可执行文件wimlib-imagex

## 32位可执行文件生成

1.生成32位可使用的libiconv静态库

进入下载好的libiconv-1.14文件夹

./configure --enable-static --prefix=$(pwd)/_INSTALL CFLAGS="-static -m32"

make -j8 

可能会出现链接错误，不用管它，直接查看preload/.libs文件夹里是否生成libiconv.a,若生成继续,使用objdump 查看是否是32位的静态库

2.生产32位的可执行文件wimlib-imagex(静态链接)

./configure --prefix=$(pwd)/_INSTALL --enable-static --with-libiconv-prefix=/home/chyz/Downloads/libiconv-1.14/preload/.libs CFLAGS="-static -m32" 

sudo apt-get install ntfs-3g-dev:i386

sudo apt-get install libfuse-dev:i386

修改Makefile，将LDFLAGS=-Wl，然后进行编译

make -j8

之后会出现错误，这是正常现象

创建一个test.sh,内容如下：

~~~
#! /bin/bash

g++  -static -m32 -std=gnu99 -fvisibility=hidden -v -fno-common -Wmissing-prototypes -Wstrict-prototypes -Wundef -Wno-pointer-sign -Wno-deprecated-declarations -o wimlib-imagex programs/wimlib_imagex-imagex.o  ./.libs/libwim.a /home/chyz/Downloads/libiconv-1.14/preload/.libs/libiconv.a -Wl,--start-group -ldl -lxml2 -lpthread -lntfs-3g -lfuse -lm -lattr -lz -lrt -licule -licutu -licui18n -licuuc -licuio -liculx -licudata  -Wl,--end-group
~~~

运行bash test.sh 可以获得静态链接生成的可执行文件wimlib-imagex(32位)


