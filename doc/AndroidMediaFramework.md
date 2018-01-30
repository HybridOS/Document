## Android Media Framework
----

最近在解Android-x86上一个直播视频不能播放的bug,一不小心就踩进了这么打一个"坑";当然了,是坑还是要填的,谁让我们的先人愚公先生可以有移山的气概呢!在google上找了很多文章参考,这篇文章不知道该从何写起,整理思路,先了解一下Android流媒体框架的发展历程.
### android 流媒体框架的发展历程介绍:
 >|Android 流媒体框架随着Android版本的提升过发生了几次重要的变化,下面简单对几个阶段进行概览,以方便了解流媒体框架的前世今生.

**Opencore (Android 1.0开始使用的流媒体框架)**

Android初始版本上，预设的多媒体框架(multimedia framework)是OpenCORE。OpenCORE的优点是兼顾了跨平台的移植性，而且进过了多年的打磨和验证，所以相对来说较为稳定；但是其优点也是确定,就是国语庞大和复杂，维护成本比较打。Android 2.0开始，Google引进了架构稍微简介的Stagefright，并且有逐渐取代OpenCORE的趋势,当然事实是opencore最终被取代;


[1.android媒体开发--OpenCore和Stagefright](http://blog.csdn.net/xmc281141947/article/details/60766737)<br>
[2.android多媒体框架](http://blog.csdn.net/a632526511/article/details/47100341)<br>
[3.Android Media Framework](https://oopsmonk.github.io/blog/2016/06/16/android-media-framework)<br>
[4.NuPlayer for HTTP live streaming](http://www.cnblogs.com/zhgyee/archive/2011/10/31/2230852.htm)<br>
[5.MediaCodec与ACodec通知分析](http://blog.csdn.net/coolcary/article/details/51939080)<br>
[6.Android-7.0-Nuplayer-启动流程](http://blog.csdn.net/miaomiao12345678/article/details/56961370)<br>
[7.Android平台使用MediaCodec进行H264格式的视频编解码](http://www.it610.com/article/5752036.htm)<br>
[8.NuPlayer different from the AwesomePlayer](https://www.quora.com/How-is-the-new-NuPlayer-different-from-the-AwesomePlayer-in-Android)

**Current Media Framework**

Android 包含 Stagefright。Stagefright 是位于 Native 层的媒体播放引擎，内置了基于软件的编解码器，且适用于热门媒体格式。

Stagefright 音频和视频播放功能包括集成 OpenMAX 编解码器、会话管理、基于时间的同步渲染、传输控制和 DRM。

Stagefright 还支持集成您提供的自定义硬件编解码器。要设置编码和解码媒体的硬件路径，您必须将基于硬件的编解码器作为 OpenMax IL（集成层）组件进行实现。

![媒体架构](https://source.android.com/devices/media/images/ape_fwk_media.png)

**应用框架**<br>
应用代码位于应用框架层，可利用 android.media API 与多媒体硬件进行交互。

**Binder IPC**<br>
Binder IPC 代理用于促进跨越进程边界的通信。它们位于 frameworks/av/media/libmedia 目录中，并以字母“I”开头。

**Native 多媒体框架**<br>
在 Native 层，Android 提供了一个利用 Stagefright 引擎进行音频和视频录制及播放的多媒体框架。Stagefright 随附支持的软件编解码器的默认列表，并且您可以使用 OpenMax 集成层标准实现自己的硬件编解码器。有关实现的更多详细信息，请参阅位于 frameworks/av/media 中的 MediaPlayer 和 Stagefright 组件。

**OpenMAX 集成层 (IL)**<br>
OpenMAX IL 为 Stagefright 提供了一种标准化的方式来识别和使用基于硬件的自定义多媒体编解码器（称为组件）。您必须以名为 libstagefrighthw.so 的共享库的形式提供 OpenMAX 插件。此插件将 Stagefright 与您的自定义编解码器组件相连接，并且该组件必须根据 OpenMAX IL 组件标准来实现。

### 参考:

- [Multimedia framework](https://en.wikipedia.org/wiki/Multimedia_framework)
- [Mux? Demux? Remux?Huh?](http://adubvideo.net/informative/mux-demux-remux-huh)
- [Android MediaPlayerService Architecture](https://edwardlu0904.wordpress.com/2015/09/04/android-mediaplayerservice-architecture/)
- [OpenMAX](https://www.khronos.org/openmax/)
- [websequencediagrams](https://www.websequencediagrams.com/)
