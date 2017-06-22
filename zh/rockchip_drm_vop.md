# VOP驱动解读

代码位置:

    drivers/gpu/drm/rockchip/rockchip_drm_vop.c
    drivers/gpu/drm/rockchip/rockchip_vop_reg.c

结构介绍:

    struct vop;
    // vop 驱动根结构, 一个vop对应一个struct vop结构

    struct vop_win;
    // 描述图层信息, 一个硬件图层对应一个struct vop_win结构

寄存器读写:

    为了兼容各种不同版本的vop, vop驱动里面使用了寄存器级的抽象, 由一个结构体来保
    存抽象关系, 这样主体逻辑只需要操作抽象后的功能定义, 由抽象的读写接口根据抽象
    关系写到真实的vop硬件中.

    示例:
      static const struct vop_win_phy rk3288_win23_data = {
           .enable = VOP_REG(RK3288_WIN2_CTRL0, 0x1, 4),
      }
      static const struct vop_win_phy rk3368_win23_data = {
           .enable = VOP_REG(RK3368_WIN2_CTRL0, 0x1, 4),
      }
    rk3368和rk3288图层的地址分布不同, 但在结构定义的时候, 可以将不同的硬件图层bit
    映射到同一个enable功能上, 这样vop驱动主体调用VOP_WIN_SET(vop, win, enable, 1);
    时就能操作到真实的vop寄存器了.

图层接口:

    static const struct drm_plane_helper_funcs plane_helper_funcs = {
        // 预先对图层进行处理
    	.prepare_fb = vop_plane_prepare_fb,
        // 图层显示完成后的处理
    	.cleanup_fb = vop_plane_cleanup_fb,
        // 在显示前进行参数检查
    	.atomic_check = vop_plane_atomic_check,
        // 更新图层参数
    	.atomic_update = vop_plane_atomic_update,
        // 关闭图层
    	.atomic_disable = vop_plane_atomic_disable,
    };

vop接口:

    static const struct drm_crtc_helper_funcs vop_crtc_helper_funcs = {
        // 使能vop, 在这里面会将timing配好
    	.enable = vop_crtc_enable,
        // 关闭vop
    	.disable = vop_crtc_disable,
        // 对timing进行检查修正
    	.mode_fixup = vop_crtc_mode_fixup,
        // 在一帧显示开始前做的处理
    	.atomic_begin = vop_crtc_atomic_begin,
        // 检查显示的参数
    	.atomic_check = vop_crtc_atomic_check,
        // 提交硬件显示
    	.atomic_flush = vop_crtc_atomic_flush,
    };
