#cts测试
##测试目的：测试运行Android系统的设备是否完全兼容Android规范。

cts测试大部分是基于Junit和仪表盘技术编写的。还扩展了自动化测试过程，可以自动执行用例，自动收集和汇总测试结果。CTS采用XML配置文件的方式将这些测试用例分组成多个测试计划（plan）,第三方也可以创建自己的plan。

本测试说明ubuntu中进行测试，在windows相关需要自己进行一定修改

测试前需要准备：

##1. 测试机下载安装android-sdk：

下载地址：http://developer.android.com/sdk/index.html

安装参照官网教程

设置环境变量：
    $vi ~/.bashrc
    添加
    PATH=/home/chyz/android-related/android-sdk/Android/platform-tools:$PATH
   
1.Ensure 'adb' is in your current PATH. adb can be found in the Android SDK available from http://developer.android.com

Example:
  PATH=$PATH:/home/myuser/android-sdk-linux_x86/platform-tools

##2. 测试机下载cts测试包：

下载地址： https://dl.google.com/dl/android/cts/android-cts-5.1_r4-linux_x86-x86.zip

下载版本：5.1-r4

##3. 在android-x86相关设置

可以参照官方测试前设置：（以下摘取较为重要的设置，参照可以完成cts测试）

（1）恢复出厂设置：Do a factory data reset on the device (Settings > storage > Factory data reset).

Warning: This will erase all user data from the device.

（2）设置手机语言为英语：Setting->Language&input->language->English(United States)。

（3）去掉屏幕锁：Make sure no lock pattern is set on the device (Settings > Security > Screen Lock should be 'None').

（4）USB调试：Make sure the "USB Debugging" development option is checked (Settings > Developer options > USB debugging).

（5）保持唤醒：Settings > Developer options > Stay Awake is checked

（6）允许模拟位置：Settings > Developer options > Allow mock locations is checked

（7）设备连接到可用wifi：device is connected to a functioning Wi-Fi network (Settings > Wi-Fi)

（8）CTS测试前设备处于主屏幕：device is at the home screen at the start of CTS (Press the home button).

（9）设备在测试过程中，不能运行其他任务：While a device is running tests, it must not be used for any other tasks.

（10）CTS测试过程中，不要触碰任何键：Do not press any keys on the device while CTS is running. Pressing keys or touching the screen of a test device will interfere with the running tests and may lead to test failures.

##4. 利用wifi调试android-x86(adb连接)
（1）启动adbd服务

打开终端运行shell命令:

~~~
    su
    setprop service.adb.tcp.port 5555
    stop adbd
    start adbd 
~~~

su命令表示切换到root状态；

setprop service.adb.tcp.port 5555表示设置adb tcp连接的端口为5555；

stop adb及start adbd表示重启adbd服务。

这时到手机的设置->系统->网络里查看手机ip地址多少，如为192.168.1.193

（2）在测试机上进行adb连接（默认ubuntu中）

输入：adb connect 192.168.1.193 （android-x86的ip）注意：两者需要连在同一局域网中

表示adb连接到手机，连接成功会显示connected to 192.168.1.194:5555或already connected to 192.168.1.194:5555。如果adb命令没有找到，添加android sdk的tools目录到系统path中（即#1步骤）。

（3）测试连接是否成功

输入：adb shell 

看是否进入android-x86中的shell,正常进入后可以exit

（4）如果需要执行可访问性方面的兼容性测试，则在测试机上安装“CtsDelegatingAccessibilityService.apk”（adb install –r */android-cts/repository/testcases/ CtsDelegatingAccessibilityService.apk 假如目录中没有此文件，就省去这一步，一般情况下是没有的），并在设备上将Settings->Accessibility->Delegating Accessibility Service选项打开。

（5）如果需要执行设备管理方面的兼容性测试，则在测试机上安装“CtsDeviceAdmin.apk” (adb install –r */android-cts/repository/testcases/ CtsDeviceAdmin.apk)，并在设备上将Setting->Security->Devices Administrators->android.devicesadmin.cts.CtsDevicesAdmin等选项打开。On the device, enable Settings > Security > Device Administrators > android.deviceadmin.cts.CtsDeviceAdmin* settings 

##5. 进行测试：
    cd where/your/android-cts/tools   -->  进入到“*/android-cts/tools”目录；
    ./cts-tradefed  -->  执行bash cts-tradefed（或./cts-tradefed），识别设备。之后出现cts_host >，则证明已进入CTS命令行交互界面；
    cts-tf > help   -->  输入cts相关命令来执行cts测试(可以了解相关的help内容)
    run cts --plan CTS （测试默认CTS，其中包括所有的packages）  或  run cts --plan CTS --disable-reboot(Do not reboot device after running some amount of tests在测试过程中不需要重启手机)
    
##6. CTS测试结果分析：

测试结束后在*/android-cts/respository/results文件夹中，会看到以日期和时间命名的文件夹用于保存执行过的测试结果。而且还有一个同名的zip文件保存同样的内容。

测试过程中的自动记录log，测试结束后log自动保存在*/android-cts/respository/logs里边以日期和时间命名的文件夹。

在测试结果文件夹中，所有的测试结果是以XML的形式保存的。通常测试结果网页分成“Device Information”、“Test Summary”、“Test Summary by Package”、“Test Failures(xx)”和“Detailed Test Report”等四个区域。其中：

（1） “Device Information”中列出了被测设备具体的软硬件以及功能配置信息；

（2） “Test Summary”列出了CTS 版本号，各状态case个数等信息；

（3） “Test Failures(xx)”会将断言失败时的输出记录在内；

（4） “Detailed Test Report”是测试结果的详细报告。

每次测试保证把CTS测试case全部跑完，用 “l r”查看，本次CTS测试是否全部run完，即not executed一列的数值是0，如果数值不为0，则表示还剩下没有run完的case,有可能是手机冻结或者reset导致adb识别不了设备，所以后边的case都为not executed状态。

##附加资料--CTS测试部分常用命令

与host相关的部分常用命令：

help: CTS命令一览表

help all: show the complete tradefed help

exit：退出cts终端

与run相关的部分常用命令：

run cts --plan test_plan_name: 执行一个测试计划

run cts --package/-p package_name: 单独run cts测试中的一个名为package_name包

run cts --class/-c com.class_name [--method/-m] methmod_name: run指定的类，或具体到类中的方法（单独测试某个类的方法，-c后面跟类名全路径，–m后面跟方法名）

run cts --continue-session session_ID: 继续run指定session上状态为not executed 的case

run cts [options] --serial/-s device_ID: 在指定device_ID上run cts [option]

run cts [options] --shards number_of_shards: shard a CTS run into given number of independent chunks, to run on multiple devices inparallel

run cts --help/--help-all: get more help on running CTS

与java包相关的部分常用命令：

l/list d/devices: 列出所有连接的设备和设备的状态

l/list packages: 列出CTS所有的测试包

l/list p/plan: 列出CTS所有的测试计划

l/list i/invocations: list invocations aka CTS test runs currentlyin progress

l/list c/commands: list commands: aka CTS test run commands currently in the queue waiting to be allocated devices

l/list r/results: list CTS results currently present in the repository

与测试计划相关的部分常用命令：

add derivedplan --plan plan_name --session/-s session_id –r [pass/fail/notExecuted/timeout]：从指定session id中根据case的各种状态产生一个新的测试计划

Dump:

  d/dump l/logs: dump the tradefed logsfor all running invocations

与option相关的命令：

run cts --disable-reboot [option]: 在测试过程中不需要重启手机

参照：

http://www.cnblogs.com/zh-ya-jing/p/4396918.html

http://blog.csdn.net/andrewblog/article/details/17510869

http://www.cnblogs.com/liu-ke/p/4300666.html

完整测试时间较长，大概需要3小时