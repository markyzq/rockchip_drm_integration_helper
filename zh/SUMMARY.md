# Summary

* [概述](introduce.md)
* [概念解析]
 - [Framebuffer](component_framebuffer.md)
 - [Crtc](component_crtc.md)
 - [Planes](component_planes.md)
 - [Encoder](component_encoder.md)
 - [Connector](component_connector.md)
* [设备节点](drm_devices_node.md)
* [内存相关]
 - [userspace内存操作](drm_userspace_memory.md)
* [常用操作]
 - [设置显示](ioctls_setcrtc.md)
* [drm/rockchip]
 - [文件清单](rockchip_code_list.md)
 - [component框架](component_framework.md)
 - [启动过程](rockchip_drm_probe.md)
 - [rockchip主驱动](rockchip_drm_drv.md)
 - [vop驱动](rockchip_drm_vop.md)
* [硬件参数]
 - [vop硬件版本](rockchip_hardware_vop_version.md)
* [特殊drm config]
 - [CONFIG_DRM_IGNORE_IOTCL_PERMIT](drm_config_ignore_ioctl_premit.md)
 - [CONFIG_DRM_FBDEV_EMULATION](drm_config_fbdev_emulation.md)
 - [CONFIG_DRM_LOAD_EDID_FIRMWARE](drm_config_load_firmware.md)
* [休眠]
 - [android一级二级休眠](drm_suspend.md)
* [DRM device Tree解析](drm_devicetree.md)
* [屏相关配置]
 - [屏配置的几个方式](generic_panel.md)
 - [edp屏点亮步骤](bringup_edp_panel.md)
 - [LVDS配置](lvds_panel.md)
* [开机logo配置]
 - [device tree配置说明](logo_devicetree.md)
 - [双屏异显](logo_dualdisplay.md)
 - [多屏抢占及热拔插](multi_grab_and_hotplug.md)
 - [支持的logo图片](supported_logo.md)
* [调试手段]
 - [dump当前显示状态](drm_dump_summary.md)
 - [dump当前drm使用的buffer](drm_dump_mm.md)
 - [modetest使用](drm_modetest.md)
 - [调整drm log等级](drm_log_level.md)
 - [查看当前显示的时钟情况](drm_display_clock.md)
 - [查看当前gpio的配置](drm_display_gpio.md)
 - [强行使能显示设备](drm_force_enable.md)
* [常见问题]
* [参者文献](reference.md)
