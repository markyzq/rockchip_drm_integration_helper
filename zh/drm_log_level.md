设置drm的调试log等级:

    sys结点位置: /sys/module/drm/parameters/debug

    debug:Enable debug output, where each bit enables a debug category.
        Bit 0 (0x01) will enable CORE messages (drm core code)
        Bit 1 (0x02) will enable DRIVER messages (drm controller code)
        Bit 2 (0x04) will enable KMS messages (modesetting code)
        Bit 3 (0x08) will enable PRIME messages (prime code)
        Bit 4 (0x10) will enable ATOMIC messages (atomic code)
        Bit 5 (0x20) will enable VBL messages (vblank code) (int)

    示例:　echo 0x0c > /sys/module/drm/parameters/debug
         打开了KMS, PRIME这些的打印, kernel驱动中使用DRM_DEBUG_KMS和
         DRM_DEBUG_PRIME打印的信息都可以打印出来
