# 理解init.sh 和　init.x86.rc
位于 device/generic/common git repo中
## 启动过程
下面代码可以看出，正常情况下就是执行do_init函数，这个函数完成一系列初始化
```
# import the vendor specific script
hw_sh=/vendor/etc/init.sh
[ -e $hw_sh ] && source $hw_sh

case "$1" in
	netconsole)
		[ -n "$DEBUG" ] && do_netconsole
		;;
	bootcomplete)
		do_bootcomplete
		;;
	hci)
		do_hci
		;;
	init|"")
		do_init
		;;
esac

return 0

function do_init()
{
	init_misc
	init_hal_audio
	init_hal_bluetooth
	init_hal_camera
	init_hal_gps
	init_hal_gralloc
	init_hal_hwcomposer
	init_hal_lights
	init_hal_power
	init_hal_sensors
	init_tscal
	init_ril
	chmod 640 /x86.prop
	post_init
}
```