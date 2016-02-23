#bug
log for msexcel
`````
12-14 06:52:24.620  2902  3020 I OpenGLRenderer: Initialized EGL, version 1.4
12-14 06:52:24.620  2902  3020 W OpenGLRenderer: Failed to choose config with EGL_SWAP_BEHAVIOR_PRESERVED, retrying without...
12-14 06:52:24.629  2902  3020 D OpenGLRenderer: Enabling debug mode 0
12-14 06:52:24.658  2902  3020 W GraphicBufferMapper: registerBuffer(0xe1f95340) failed -22 (Invalid argument)
12-14 06:52:24.658  2902  3020 E GraphicBuffer: unflatten: registerBuffer failed: Invalid argument (-22)
12-14 06:52:24.658  2902  3020 E Surface : dequeueBuffer: IGraphicBufferProducer::requestBuffer failed: -22
--------- beginning of crash
12-14 06:52:24.658  2902  3020 F libc    : Fatal signal 11 (SIGSEGV), code 1, fault addr 0x1a8 in tid 3020 (RenderThread)
12-14 06:52:24.709  1796  1796 I DEBUG   : *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
12-14 06:52:24.709  1796  1796 I DEBUG   : Build fingerprint: 'Android-x86/android_x86_64/x86_64:5.1.1/LVY48I/12080507:eng/test-keys'
12-14 06:52:24.709  1796  1796 I DEBUG   : Revision: '0'
12-14 06:52:24.709  1796  1796 I DEBUG   : ABI: 'x86'
12-14 06:52:24.709  1796  1796 I DEBUG   : pid: 2902, tid: 3020, name: RenderThread  >>> com.microsoft.office.excel <<<
12-14 06:52:24.709  1796  1796 I DEBUG   : signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x1a8
12-14 06:52:24.710  1796  1796 I DEBUG   :     eax 00000000  ebx e110acec  ecx 00000001  edx e1f07280
12-14 06:52:24.710  1796  1796 I DEBUG   :     esi e1ec0014  edi e1d97f60
12-14 06:52:24.710  1796  1796 I DEBUG   :     xcs 00000023  xds 0000002b  xes 0000002b  xfs 000000b7  xss 0000002b
12-14 06:52:24.710  1796  1796 I DEBUG   :     eip e0ba952d  ebp e1d97ef0  esp e1211700  flags 00210246
12-14 06:52:24.710  1796  1796 I DEBUG   :
12-14 06:52:24.710  1796  1796 I DEBUG   : backtrace:
12-14 06:52:24.710  1796  1796 I DEBUG   :     #00 pc 000fc52d  /android/system/lib/dri/i965_dri.so
12-14 06:52:24.710  1796  1796 I DEBUG   :     #01 pc 12345677  <unknown>
12-14 06:52:24.733  1796  1796 I DEBUG   :
12-14 06:52:24.733  1796  1796 I DEBUG   : Tombstone written to: /data/tombstones/tombstone_09
12-14 06:52:24.733  2066  2092 I BootReceiver: Copying /data/tombstones/tombstone_09 to DropBox (SYSTEM_TOMBSTONE)
12-14 06:52:24.734  2066  3026 W ActivityManager:   Force finishing activity 1 com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity
12-14 06:52:24.735  1792  1838 D PermissionCache: checking android.permission.READ_FRAME_BUFFER for uid=1000 => granted (36 us)
12-14 06:52:24.741  2066  2108 W InputDispatcher: channel '2d77d785 com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity (server)' ~ Consumer closed input channel or an error occurred.  events=0x9
12-14 06:52:24.741  2066  2108 E InputDispatcher: channel '2d77d785 com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity (server)' ~ Channel is unrecoverably broken and will be disposed!
12-14 06:52:24.741  2066  2108 W InputDispatcher: channel 'f53b1ef com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity (server)' ~ Consumer closed input channel or an error occurred.  events=0xd
12-14 06:52:24.741  2066  2108 E InputDispatcher: channel 'f53b1ef com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity (server)' ~ Channel is unrecoverably broken and will be disposed!
12-14 06:52:24.741  1804  1804 I Zygote  : Process 2902 exited due to signal (11)
12-14 06:52:24.745  2066  2402 I WindowState: WIN DEATH: Window{f53b1ef u0 com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity}
12-14 06:52:24.745  2066  2402 W InputDispatcher: Attempted to unregister already unregistered input channel 'f53b1ef com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity (server)'
12-14 06:52:24.746  2066  2108 I InputDispatcher: Dropping event because there is no touchable window at (1619, 374).
12-14 06:52:24.747  2066  2843 I WindowState: WIN DEATH: Window{2d77d785 u0 com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity}
12-14 06:52:24.747  2066  2843 W InputDispatcher: Attempted to unregister already unregistered input channel '2d77d785 com.microsoft.office.excel/com.microsoft.office.apphost.LaunchActivity (server)'
12-14 06:52:24.747  2066  3026 W ActivityManager: Exception thrown during pause
12-14 06:52:24.747  2066  3026 W ActivityManager: android.os.DeadObjectException
12-14 06:52:24.747  2066  3026 W ActivityManager:        at android.os.BinderProxy.transactNative(Native Method)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at android.os.BinderProxy.transact(Binder.java:496)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at android.app.ApplicationThreadProxy.schedulePauseActivity(ApplicationThreadNative.java:704)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityStack.startPausingLocked(ActivityStack.java:825)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityStack.finishActivityLocked(ActivityStack.java:2726)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityStack.finishTopRunningActivityLocked(ActivityStack.java:2583)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityStackSupervisor.finishTopRunningActivityLocked(ActivityStackSupervisor.java:2497)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityManagerService.handleAppCrashLocked(ActivityManagerService.java:11505)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityManagerService.makeAppCrashingLocked(ActivityManagerService.java:11402)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityManagerService.crashApplication(ActivityManagerService.java:12086)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.ActivityManagerService.handleApplicationCrashInner(ActivityManagerService.java:11597)
12-14 06:52:24.747  2066  3026 W ActivityManager:        at com.android.server.am.NativeCrashListener$NativeCrashReporter.run(NativeCrashListener.java:86)
12-14 06:52:24.747  1789  1789 E lowmemorykiller: Error opening /proc/2902/oom_score_adj; errno=2
12-14 06:52:24.748  2066  2276 I ActivityManager: Process com.microsoft.office.excel (pid 2902) has died
`````

#solution

bigandroid@googlegroups.com 代表 就是与众不同 [stormdragon@vip.qq.com]  
[答复] [全部答复] [转发]  
操作  
收件人:  
 bigandroid ‎[bigandroid@googlegroups.com]‎  
收件箱  
2015年12月18日 2:00  
各位好！  
      这是技德工程师有关本次错误的原因说明。  


------------------ 原始邮件 ------------------  
发件人: "wuzhen";<wuzhen@jidemail.com>;  
发送时间: 2015年12月17日(星期四) 下午4:43  
收件人: "就是与众不同"<stormdragon@vip.qq.com>;  
 
主题: Re: 回复： 回复： 回复： 有关您说的错误修复方式 


这个改动我们会提交给android-x86项目。这个问题在4.0的kernel上也存在。问题在于gralloc_drm_handle_t 类型在64位上长度不是40字节而是44字节。所以在ipc的时候data指针的值是错的。kernel 4.4可能改了分配器的行为，使得分配数据在4GB以上了。所以更容易触发这个问题。

如果替换了lib能运行，那就不用改build里那个问题了。那个我是为了之后的crash log更详细让你加的。和这个bug无关

