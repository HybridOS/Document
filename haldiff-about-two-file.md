##diff分析


##文件：hardware/libhardware/include/hardware/gralloc.h

##区别1：android-x86版本中增加了一组变量定义，如下：
enum {
    GRALLOC_MODULE_PERFORM_GET_DRM_FD                = 0x80000002,
    GRALLOC_MODULE_PERFORM_GET_DRM_MAGIC             = 0x80000003,
    GRALLOC_MODULE_PERFORM_AUTH_DRM_MAGIC            = 0x80000004,
    GRALLOC_MODULE_PERFORM_ENTER_VT                  = 0x80000005,
    GRALLOC_MODULE_PERFORM_LEAVE_VT                  = 0x80000006,
};
具体使用有待考察。



##文件：hardware/libhardware/modules/gralloc/framebuffer.cpp


##区别1：对frambuffer进行map部分的代码存在较大出入如下：
android-x86：
    module->finfo = finfo;
    module->xdpi = xdpi;
    module->ydpi = ydpi;
    module->fps = fps;
    /*
     * map the framebuffer
     */

    while (info.yres_virtual > 0) {
        size_t fbSize = roundUpToPageSize(finfo.line_length * info.yres_virtual);
        module->numBuffers = info.yres_virtual / info.yres;
        void* vaddr = mmap(0, fbSize, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
        if (vaddr != MAP_FAILED) {
            module->info = info;
            module->flags = flags;
            module->bufferMask = 0;
            module->framebuffer = new private_handle_t(dup(fd), fbSize, 0);
            module->framebuffer->base = intptr_t(vaddr);
            memset(vaddr, 0, fbSize);
            return 0;
        }

        ALOGE("Error mapping the framebuffer (%s)", strerror(errno));

        info.yres_virtual -= info.yres;
        ALOGW("Fallback to use fewer buffer: %d", info.yres_virtual / info.yres);
        if (ioctl(fd, FBIOPUT_VSCREENINFO, &info) == -1)
            break;

        if (info.yres_virtual <= info.yres)
            flags &= ~PAGE_FLIP;
    }

    return -errno;
}

AOSP：
    module->flags = flags;
    module->info = info;
    module->finfo = finfo;
    module->xdpi = xdpi;
    module->ydpi = ydpi;
    module->fps = fps;
    /*
     * map the framebuffer
     */

    int err;
    size_t fbSize = roundUpToPageSize(finfo.line_length * info.yres_virtual);
    module->framebuffer = new private_handle_t(dup(fd), fbSize, 0);

    module->numBuffers = info.yres_virtual / info.yres;
    module->bufferMask = 0;

    void* vaddr = mmap(0, fbSize, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
    if (vaddr == MAP_FAILED) {
        ALOGE("Error mapping the framebuffer (%s)", strerror(errno));
        return -errno;
    }
    module->framebuffer->base = intptr_t(vaddr);
    memset(vaddr, 0, fbSize);
    return 0;
}

差异原因：
为了阐明差异原因首先需要对这段代码进行理解。
首先是以下几个变量：
info.yres_virtual:虚拟纵向分辨率
info.yres:实际纵向分辨率
通过上面两条变量我们可以得知：
	module->numBuffers = info.yres_virtual / info.yres;
语句计算了整个系统帧缓冲区可以划分为多少个图形缓冲区。
在考虑下面这个变量：
finfo.line_length：屏幕以一行显示所需要的内存size。
我们便可以得出
	size_t fbSize = roundUpToPageSize(finfo.line_length * info.yres_virtual);
这条语句首先计算了整个缓冲区的大小，之后利用roundUpToPageSize将整个缓冲区大小对齐到页面边界（？）。即可算出需要的内存大小。
之后我们可以看到，该段代码调用了mmap方法进行内存分配。并根据内存分配的结果对module的内容进行修改之后返回。
而两段代码最大的区别便是出现在这部分。
android-x86代码在这里提出了一个循环：
若是分配成功，没的说，将module内容赋值并return 0。这点和AOSP的代码实际上逻辑是一样的。但当分配失败的时候，AOSP部分的代码会输出错误信息并返回-error。但android-x86代码则会调用
	info.yres_virtual -= info.yres;
这条语句对虚拟分辨率进行修改。将其减去一个实际分辨率。从之前的变量设计来看，这样会导致重新调用mmap时所申请的内存减小。在改动后，首先调用ioctl函数将修改后的信息进行FBIOPUT操作，若能成功，则进一步判断修改后的虚拟分辨率与实际分辨率的大小关系，并对flag进行修改。之后，在虚拟分辨率没有下降到0以下的情况下进行循环，再度申请内存。一直到申请成功或满足跳出条件位置，若跳出循环则输出-errono。

根据上面的差异分析，可以初步认为android-x86对AOSP中的为帧缓冲区申请内存的算法进行了优化。这种优化的目的尚不清楚。


##区别2：android-x86中fb_device_open方法中获取从底层读取的设备参数时有一定的改动。

android-x86：
/*
             * Auto detect current depth and select mode
             */
            int format;
            if (m->info.bits_per_pixel == 32) {
                format = (m->info.red.offset == 16) ? HAL_PIXEL_FORMAT_BGRA_8888
                       : (m->info.red.offset == 24) ? HAL_PIXEL_FORMAT_RGBA_8888
                       : HAL_PIXEL_FORMAT_RGBX_8888;
            } else if (m->info.bits_per_pixel == 16) {
                format = HAL_PIXEL_FORMAT_RGB_565;
            } else {
                ALOGE("Unsupported format %d", m->info.bits_per_pixel);
                return -EINVAL;
            }

AOSP：
int format = (m->info.bits_per_pixel == 32)
                         ? (m->info.red.offset ? HAL_PIXEL_FORMAT_BGRA_8888 : HAL_PIXEL_FORMAT_RGBX_8888)
                         : HAL_PIXEL_FORMAT_RGB_565;

差别原因：我们可以看到，android-x86的代码相比AOSP来说多了几个设备参数，并增加了对它们获取条件的判断（例如HAL_PIXEL_FORMAT_RGBA_8888）。根据查阅资料，这些大写字母构成的format参数均代表图形缓冲区的颜色格式，而决定它们的是每一个像素由多少位来表示。因此初步认为本部分代码的目的是对新增的颜色格式进行支持。同时对AOSP中颜色格式选择的参数也进行修改（例如选择HAL_PIXEL_FORMAT_RGB_565的条件）。