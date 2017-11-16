# 用户空间内存操作

为了便于说明, 额外定义一个外部内存结构:

    // drm相关操作需要引用该头文件
    #include <drm.h>
    struct bo {
       int fd;
       void *ptr;
       size_t size;
       size_t offset;
       size_t pitch;
       unsigned handle;
    };

获取设备节点:

    bo->fd = open("/dev/dri/card0", O_RDWR, 0);

alloc

    struct drm_mode_create_dumb arg;
    int handle, size, pitch;
    int ret;
    memset(&arg, 0, sizeof(arg));
    arg.bpp = bpp;
    arg.width = width;
    arg.height = height;
    ret = drmIoctl(bo->fd, DRM_IOCTL_MODE_CREATE_DUMB, &arg);
    if (ret) {
        fprintf(stderr, "failed to create dumb buffer: %s\n", strerror(errno));
        return ret;
    }
    bo->handle = arg.handle;
    bo->size = arg.size;
    bo->pitch = arg.pitch;

mmap

    struct drm_mode_map_dumb arg;
    void *map;
    int ret;

    memset(&arg, 0, sizeof(arg));
    arg.handle = bo->handle;
    ret = drmIoctl(fd, DRM_IOCTL_MODE_MAP_DUMB, &arg);
    if (ret)
        return ret;
    map = mmap(0, bo->size, PROT_READ | PROT_WRITE, MAP_SHARED, fd, arg.offset);
    //64位
    map = mmap64(0, bo->size, PROT_READ | PROT_WRITE, MAP_SHARED, fd, arg.offset);
    if (map == MAP_FAILED)
       return -EINVAL;
    bo->ptr = map

unmap

    drm_munmap(bo->ptr, bo->size);
    bo->ptr = NULL;

free

    struct drm_mode_destroy_dumb arg;
    int ret;

    memset(&arg, 0, sizeof(arg));
    arg.handle = bo->handle;
    ret = drmIoctl(bo->fd, DRM_IOCTL_MODE_DESTROY_DUMB, &arg);
    if (ret)
        fprintf(stderr, "failed to destroy dumb buffer: %s\n", strerror(errno));

free gem handle:

    // gem handle减引用操作
    struct drm_gem_close args;
    memset(&args, 0, sizeof(args));
    args.handle = bo->handle;
    drmIoctl(bo->fd, DRM_IOCTL_GEM_CLOSE, &args);

export dmafd

    int export_dmafd;
    ret = drmPrimeHandleToFD(bo->fd, bo->handle, 0, &export_dmafd);
    // drmPrimeHandleToFD是会给dma_buf加引用计数的
    // 使用完export_dmafd后, 需要使用close(export_dmafd)来减掉引用计数

import dmafd

    ret = drmPrimeFDToHandle(bo->fd, import_dmafd, &bo->handle);
    // drmPrimeHandleToFD是会给dma_buf加引用计数的
    // 使用完bo->handle后, 需要对handle减引用, 参看free gem handle部分.
