##20160121工作日志
**今日工作**

1. 编写shell脚本，完成自动安装APK包
2. 开multi-windown技术交流会
3. 编写年终总结


##20160120工作日志
**今日工作**

1. 完成[[cts testresult结果分析文档编写 |https://github.com/openthos/testing-analysis/blob/master/tech-report/CTS%205.1testResult.xml%E5%88%86%E6%9E%90.md]]

2. github上文档整理及编写[[测试流程 | https://github.com/openthos/app-testing-results/wiki]]


##20160119工作日志
**今日工作**

1. 继续整理CTS packages及[[编写文档|https://github.com/openthos/testing-analysis/blob/master/tech-report/CTS%205.1testResult.xml%E5%88%86%E6%9E%90.md]]

##20160118工作日志
**今日工作**

1. CTS packages调研及生成文档
2. 学习编译，下载源码后编译了android-x86的传统安装方式iso及EFI安装方式的ISO文件

##20160115工作日志
**今日工作**

1. final-user版本连网测试，及研究分析CTS结果。
 - 已初有成效，下周一提交分析结果。目前除3D图形用例均为未执行外，其他包均有pass或fail的结果
   
2. 内部总结会


##20160114工作日志
**今日工作**

1. 继续研究分析CTS结果，
结果汇总中数据如下，看到pass数仅为不仅四万，而not executed数达到15万。pass率不足五分之

  分析发现CTS 5.1版本共含有21个plan。结果中的Test Package共211个，其中有105个package重复出现。去掉重复数据后结果如下，pass率提高至三分之一
2. 安装android studio;搭建编译环境，编译android 5.1 x86-64版本。

**今日文档**

1、撰写 [[CTS 结果分析 |https://github.com/openthos/testing-analysis/wiki/CTS-5.1testResult.xml%E5%88%86%E6%9E%90]]
2、阅读 [[CTS测试结果分析 |http://blog.csdn.net/chengfuyong001/article/details/8720287]]
3、推荐 [[ CTS测试研究之一二三|http://blog.csdn.net/zjujoe/article/details/5673469]]
4、收集 
5、想看  

##20160113工作日志
**今日工作**

1. 研究分析CTS结果，分析的方法有两种：

 -1、可以直接从Failure Details找原因；（个人感觉应该难度较大）

 -2、结合源代码以及Failure Details的信息找原因
  第二种方法牵扯到找测试源代码的问题，这就要对CTS源码目录以及相应生成物的命名有一定的了解。需要学习研究:android build system、CTS源码、makefile。
2. 参加openthos技术交流会议
3. 办公机ubuntu系统上安装studio，学习分析android build system


**今日文档**

1、撰写 [[CTS 结果分析 |https://github.com/openthos/testing-analysis/blob/master/tech-report/CTS%20%E7%BB%93%E6%9E%9C%E5%88%86%E6%9E%90.md]]
2、阅读 [[CTS测试 |https://source.android.com/compatibility/cts/index.html]]
3、推荐 [[CTS 测试研究 |http://blog.csdn.net/zjujoe/article/details/5640461]]
4、收集 
5、想看  

##20160112工作日志
**今日工作**

1. 在两台机器上进行final-user版本测试 
2. 抓取各个APP-log信息，并分别上传至各目录.地址：https://github.com/openthos/app-testing-results/
3. final-user版本各APP测试结果汇总详见： https://github.com/openthos/app-testing-results/blob/master/app-lists.md
4. 此版发现的问题均提交至https://github.com/openthos/app-testing-results/issues
  **如以下几个问题**

 -1)QQ music 无法下载声音，只能播放本地音乐。后期需考虑APP识别有线网线，而非只支持WIFI和流量

 -2)在以下四种情况下rotation功能失效

     -**重启;休眠唤醒后;新安装APP;一段时间不操作再次打开竖屏程序时又回到竖屏**

 -3)设置-语言-中文（简体）后有的APP仍为英文显示，如note，打开后菜单项均为英文

 -4)删除预安装的APP后，APP图标仍在，打开后不停提示“*APP已停止运行”。无法进行其他操作，重启后恢复正常

**明日计划**

 CTS学习及分析 

----------------------------------------------------------------------------------------------------------------------

##20160111工作日志
**今日工作**

1. 查看学习[[CTS测试 |https://source.android.com/compatibility/cts/index.html]]
2. 查看git上文章。下午回公司开会 

**明日计划**
final-user版本测试 
Recovery for window APP测试
----------------------------------------------------------------------------------------------------------------------
##20160108工作日志
**今日工作**

1. CTS 結果收集，上传至https://github.com/openthos/testing-analysis/tree/master/CTS-testresult
2. 下载remix_x86_64_eng版本，dd到u盘安装未果。陈一璋在windows上重做镜像安装盘，修改安装方式为legacy.再次尝试安装.

- 2.1.  Resident mode 测试remix版本，问题如下：

 * 命令行界面时间超长，误认为无响应呢
 * 图形安装界面点[下一步]后反应迟頓，要一段时间才会进入下一页面
 * 进入系统后点任何一APP，约5分钟左右才会有相应反应。多点击几次系统会重启

- 2.2 Guest mode 测试remix版本，问题如下：
 * 安装QQbrower浏览器，提示：“您的手机CPU类型与当前安装的浏览器版本不相符，立即下载相符版本？”----------注意提示中提到的是手机。
 * play 商店不可用，打开后一直处理“正在加载”状态
 * 植物大战僵屍APP 安装后打开，界面显示一段时间 后弹出“植物大战僵屍 无响应”的提示信息
 * 地铁跑酷APP 安装后打开，点击开始游戏后运行不至不分钟弹出“地铁跑酷 无响应”的提示信息
 * 百度云安装后打开时提示“百度云”已停止运行
 * 水果忍者安装后打开时提示“水果忍者”已停止运行
 * 开心消消乐- 通过文件管理器，利用APK包安装提示“your current game version is illegal.please download the offical version.”.APP应用黑屏显示,点关闭按钮可正常关闭此APP
 * 点已集成安装的mx player时提示“mx player”已停止运行
 * 点已集成安装的you tube，先跳转到play 商店页面，界面一直显示“”“正在加载”
 * 点已集成安装的云端硬盘，跳转到play 商店页面，界面一直显示“”“正在加载”
 * 点已集成安装的文档，跳转到play 商店页面，界面一直显示“”“正在加载”。文档无法使用
 * 点已集成安装的表格，跳转到play 商店页面，界面一直显示“”“正在加载”。表格无法使用
 * 点已集成安装的幻灯片，跳转到play 商店页面，界面一直显示“”“正在加载”。幻灯片无法使用
 * 点已集成安装的通迅录，跳转到play 商店页面，界面一直显示“”“正在加载”。通迅录无法使用-此应用是手机应用，X86上建议删除
 * apk包可以重复安装，
 * 休眠唤醒后安装APK包会弹出“解析程序包出现问题”，如微信，WPS，QQbrower。个别可以安装成功如：地铁跑酷


**今日文档**

1、撰写 
2、阅读 [[CTS测试 |https://source.android.com/compatibility/cts/index.html]]
3、推荐 
4、收集 
5、想看 

##20160107工作日志
**今日工作**

1. rc8-user版本功能测试，抓取各个APP-log信息，并分别上传至各目录.地址：https://github.com/openthos/app-testing-results/
   各目录下有单个APP的测试结果，详见每个目录下的test-record.txt文件
2. rc8-user版本各APP测试结果汇总详见： https://github.com/openthos/app-testing-results/blob/master/app-lists.md
3. 此版本去除了手机应用 phone,message,contacts
4. 无存储空间问题定位
设置-显示-休眠（默认无操作10分钟后进入休眠状态），这时需要按电源键唤醒。唤醒后安装APP时会有两种错误
 - 提示“没有存储空间”
 - 提示“解析软件包出现问题” 
此时无论安装哪个APK均报以上两种错误中的一种。即唤醒系统后安装APK存在问题。重启后再安装apk 包才会正常。 设置中选项最长为30分钟，所以这个问题需要解决，用户在使用过程中肯定会存在休眠问题，如中午去吃饭不关机。

**明日计划**

1.CTS rc8-user版本测试 及结果收集
2.查看CTS相关资料，学习分析CTS测试结果
3.自动化工具及框架调研

**今日文档**

1、撰写 
2、阅读 [[appium测试 |https://github.com/appium/appium]]应用自动化工具
3、推荐 
4、收集 
5、想看 


##20160106工作日志
1. 收集rc8-eng版CTS测试结果，上传至：https://github.com/openthos/testing-analysis/tree/master/CTS-testresult
2. 进行rc8-eng功能测试，抓取各个APP-log信息，并分别上传至各目录.地址：https://github.com/openthos/app-testing-results/
   各目录下有单个APP的测试结果，详见每个目录下的test-record.txt文件
3. rc8-eng各APP测试结果汇总详见： https://github.com/openthos/app-testing-results/blob/master/app-lists.md
4. 安装修改后的百度云apk包，安装后可以使用。可以将此包集成到发布版本中。如自己用下载的包安装仍无法使用。


##20160105工作日志
1. 收集CTS测试结果，与11.20号的那次对比查看了下。20160104 PASS数少。两次CTS测试均有大量的包未跑通。原因需细查
   测试结果已上传至：https://github.com/openthos/testing-analysis/tree/master/CTS-testresult
2. 学习refind下修改GRUB信息
3. rc7_eng版本上，执行enable_nativebridge后，回归验证issue所提交的BUG，并处理。
4. 提交三个版本存在的同性问题到issue上。
5. 下班前刚安装好RC8-eng版测试，周三进行功能测试，今晚先跑上CTS


##20160104工作日志
1. 查看CTS测试未保存LOG的原因，使用了普通用户，故LOG目录未生成。
2. 直连笔记本与测试机，在RC7 ENG版本上重新测试 CTS
3. 回单位开会，交接之前项目内容。
