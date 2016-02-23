#与qemu/kvm相关的信息

## qemu相关
###编译/安装qemu
 - http://qemu-project.org/Hosts/Linux

```
sudo apt-get install git libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev
sudo apt-get install git-email
sudo apt-get install libaio-dev libbluetooth-dev libbrlapi-dev libbz2-dev
sudo apt-get install libcap-dev libcap-ng-dev libcurl4-gnutls-dev libgtk-3-dev
sudo apt-get install librbd-dev librdmacm-dev
sudo apt-get install libsasl2-dev libsdl1.2-dev libseccomp-dev libsnappy-dev libssh2-1-dev
sudo apt-get install libvde-dev libvdeplug-dev libvte-2.90-dev libxen-dev liblzo2-dev
sudo apt-get install valgrind xfslibs-dev 
sudo apt-get install libnfs-dev libiscsi-dev
cd qemu
mkdir -p bin/debug/native
../../../configure --target-list=x86_64-softmmu --enable-debug

OR
# because now virgl opt is in gtk
../../../configure --target-list=x86_64-softmmu --enable-debug ­­--enable-gtk --with-gtkabi=3.0 ­­--enable-kvm

make -j8
```
## kvm相关

## mesa 相关
### setup env
```
sudo apt-get build-dep mesa 
```
### related libs
 - http://glew.sourceforge.net/ need by mesa-demos
 - git://anongit.freedesktop.org/mesa/drm need by mesa
 - http://nouveau.freedesktop.org/wiki/InstallNouveau/
 - http://cgit.freedesktop.org/mesa/demos mesa-demos //need apt-get install libglew-dev

### 编译android-x86中的mesa
```
mmm -j8 --trace external/mesa/ 2>&1|tee /tmp/make.log
```

## kernel/initrd 相关
### 编译linux kernel及制作initrd
 - [编译linux kernel及制作initrd ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/8546687)
 - https://github.com/chyyuu/tiny_linux_ChildIsTalent

在ramdisk目录下，建立了所需文件后，可通过如下命令生成ramdisk
```
cd ramdisk
find . -print0 | cpio --null -ov --format=newc | gzip -9 > ../initrd-new3.img.gz
```
## 运行kernel 
### 只运行kernel
```
./qemu/bin/debug/native/x86_64-softmmu/qemu-system-x86_64 -kernel ./linux-image/boot/vmlinuz-4.4.0-040400rc8-generic -append "console=ttyS0" -serial stdio
```
### 运行kernel+ramdisk
```
./qemu/bin/debug/native/x86_64-softmmu/qemu-system-x86_64 -enable-kvm -kernel ./linux-image/boot/vmlinuz-4.4.0-040400rc8-generic -initrd ./qemu_debug_linux/rootfs_64.img.gz -append "root=/dev/ram rdinit=/sbin/init console=ttyS0" -serial stdio
```

### 加载ko
```
 # ls lib/modules/4.4.0-040400rc8-generic/kernel/drivers/gpu/drm/virtio/
virtio-gpu.ko
/ # modprobe virtio-gpu
[   63.943318] [drm] Initialized drm 1.1.0 20060810
# lsmod
virtio_gpu 53248 0 - Live 0xffffffffc00d3000
ttm 94208 1 virtio_gpu, Live 0xffffffffc0059000
drm_kms_helper 135168 1 virtio_gpu, Live 0xffffffffc0097000
sysimgblt 16384 1 drm_kms_helper, Live 0xffffffffc0090000
sysfillrect 16384 1 drm_kms_helper, Live 0xffffffffc0089000
syscopyarea 16384 1 drm_kms_helper, Live 0xffffffffc0082000
fb_sys_fops 16384 1 drm_kms_helper, Live 0xffffffffc007b000
drm 360448 3 virtio_gpu,ttm,drm_kms_helper, Live 0xffffffffc0000000
```

run android-x86
```
/home/chyyuu/develop/qemu-related/qemu/bin/test/x86_64-softmmu/qemu-system-x86_64 -enable-kvm -m 4G -cdrom /media/chyyuu/opt2/android-x86/out/target/product/x86_64/android_x86_64.iso -device virtio-gpu-pci,virgl -display gtk,gl=on -device virtio-mouse-pci -device virtio-keyboard-pci -d guest_errors -serial stdio
```
## 备注
### 加上 virgl

#### device
```
文件
android-repo/device/generic/common/BoardConfig.mk
修改
BOARD_GPU_DRIVERS ?= i915 i965 nouveau r300g r600g swrast virgl
```

#### mesa
使用v11.1.0+

```
diff --git a/Android.mk b/Android.mk
index ed160fb..1d76559 100644
--- a/Android.mk
+++ b/Android.mk
@@ -24,7 +24,7 @@
 # BOARD_GPU_DRIVERS should be defined.  The valid values are
 #
 #   classic drivers: i915 i965
-#   gallium drivers: swrast freedreno i915g ilo nouveau r300g r600g radeonsi vc4 vmwgfx
+#   gallium drivers: swrast freedreno i915g ilo nouveau r300g r600g radeonsi vc4 virgl vmwgfx
 #
 # The main target is libGLES_mesa.  For each classic driver enabled, a DRI
 # module will also be built.  DRI modules will be loaded by libGLES_mesa.
@@ -46,7 +46,7 @@ MESA_COMMON_MK := $(MESA_TOP)/Android.common.mk
 MESA_PYTHON2 := python
 
 classic_drivers := i915 i965
-gallium_drivers := swrast freedreno i915g ilo nouveau r300g r600g radeonsi vmwgfx vc4
+gallium_drivers := swrast freedreno i915g ilo nouveau r300g r600g radeonsi vmwgfx vc4 virgl
 
 MESA_GPU_DRIVERS := $(strip $(BOARD_GPU_DRIVERS))
 
diff --git a/include/pci_ids/virtio_gpu_pci_ids.h b/include/pci_ids/virtio_gpu_pci_ids.h
new file mode 100644
index 0000000..2e6ecaf
--- /dev/null
+++ b/include/pci_ids/virtio_gpu_pci_ids.h
@@ -0,0 +1 @@
+CHIPSET(0x0010, VIRTGL, VIRTGL)
diff --git a/src/gallium/Android.mk b/src/gallium/Android.mk
index b406d4a..749be7d 100644
--- a/src/gallium/Android.mk
+++ b/src/gallium/Android.mk
@@ -83,6 +83,11 @@ ifneq ($(filter vc4, $(MESA_GPU_DRIVERS)),)
 SUBDIRS += winsys/vc4/drm drivers/vc4
 endif
 
+# virgl
+ifneq ($(filter virgl, $(MESA_GPU_DRIVERS)),)
+SUBDIRS += winsys/virgl/drm drivers/virgl
+endif
+
 # vmwgfx
 ifneq ($(filter vmwgfx, $(MESA_GPU_DRIVERS)),)
 SUBDIRS += winsys/svga/drm drivers/svga
diff --git a/src/gallium/drivers/virgl/Android.mk b/src/gallium/drivers/virgl/Android.mk
new file mode 100644
index 0000000..b8309e4
--- /dev/null
+++ b/src/gallium/drivers/virgl/Android.mk
@@ -0,0 +1,35 @@
+# Copyright (C) 2014 Emil Velikov <emil.l.velikov@gmail.com>
+#
+# Permission is hereby granted, free of charge, to any person obtaining a
+# copy of this software and associated documentation files (the "Software"),
+# to deal in the Software without restriction, including without limitation
+# the rights to use, copy, modify, merge, publish, distribute, sublicense,
+# and/or sell copies of the Software, and to permit persons to whom the
+# Software is furnished to do so, subject to the following conditions:
+#
+# The above copyright notice and this permission notice shall be included
+# in all copies or substantial portions of the Software.
+#
+# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
+# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
+# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL
+# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
+# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
+# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
+# DEALINGS IN THE SOFTWARE.
+
+LOCAL_PATH := $(call my-dir)
+
+# get C_SOURCES
+include $(LOCAL_PATH)/Makefile.sources
+
+include $(CLEAR_VARS)
+
+LOCAL_SRC_FILES := \
+	$(C_SOURCES)
+
+LOCAL_SHARED_LIBRARIES := libdrm
+LOCAL_MODULE := libmesa_pipe_virgl
+
+include $(GALLIUM_COMMON_MK)
+include $(BUILD_STATIC_LIBRARY)
diff --git a/src/gallium/targets/dri/Android.mk b/src/gallium/targets/dri/Android.mk
index 32f5d52..3cde233 100644
--- a/src/gallium/targets/dri/Android.mk
+++ b/src/gallium/targets/dri/Android.mk
@@ -92,6 +92,10 @@ ifneq ($(filter vc4,$(MESA_GPU_DRIVERS)),)
 LOCAL_CFLAGS += -DGALLIUM_VC4
 gallium_DRIVERS += libmesa_winsys_vc4 libmesa_pipe_vc4
 endif
+ifneq ($(filter virgl,$(MESA_GPU_DRIVERS)),)
+LOCAL_CFLAGS += -DGALLIUM_VIRGL
+gallium_DRIVERS += libmesa_winsys_virgl libmesa_pipe_virgl
+endif
 ifneq ($(filter vmwgfx,$(MESA_GPU_DRIVERS)),)
 gallium_DRIVERS += libmesa_winsys_svga libmesa_pipe_svga
 LOCAL_CFLAGS += -DGALLIUM_VMWGFX
diff --git a/src/gallium/winsys/virgl/drm/Android.mk b/src/gallium/winsys/virgl/drm/Android.mk
new file mode 100644
index 0000000..8493503
--- /dev/null
+++ b/src/gallium/winsys/virgl/drm/Android.mk
@@ -0,0 +1,34 @@
+# Copyright (C) 2014 Emil Velikov <emil.l.velikov@gmail.com>
+#
+# Permission is hereby granted, free of charge, to any person obtaining a
+# copy of this software and associated documentation files (the "Software"),
+# to deal in the Software without restriction, including without limitation
+# the rights to use, copy, modify, merge, publish, distribute, sublicense,
+# and/or sell copies of the Software, and to permit persons to whom the
+# Software is furnished to do so, subject to the following conditions:
+#
+# The above copyright notice and this permission notice shall be included
+# in all copies or substantial portions of the Software.
+#
+# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
+# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
+# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL
+# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
+# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
+# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
+# DEALINGS IN THE SOFTWARE.
+
+LOCAL_PATH := $(call my-dir)
+
+# get C_SOURCES
+include $(LOCAL_PATH)/Makefile.sources
+
+include $(CLEAR_VARS)
+
+LOCAL_SRC_FILES := $(C_SOURCES)
+
+LOCAL_SHARED_LIBRARIES := libdrm
+LOCAL_MODULE := libmesa_winsys_virgl
+
+include $(GALLIUM_COMMON_MK)
+include $(BUILD_STATIC_LIBRARY)
diff --git a/src/loader/pci_id_driver_map.h b/src/loader/pci_id_driver_map.h
index 11e39d3..cab69fb 100644
--- a/src/loader/pci_id_driver_map.h
+++ b/src/loader/pci_id_driver_map.h
@@ -53,6 +53,12 @@ static const int radeonsi_chip_ids[] = {
 #undef CHIPSET
 };
 
+static const int virtio_gpu_chip_ids[] = {
+#define CHIPSET(chip, name, family) chip,
+#include "pci_ids/virtio_gpu_pci_ids.h"
+#undef CHIPSET
+};
+
 static const int vmwgfx_chip_ids[] = {
 #define CHIPSET(chip, name, family) chip,
 #include "pci_ids/vmwgfx_pci_ids.h"
@@ -78,6 +84,7 @@ static const struct {
    { 0x1002, "radeonsi", radeonsi_chip_ids, ARRAY_SIZE(radeonsi_chip_ids), _LOADER_GALLIUM},
    { 0x10de, "nouveau_vieux", NULL, -1,  _LOADER_DRI, is_nouveau_vieux },
    { 0x10de, "nouveau", NULL, -1,  _LOADER_GALLIUM },
+   { 0x1af4, "virtio_gpu", virtio_gpu_chip_ids, ARRAY_SIZE(virtio_gpu_chip_ids), _LOADER_GALLIUM },
    { 0x15ad, "vmwgfx", vmwgfx_chip_ids, ARRAY_SIZE(vmwgfx_chip_ids), _LOADER_GALLIUM },
    { 0x0000, NULL, NULL, 0 },
 };
```
#### drm_gralloc

#### drm_hwcomposer

#### libdrm
没有啥变化

## Troubleshooting
### Error: uvesafb cannot reserve memory in qemu/kvm
Solution:
```
Check if you forgot to remove any vga=xxx kernel parameter -- this overrides the UVESA framebuffer with a standard VESA one.
Or try to add "video=vesa:off vga=normal" to kernel cmdline.
```

### Error: "pci_root PNP0A08:00 address space collision + Uvesafb cannot reserve memory"
Solution:
```
This occurs on the qemu; whether it also occurs on other systems is unknown. Even without another framebuffer interfering with the uvesafb setup, uvesafb cannot reserve the necessary memory region.
You can fix this issue by adding the following to the kernel parameters in your bootloader's configuration.
pci=nocrs
```