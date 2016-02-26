2016-01-27 工作内容
---
1. 整理这几天的工作文档。已上传：https://github.com/openthos/xposed-analysis/wiki/xposed-framework-build-script-analysis

2016-01-26 工作内容
---
1. 根据昨天对XposedTools中的编译脚本的分析，发现其实该脚本真正在android源码中的编译命令和直接make的区别不是很大。尝试提取编译命令直接在android源码下make，用提取出来的命令编译可以通过。但是和android源码一起编译仍然不行。
明天打算先整理下这几天的工作文档。整理完成后继续研究xposed。


2016-01-25 工作内容
---
1. 分析XposedTools中，用来编译Xposed Framework的脚本（主要是build.pl和Xposed.pm)，了解他们都干了什么事情，为后面的集成到安卓源码中，随安卓源码一起编译打基础。

2016-01-19~21 工作内容
---
1. 学习Perl。
2. 整理github上的文档。

2016-01-18 工作内容
---
1. 学习Perl。Xposed的框架编译工具是用perl写的。发现里面很多东西不理解，只好再恶补一下基础。

2016-01-14 工作内容
---
1. 找了个Makefile教程，学习makefile文件的语法、格式等。  
本来是想分析下xposed框架的源码的，发现android.mk的东西看的一知半解的，然后android.mk又是在makefile的基础上扩展或者说修改来的，所以还是先看makefile吧。磨刀不误砍柴工，后面修订、集成xposed到安卓源码中，少不了和他们打交道。打好基础，后面事半功倍。毕竟编译、安装、调试都不是分分钟能搞定的事。  


2016-01-13 工作内容
---
1. 翻阅Xposed资料。
2. 找android studio的使用教程看了看。前面想自己根据Xposed作者的模块例子，自己编译一个试试呢，发现Studio不太会用。
3. 参加学生交流会。

2016-01-12 工作内容
---
1. 找Xposed模块。翻了不少，但是最后只又装了一个简单的，模拟器实在太卡。
2. 翻阅Xposed资料，撰写交流ppt

2016-01-11 工作内容
---
1. 在安卓系统上运行xposed。今天终于跑起来了。
2. 撰写xposed运行中碰到的问题，提交到issue上了。
3. 撰写xposed运行的方法，提交到了项目文档中。
