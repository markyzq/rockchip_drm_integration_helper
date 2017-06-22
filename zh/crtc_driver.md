# Crtc驱动详解

Drm 的Crtc对应到rockchip平台上,即可vop驱动

源码位置:

    drivers/gpu/drm/rockchip/rockchip_drm_vop.c
    drivers/gpu/drm/rockchip/rockchip_drm_vop.h
    drivers/gpu/drm/rockchip/rockchip_vop_reg.c
    drivers/gpu/drm/rockchip/rockchip_vop_reg.h

已支持验证的芯片

    rockchip,rk3036-vop
    rockchip,rk3288-vop
    rockchip,rk3399-vop-big
    rockchip,rk3399-vop-lit

代码支持但还未验证的:

    rockchip,rk3368-vop
    rockchip,rk3366-vop
    rockchip,rk322x-vop

驱动详解:


