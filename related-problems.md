##1. 正常编译的user版本android-x86无法启动

解决方法：原因是调用selinux相关内容，导致系统无法正常启动，需要修改源码，修改/system/core/init/Android.mk：
~~~
#ifneq (,$(filter userdebug eng,$(TARGET_BUILD_VARIANT)))
LOCAL_CFLAGS += -DALLOW_LOCAL_PROP_OVERRIDE=1 -DALLOW_DISABLE_SELINUX=1
#endif
~~~


