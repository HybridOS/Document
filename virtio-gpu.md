Instructions for building Android-x86 Marshmallow using mesa/DRM graphics stack. Currently supported is virtio-gpu on QEMU (x86 KVM).

### Install dependent packages
This is for ubuntu and assuming your machine is already setup for building kernel and QEMU.

`sudo apt-get install libgbm-dev libsdl2-dev libgtk-3-dev libgles2-mesa-dev libpixman-1-dev libtool autoconf`

### Build virglrenderer

- `git clone git://people.freedesktop.org/~airlied/virglrenderer`
- `./autogen.sh`
- HACK: Remove the line with CODE_COVERAGE_RULES in generated Makefile. Alternatively, install the necessary dependency.
- `make`
- `sudo make install`

### Build QEMU
- fetch current mainline (ver-2.5+)
- HACK: Mouse input with GTK 3.16 and OpenGL appears to be broken. Work-around is edit configure script and force ‘gtk_gl=”no”’.
- `./configure --target-list=x86_64-softmmu --enable-gtk --with-gtkabi=3.0 --enable-kvm`
- `make`

### Build Android-x86 

- `repo init -u git://gitscm.sf.net/gitroot/android-x86/manifest -b marshmallow-x86`
- `repo sync -j10`
- `cd external/drm_gralloc`
- `git fetch x86 virgl`
- `git checkout x86/virgl`
- `cd ../..`
- `cd kernel`
- `git fetch x86 kernel-4.4`
- `git checkout x86/kernel-4.4`
- `cd ..`
- `lunch` Select android_x86_64-eng
- `make iso_img -j8`

### Run QEMU
This is the script I use:

```
#!/bin/sh

${QEMU_PATH}/qemu-system-x86_64 \
    -enable-kvm \
	-m 1024 \
	-serial stdio \
	-cdrom  ${ANDROID_IMAGE_PATH}/android_x86_64.iso\
	-device virtio-gpu-pci,virgl -display gtk,gl=on
```

Then press "ctrl-alt-2" to see the GPU-based screen in qemu.


## Reference
 - https://github.com/robherring/generic_device/wiki/Android-with-DRM-mesa-graphics