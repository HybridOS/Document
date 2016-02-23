多窗口阶段性总结
===

##介绍

我们的目标其实是将android系统改造为我们大家平时熟悉pc操作系统的使用体验；也就是多窗口多任务的，可使用鼠标键盘来方便的的操作android系统。

##进展

* 了解现在市面上已有的两款android桌面系统（remixos/phoneixos）

* 调查当前已有资源（一些开源项目的进展--android多窗口系统），为下一步的介入进行预研；
   * Tieto 开源项目
   * CornerStone 开源项目
* 基于上面的了解，进行相关代码测试
   * 基于android4.4进行了arm平台的测试，在android模拟器上运行进行了初步验证；
   * 代码移植，基于多窗口系统的实现，将代码应用到andriod-x86 的kitkat-x86分支，进行结果验证，符合预期效果；

* 代码消化（下一步需要做的事情）
   * 理解Tieto提交的还以，跟深入的窥视framework层面窗口处理的机制

* 产品化的设计（这是一个比较重要的部分）
   * 产品交互界面的设计


##结果
初步完成对kitkat-x86分支的多窗口demo系统；

##演示
![android-mw-cut](https://cloud.githubusercontent.com/assets/16587815/12527611/84bf5d1a-c1b9-11e5-909a-d6f105eddec1.png)
![mwdemo2](https://cloud.githubusercontent.com/assets/16587815/12527614/889a2aaa-c1b9-11e5-9040-ce2f74b89517.png)
<img src="/home/zhongtian/Pictures/android-mw-cut.png" width="600" height="350" />
<br>
<img src="/home/zhongtian/Pictures/mwdemo2.png" width="500" height="300" />