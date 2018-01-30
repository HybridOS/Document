
### Debug Android native program 

在调试初期我也使用了上面的方法，能比较有效进行调试; 这里有更好的调试方式可以参考谷歌官方网站：

- 首先`adb connect tagetip && adb root`

**调试运行中的应用或进程**

要连接到已在运行的应用或本机守护进程，请配合使用 gdbclient 和 PID。例如，要调试 PID 为 1234 的进程，请运行：

`gdbclient 1234`

- **此脚本会做下面几件事情：**

   - 1.设置端口转发并在目标设备(要调试的设备)上启动相应的 gdbserver（根据要调试的库的目标平台(32位程序还是64位程序)数启动相应版本的gdbserver（gdbserver64））
   - 2.在宿主机上启动相应的 gdb，配置gdb以找出符号（配置带调试符号库所在位置，可以避免手动配置）;
然后将 gdb 连接到远程 gdbserver（使用了target remote :$PORT,你可以去看一下这个脚本）。


**调试本机进程启动**

要在进程启动时对其进行调试，请使用 gdbserver 或 gdbserver64（适用于 64 位进程）。例如：

```adb shell gdbserver :5039 /system/bin/MY_TEST_APP```
输出示例：
```
Process MY_TEST_APP created; pid = 3460
Listening on port 5039
```
接下来，从 gdbserver 输出中找出应用 PID，并将其用于其他终端窗口。

`gdbclient APP_PID`
最后，在 gdb 提示处输入 ``continue``。

注意：如果您指定了错误的 gdbserver，将会收到无用的错误消息（例如“Reply contains invalid hex digit 59”）。


### 参考：
1.如何使用 gdb 调试 Android 应用和进程

  链接： https://source.android.com/devices/tech/debug/gdb

2.Google Chrome for x86

  https://www.apkmirror.com/apk/google-inc/chrome/variant-%7B%22arches_slug%22%3A%5B%22x86%22%5D%7D/

3.Android平台使用MediaCodec进行H264格式的视频编解码

  链接 :http://www.it610.com/article/5752036.htm

4.Debugging Android native shared libraries

链接:http://blog.dornea.nu/2015/07/01/debugging-android-native-shared-libraries/

