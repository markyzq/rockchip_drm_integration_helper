#一级休眠

drm 原生只支持pm runtime 的休眠方式，不支持一级二级休眠，为了支持android的一级
休眠功能，在drm上采用如下的方式进行一级休眠：

    使能CONFIG_DRM_FBDEV_EMULATION功能

    echo 3 > /sys/class/graphics/fb0/blank //进入一级待机
    echo 0 > /sys/class/graphics/fb0/blank //退出一级待机

    使用fbdev的blank时，kernel会发notify到需要一级待机的设备，如touchscreen,
    keyboard等，将这些设备suspend掉

#二级休眠

    走pm runtime通路
