# Rockchip drm主设备

#设备device tree:

    display_subsystem: display-subsystem {
    	compatible = "rockchip,display-subsystem";
    	ports = <&vopl_out>, <&vopb_out>;
    	status = "disabled";
    };

    - compatible: Should be "rockchip,display-subsystem"
    - ports: Should contain a list of phandles pointing to display interface port
      of vop devices. vop definitions as defined in
      kernel/Documentation/devicetree/bindings/display/rockchip/rockchip-vop.txt

#代码位置:

    drivers/gpu/drm/rockchip/rockchip_drm_drv.c
    drivers/gpu/drm/rockchip/rockchip_drm_drv.h

#主要结构:
    static struct drm_driver rockchip_drm_driver = {
    	.driver_features	= DRIVER_MODESET | DRIVER_GEM |
    				  DRIVER_PRIME | DRIVER_ATOMIC |
    				  DRIVER_RENDER,
    	.preclose		= rockchip_drm_preclose,
    	.lastclose		= rockchip_drm_lastclose,
    	.get_vblank_counter	= drm_vblank_no_hw_counter,
    	.open			= rockchip_drm_open,
    	.postclose		= rockchip_drm_postclose,
    	.enable_vblank		= rockchip_drm_crtc_enable_vblank,
    	.disable_vblank		= rockchip_drm_crtc_disable_vblank,
    	.gem_vm_ops		= &rockchip_drm_vm_ops,
    	.gem_free_object	= rockchip_gem_free_object,
    	.dumb_create		= rockchip_gem_dumb_create,
    	.dumb_map_offset	= rockchip_gem_dumb_map_offset,
    	.dumb_destroy		= drm_gem_dumb_destroy,
    	.prime_handle_to_fd	= drm_gem_prime_handle_to_fd,
    	.prime_fd_to_handle	= drm_gem_prime_fd_to_handle,
    	.gem_prime_import	= drm_gem_prime_import,
    	.gem_prime_export	= drm_gem_prime_export,
    	.gem_prime_get_sg_table	= rockchip_gem_prime_get_sg_table,
    	.gem_prime_import_sg_table	= rockchip_gem_prime_import_sg_table,
    	.gem_prime_vmap		= rockchip_gem_prime_vmap,
    	.gem_prime_vunmap	= rockchip_gem_prime_vunmap,
    	.gem_prime_mmap		= rockchip_gem_mmap_buf,
    #ifdef CONFIG_DEBUG_FS
    	.debugfs_init		= rockchip_drm_debugfs_init,
    	.debugfs_cleanup	= rockchip_drm_debugfs_cleanup,
    #endif
    	.ioctls			= rockchip_ioctls,
    	.num_ioctls		= ARRAY_SIZE(rockchip_ioctls),
    	.fops			= &rockchip_drm_driver_fops,
    	.name	= DRIVER_NAME,
    	.desc	= DRIVER_DESC,
    	.date	= DRIVER_DATE,
    	.major	= DRIVER_MAJOR,
    	.minor	= DRIVER_MINOR,
    };

