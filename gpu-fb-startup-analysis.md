#理解gpu fb初始化
在用qemu模拟执行时，看到没有识别出qemu支持的virtio-gpu，这部分代码位于
```
01-11 07:36:40.060  1104  1104 D libEGL  : 3D hardware acceleration is disabled
01-11 07:36:40.060  1104  1104 D libEGL  : Emulator without GPU support detected. Fallback to software renderer.
```
查找字符串，可以定位到源码 framework/native/opengl/libs/EGL/Loader.cpp中checkGlesEmulationStatus(void)函数
```
/* This function is called to check whether we run inside the emulator,
 * and if this is the case whether GLES GPU emulation is supported.
 *
 * Returned values are:
 *  -1   -> not running inside the emulator
 *   0   -> running inside the emulator, but GPU emulation not supported
 *   1   -> running inside the emulator, GPU emulation is supported
 *          through the "emulation" config.
 */
static int
checkGlesEmulationStatus(void)
{
    /* We're going to check for the following kernel parameters:
     *
     *    qemu=1                      -> tells us that we run inside the emulator
     *    android.qemu.gles=<number>  -> tells us the GLES GPU emulation status
     *
     * Note that we will return <number> if we find it. This let us support
     * more additionnal emulation modes in the future.
     */
    char  prop[PROPERTY_VALUE_MAX];
    int   result = -1;

    /* Check if hardware acceleration disabled explicitly */
    property_get("debug.egl.hw", prop, "1");
    if (!atoi(prop)) {
        ALOGD("3D hardware acceleration is disabled");
        return 0;
    }

    /* First, check for qemu=1 */
    property_get("ro.kernel.qemu",prop,"0");
    if (atoi(prop) != 1)
        return -1;

    /* We are in the emulator, get GPU status value */
    property_get("ro.kernel.qemu.gles",prop,"0");
    return atoi(prop);
}
```
而在getprop命令的输出中，可以看到
```
[ro.kernel.qemu]: [1]
```
而ro.kernel.qemu.gles的设置决定了qemu是否支持opengl
在目前的getprop中，找不到这个属性。