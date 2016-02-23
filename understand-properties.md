#　理解启动后的系统属性
在启动后的shell中，执行
```
getprop
```
就可以得到如下信息
```
root@x86_64:/ # getprop                                                        
[app.setupwizard.disable]: [0]
[dalvik.vm.dex2oat-Xms]: [64m]
[dalvik.vm.dex2oat-Xmx]: [512m]
[dalvik.vm.dex2oat-filter]: [verify-at-runtime]
[dalvik.vm.heapgrowthlimit]: [192m]
[dalvik.vm.heapmaxfree]: [8m]
[dalvik.vm.heapminfree]: [512k]
[dalvik.vm.heapsize]: [512m]
[dalvik.vm.heapstartsize]: [16m]
[dalvik.vm.heaptargetutilization]: [0.75]
[dalvik.vm.image-dex2oat-Xms]: [64m]
[dalvik.vm.image-dex2oat-Xmx]: [64m]
[dalvik.vm.image-dex2oat-filter]: [verify-at-runtime]
[dalvik.vm.isa.x86.variant]: [dalvik.vm.isa.x86.features=default]
[dalvik.vm.isa.x86_64.features]: [default]
[dalvik.vm.isa.x86_64.variant]: [x86_64]
[dalvik.vm.lockprof.threshold]: [500]
[dalvik.vm.stack-trace-file]: [/data/anr/traces.txt]
[dalvik.vm.usejit]: [true]
[debug.atrace.tags.enableflags]: [0]
[debug.egl.hw]: [0]
[debug.force_rtl]: [0]
[debug.logcat]: [1]
[dhcp.eth0.dns1]: [10.0.2.3]
[dhcp.eth0.dns2]: []
[dhcp.eth0.dns3]: []
[dhcp.eth0.dns4]: []
[dhcp.eth0.domain]: []
[dhcp.eth0.gateway]: [10.0.2.2]
[dhcp.eth0.ipaddress]: [10.0.2.15]
[dhcp.eth0.leasetime]: [86400]
[dhcp.eth0.mask]: [255.255.255.0]
[dhcp.eth0.mtu]: []
[dhcp.eth0.pid]: [1510]
[dhcp.eth0.reason]: [BOUND]
[dhcp.eth0.result]: [ok]
[dhcp.eth0.server]: [10.0.2.2]
[dhcp.eth0.vendorInfo]: []
[gsm.current.phone-type]: [1]
[gsm.network.type]: [Unknown]
[gsm.operator.alpha]: []
[gsm.operator.iso-country]: []
[gsm.operator.isroaming]: [false]
[gsm.operator.numeric]: []
[gsm.sim.operator.alpha]: []
[gsm.sim.operator.iso-country]: []
[gsm.sim.operator.numeric]: []
[gsm.sim.state]: [NOT_READY]
[init.svc.adbd]: [running]
[init.svc.bootanim]: [running]
[init.svc.console]: [running]
[init.svc.debuggerd]: [running]
[init.svc.debuggerd64]: [running]
[init.svc.dhcpcd_eth0]: [running]
[init.svc.drm]: [running]
[init.svc.gatekeeperd]: [running]
[init.svc.healthd]: [running]
[init.svc.installd]: [running]
[init.svc.keystore]: [running]
[init.svc.lmkd]: [running]
[init.svc.logcat]: [running]
[init.svc.logd]: [running]
[init.svc.logd-reinit]: [stopped]
[init.svc.media]: [running]
[init.svc.netd]: [running]
[init.svc.perfprofd]: [running]
[init.svc.powerbtnd]: [running]
[init.svc.ril-daemon]: [restarting]
[init.svc.servicemanager]: [running]
[init.svc.surfaceflinger]: [running]
[init.svc.ueventd]: [running]
[init.svc.vold]: [running]
[init.svc.zygote]: [running]
[init.svc.zygote_secondary]: [running]
[keyguard.no_require_sim]: [true]
[net.bt.name]: [Android]
[net.change]: [net.dns1]
[net.dns1]: [10.0.2.3]
[net.hostname]: [android-ad43bb47f760d150]
[net.qtaguid_enabled]: [0]
[net.tcp.default_init_rwnd]: [60]
[persist.sys.dalvik.vm.lib.2]: [libart.so]
[persist.sys.profiler_ms]: [0]
[persist.sys.strictmode.disable]: [1]
[persist.sys.strictmode.visual]: [0]
[persist.sys.usb.config]: [adb]
[rild.libargs]: [-d /dev/ttyUSB2]
[rild.libpath]: [/system/lib/libreference-ril.so]
[ro.alarm.volume.adjustable]: [true]
[ro.allow.mock.location]: [1]
[ro.arch]: [x86]
[ro.baseband]: [unknown]
[ro.board.platform]: [android-x86]
[ro.boot.hardware]: [android_x86_64]
[ro.bootimage.build.date]: [2016年 01月 12日 星期二 08:35:17 CST]
[ro.bootimage.build.date.utc]: [1452558917]
[ro.bootimage.build.fingerprint]: [Android-x86/android_x86_64/x86_64:6.0.1/MMB29T/chyyuu01120804:eng/test-keys]
[ro.bootloader]: [unknown]
[ro.bootmode]: [unknown]
[ro.build.characteristics]: [tablet]
[ro.build.date]: [2016年 01月 12日 星期二 08:31:48 CST]
[ro.build.date.utc]: [1452558708]
[ro.build.description]: [android_x86_64-eng 6.0.1 MMB29T eng.chyyuu.20160112.080243 test-keys]
[ro.build.display.id]: [android_x86_64-eng 6.0.1 MMB29T eng.chyyuu.20160112.080243 test-keys]
[ro.build.fingerprint]: [Android-x86/android_x86_64/x86_64:6.0.1/MMB29T/chyyuu01120804:eng/test-keys]
[ro.build.flavor]: [android_x86_64-eng]
[ro.build.host]: [chyyuu-X599]
[ro.build.id]: [MMB29T]
[ro.build.product]: [x86_64]
[ro.build.tags]: [test-keys]
[ro.build.type]: [eng]
[ro.build.user]: [chyyuu]
[ro.build.version.all_codenames]: [REL]
[ro.build.version.base_os]: []
[ro.build.version.codename]: [REL]
[ro.build.version.incremental]: [eng.chyyuu.20160112.080243]
[ro.build.version.preview_sdk]: [0]
[ro.build.version.release]: [6.0.1]
[ro.build.version.sdk]: [23]
[ro.build.version.security_patch]: [2016-01-01]
[ro.carrier]: [unknown]
[ro.com.android.dataroaming]: [true]
[ro.com.android.dateformat]: [MM-dd-yyyy]
[ro.config.alarm_alert]: [Alarm_Classic.ogg]
[ro.config.notification_sound]: [OnTheHunt.ogg]
[ro.config.sync]: [yes]
[ro.crypto.state]: [unencrypted]
[ro.dalvik.vm.isa.arm]: [x86]
[ro.dalvik.vm.isa.arm64]: [x86_64]
[ro.dalvik.vm.native.bridge]: [libnb.so]
[ro.debuggable]: [1]
[ro.enable.native.bridge.exec]: [1]
[ro.enable.native.bridge.exec64]: [1]
[ro.hardware]: [android_x86_64]
[ro.hardware.sensors]: [kbd]
[ro.kernel.android.checkjni]: [1]
[ro.kernel.qemu]: [1]
[ro.opengles.version]: [196608]
[ro.product.board]: []
[ro.product.brand]: [Android-x86]
[ro.product.cpu.abi]: [x86_64]
[ro.product.cpu.abilist]: [x86_64,arm64-v8a,x86,armeabi-v7a,armeabi]
[ro.product.cpu.abilist32]: [x86,armeabi-v7a,armeabi]
[ro.product.cpu.abilist64]: [x86_64,arm64-v8a]
[ro.product.device]: [x86_64]
[ro.product.locale]: [en-US]
[ro.product.manufacturer]: [QEMU]
[ro.product.model]: [Standard PC (i440FX + PIIX, 1996)]
[ro.product.name]: [android_x86_64]
[ro.radio.noril]: [no]
[ro.radio.use-ppp]: [yes]
[ro.revision]: [0]
[ro.ril.gprsclass]: [10]
[ro.ril.hsxpa]: [1]
[ro.rtc_local_time]: [1]
[ro.secure]: [0]
[ro.serialno]: []
[ro.simulated.phone]: [false]
[ro.wifi.channels]: []
[ro.zygote]: [zygote64_32]
[selinux.reload_policy]: [1]
[service.bootanim.exit]: [0]
[status.battery.level]: [5]
[status.battery.level_raw]: [50]
[status.battery.level_scale]: [9]
[status.battery.state]: [Slow]
[sys.media.vdec.drop]: [0]
[sys.settings_global_version]: [2]
[sys.settings_secure_version]: [9]
[sys.settings_system_version]: [1]
[sys.sysctl.extra_free_kbytes]: [5625]
[sys.sysctl.tcp_def_init_rwnd]: [60]
[sys.usb.config]: [adb]
[sys.usb.configfs]: [0]
[sys.usb.state]: [adb]
[vold.has_adoptable]: [0]
[vold.post_fs_data_done]: [1]
```

## [Build.prop重要参数解释](http://www.miui.com/thread-1879888-1-1.html)
```
# begin build properties——开始设置系统性能
# autogenerated by buildinfo.sh——通过设置形成系统信息
ro.build.id=JRO03L——版本ID
ro.build.display.id=JRO03L——版本号
ro.build.version.incremental=4.7.25——版本增量
ro.build.version.sdk=16——sdk版本
ro.build.version.codename=REL——版本代号ro.build.version.release=4.1.1——android版本 
ro.build.date=Fri Jul 25 04:59:41 CST 2014——时区  时间CST可以代表4个时区，这个百度一下 
ro.build.date.utc=1406235581
ro.build.type=user
ro.build.user=builder
ro.build.host=wcc-miui-ota-bd21
ro.build.tags=release-keys
ro.product.model=MI 2——手机型号 
ro.product.brand=Xiaomi——手机品牌
ro.product.name=aries——手机正式名称
ro.product.device=aries——采用的设备
ro.product.board=MSM8960
ro.product.cpu.abi=armeabi-v7a——cpu的版本
ro.product.cpu.abi2=armeabi——cpu的品牌
ro.product.manufacturer=Xiaomi——手机制造商
ro.product.locale.language=zh——刷机后默认语言 
ro.product.locale.region=CN——刷机后启动的默认语言 
ro.wifi.channels=——WIFI连接的渠道
ro.board.platform=msm8960——主板平台
# ro.build.product is obsolete; use ro.product.device
ro.build.product=aries——建立产品
# Do not try to parse ro.build.description or .fingerprint
ro.build.description=aries-user 4.1.1 JRO03L 4.7.25 release-keys——内部版本号 
ro.build.fingerprint=Xiaomi/aries/aries:4.1.1/JRO03L/4.7.25:user/release-keys
ro.build.characteristics=nosdcard
# end build properties——性能代码完毕
#
# system.prop for surf——系统技术支持由surf提供
#

rild.libpath=/system/lib/libril-qc-qmi-1.so
rild.libargs=-d /dev/smd0
persist.rild.nitz_plmn=
persist.rild.nitz_long_ons_0=
persist.rild.nitz_long_ons_1=
persist.rild.nitz_long_ons_2=
persist.rild.nitz_long_ons_3=
persist.rild.nitz_short_ons_0=
persist.rild.nitz_short_ons_1=
persist.rild.nitz_short_ons_2=
persist.rild.nitz_short_ons_3=
ril.subscription.types=RUIM
DEVICE_PROVISIONED=1
debug.sf.hw=1——硬件加速设定 0是关闭, 1是开启
debug.egl.hw=1
debug.composition.type=dyn
debug.mdpcomp.maxlayer=3
debug.mdpcomp.logs=0

ro.sf.lcd_density=320

# save modem ramdump to sdcard
persist.radio.parsedump=1
persist.radio.ramdump_sdcard=0
persist.radio.ramdump_num=3

#
# system props for the cne module
#
#persist.cne.bat.range.low.med=30
#persist.cne.bat.range.med.high=60
#persist.cne.loc.policy.op=/system/etc/OperatorPolicy.xml
#persist.cne.loc.policy.user=/system/etc/UserPolicy.xml
#persist.cne.bwbased.rat.sel=false
#persist.cne.snsr.based.rat.mgt=false
#persist.cne.bat.based.rat.mgt=false
#persist.cne.rat.acq.time.out=30000
#persist.cne.rat.acq.retry.tout=0
#persist.cne.feature=1

ro.hdmi.enable=true
lpa.decode=false
lpa.use-stagefright=true

#system props for the MM modules

media.stagefright.enable-player=true
media.stagefright.enable-http=true
media.stagefright.enable-aac=true
media.stagefright.enable-qcp=true
media.stagefright.enable-fma2dp=true
media.stagefright.enable-scan=true
mmp.enable.3g2=true——这些全改为true会增强系统自带的多媒体效果

#
# system props for the data modules
#
ro.use_data_netmgrd=true

#system props for time-services
#persist.timed.enable=true

# System props for audio
persist.audio.fluence.mode=endfire
persist.audio.vr.enable=false
persist.audio.handset.mic=digital
persist.audio.vns.mode=2

# System prop to select audio resampler quality
af.resampler.quality=255
# System prop to select MPQAudioPlayer by default on mpq8064
mpq.audio.decode=true

#
# system prop for opengles version
#
# 131072 is decimal for 0x20000 to report version 2
ro.opengles.version=131072

#
# system property for Bluetooth Handsfree Profile version
#
ro.bluetooth.hfp.ver=1.6
#
#system prop for Bluetooth hci transport
ro.qualcomm.bt.hci_transport=smd
#
# system prop for requesting Master role in incoming Bluetooth connection.
#
ro.bluetooth.request.master=true
#
# system prop for Bluetooth Auto connect for remote initated connections
#
ro.bluetooth.remote.autoconnect=true
# system property for Bluetooth discoverability time out in seconds
# 0: Always discoverable
#debug.bt.discoverable_time=0

#system prop for switching gps driver to qmi
persist.gps.qmienabled=true

# System property for cabl
ro.qualcomm.cabl=0

# System property for csc
debug.csc.poll=0

# System props for telephony
# System prop to turn on CdmaLTEPhone always
# telephony.lteOnCdmaDevice=1

#
# System prop for sending transmit power request to RIL during WiFi hotspot on/off
#
ro.ril.transmitpower=true

#
#Simulate sdcard on /data/media
#
persist.fuse_sdcard=true
ro.hwui.text_cache_width=2048
ro.hwui.texture_cache_size=48

#
# Supports warmboot capabilities
#
ro.warmboot.capability=1

#snapdragon value add features
ro.qcom.audio.ssr=true

persist.sys.strictmode.disable=true
power.webview.DM=false

#enable cdrom installer
persist.service.cdrom.enable=1

#
# Haptic
#
ro.haptic.vibrate_ex.enabled=1
sys.haptic.long.weak=0,127,10,50,20,-50,10,0,10
sys.haptic.long.normal=0,127,10,80,20,-80,10,0,10
sys.haptic.long.strong=0,127,10,100,20,-100,10,0,10
sys.haptic.down.weak=0,120,10,-50,10,0,10
sys.haptic.down.normal=0,127,10,-80,10,0,10
sys.haptic.down.strong=0,127,20,-80,10,0,10
sys.haptic.up.weak=0,80,30,-50,10,0,10
sys.haptic.up.normal=0,100,30,-100,10,0,10
sys.haptic.up.strong=0,120,30,-120,10,0,10
sys.haptic.tap.weak=0,80,40,-5,5,0,10
sys.haptic.tap.normal=0,100,40,-5,5,0,10
sys.haptic.tap.strong=0,120,40,-5,5,0,10

# power mode
persist.sys.aries.power_profile=middle

# button jack mode and switch
persist.sys.button_jack_profile=volume
persist.sys.button_jack_switch=0

# suspend mode capability
ro.warmboot.capability=true

# display preference
persist.sys.display_prefer=0
persist.sys.display_ce=0
debug.enabletr=false
debug.hwui.render_dirty_regions=false

#add mediascanner skip list
testing.mediascanner.skiplist=/storage/sdcard0/Android/,/storage/sdcard0/MIUI/browser/.readmode/

#
# ADDITIONAL_BUILD_PROPERTIES
#
ro.product.locale.language=zh
ro.product.locale.region=CN
ro.miui.ui.version.code=3
ro.miui.ui.version.name=V5
keyguard.no_require_sim=true
ro.com.android.dateformat=MM-dd-yyyy
ro.carrier=unknown
ro.vendor.extension_library=/system/lib/libqc-opt.so
ro.com.google.clientidbase=android-xiaomi
dalvik.vm.heapstartsize=8m
dalvik.vm.heapgrowthlimit=96m
dalvik.vm.heapsize=384m——虚拟内存，默认是384m 
dalvik.vm.heaputilization=0.25
dalvik.vm.heapidealfree=8388608
dalvik.vm.heapconcurrentstart=2097152
ro.setupwizard.mode=OPTIONAL
ro.com.google.gmsversion=4.1_r6
net.bt.name=Android
dalvik.vm.stack-trace-file=/data/anr/traces.txt
ro.config.ringtone=MI.ogg——默认来电铃声
ro.config.notification_sound=FadeIn.ogg——默认通知铃声
ro.config.alarm_alert=GoodMorning.ogg——默认闹钟铃声 
ro.config.sms_received_sound=FadeIn.ogg
ro.config.sms_delivered_sound=MessageComplete.ogg
ro.com.android.mobiledata=false
persist.sys.mitalk.enable=true
ro.com.android.dataroaming=false



ro.secure=0 默认开启未知源APK 
ro.allow.mock.location=1 开启模拟位置 

ro.debuggable=1 
以上的是从boot.img中提取的 
persist.service.adb.enable=1 
开启调试模式，非常有用

```