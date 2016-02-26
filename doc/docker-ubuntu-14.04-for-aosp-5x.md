step 1
============
禁用selinux  
   由于Selinux和LXC有冲突，所以需要禁用selinux。编辑/etc/selinux/config，设置两个关键变量。    
SELINUX=disabled  
SELINUXTYPE=targeted  

安装 Docker :  
   $ sudo yum install docker  
 
启动 Docker 进程  
    $ sudo service docker start  
    \# systemctl start docker  
想要开机启动 Docker ，我们需要执行如下的命令：    
    $ sudo chkconfig docker on  
    
现在测试一下是否正常工作.  
    $ sudo docker run -i -t fedora /bin/bash  

注意: 如果你运行的时候提示一个 Cannot start container 的错误，错误中提到了 SELINUX 或者 权限不足。你需要更新 SELINUX 规则。你可以使用 sudo yum upgrade selinux-policy 然后重启。    


Docker会默认将数据文件存放在/var/lib/docker目录，如果该目录所在的分区不是很大，那么建议将这个目录改到其他容量将大的分区下 /other/dir。  
\# systemctl stop docker  
编辑docker.service  
 /usr/lib/systemd/system/docker.service   
 
 将ExecStart=/usr/bin/docker -d改为ExecStart=/usr/bin/docker -d -g /other/dir。
 
然后  
\# systemctl start docker   

step 2 
===========
然后下载ubuntu的image：   
\# docker pull ubuntu:14.04   

再创建一个名为Dockerfile的文件，加入下列内容：

FROM ubuntu:14.04

ADD sources.list /etc/apt/sources.list

ENV DEBIAN_FRONTEND noninteractive  
RUN apt-get -qq update  
RUN apt-get install -y git-core gnupg flex bison gperf build-essential zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dev:i386 g++-multilib mingw32 schedtool tofrodos python-markdown pngquant libxml2-utils xsltproc zlib1g-dev:i386 libxext-dev:i386 openjdk-7-jdk 

WORKDIR /var/aosp

保存Dockerfile文件，然后在同目录下新建一个sources.list文件，也就是ubuntu的软件源配置文件，里面根据你所使用的镜像填入对应地址。sources.list的内容见最后

接下来就可以构建新的image了，在Dockerfile所在目录运行：其中aosp-build为image名称，可随意修改。  
\# docker build -t aosp-build .  

等构建完成后，我们就可以在得到的image上运行命令，并创建container了，比如运行一个bash shell，并创建一个在宿主机和container之间共享的目录：  
\# docker run -v /host/aosp-dir:/var/aosp/source -i -t --name aosp aosp-build bash  

这里宿主机上的/host/aosp-dir目录将会和container中的/var/aosp/source目录共享，并且使用--name选项指定了container的名字，以后便可以通过下面的命令再次启动这个container：

\# docker start -i -a aosp




ubuntu 14.04 sources.list
===================
\# deb cdrom:[Ubuntu 14.04 _trusty Vervet_ - Release amd64 (20140422)]/ trusty main restricted

\# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to  
\# newer versions of the distribution.  
deb http://mirrors.yun-idc.com/ubuntu/ trusty main restricted  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty main restricted

\## Major bug fix updates produced after the final release of the  
\## distribution.  
deb http://mirrors.yun-idc.com/ubuntu/ trusty-updates main restricted  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty-updates main restricted  

\## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu  
\## team. Also, please note that software in universe WILL NOT receive any  
\## review or updates from the Ubuntu security team.  
deb http://mirrors.yun-idc.com/ubuntu/ trusty universe  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty universe  
deb http://mirrors.yun-idc.com/ubuntu/ trusty-updates universe  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty-updates universe  

\## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu   
\## team, and may not be under a free licence. Please satisfy yourself as to  
\## your rights to use the software. Also, please note that software in   
\## multiverse WILL NOT receive any review or updates from the Ubuntu  
\## security team.  
deb http://mirrors.yun-idc.com/ubuntu/ trusty multiverse  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty multiverse  
deb http://mirrors.yun-idc.com/ubuntu/ trusty-updates multiverse  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty-updates multiverse  

\## N.B. software from this repository may not have been tested as  
\## extensively as that contained in the main release, although it includes  
\## newer versions of some applications which may provide useful features.  
\## Also, please note that software in backports WILL NOT receive any review  
\## or updates from the Ubuntu security team.  
deb http://mirrors.yun-idc.com/ubuntu/ trusty-backports main restricted universe multiverse  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty-backports main restricted universe multiverse  

deb http://mirrors.yun-idc.com/ubuntu/ trusty-security main restricted  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty-security main restricted  
deb http://mirrors.yun-idc.com/ubuntu/ trusty-security universe  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty-security universe  
deb http://mirrors.yun-idc.com/ubuntu/ trusty-security multiverse  
deb-src http://mirrors.yun-idc.com/ubuntu/ trusty-security multiverse  

\## Uncomment the following two lines to add software from Canonical's  
\## 'partner' repository.  
\## This software is not part of Ubuntu, but is offered by Canonical and the  
\## respective vendors as a service to Ubuntu users.  
deb http://archive.canonical.com/ubuntu trusty partner  
deb-src http://archive.canonical.com/ubuntu trusty partner  
deb http://archive.getdeb.net/ubuntu trusty-getdeb apps  
