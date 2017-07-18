#开机logo device tree配置说明

参考配置: arch/arm64/boot/dts/rockchip/rk3399-android.dtsi

DeviceTree 解析:

    reserved-memory {

    	//在reserved memory划一块内存做为logo使用
    	drm_logo: drm-logo@00000000 {
    		compatible = "rockchip,drm-logo";

    		//size填0, uboot将会根据实际logo占用buffer的大小分配
    		//用户无需填写
    		reg = <0x0 0x0 0x0 0x0>;
    	};
    };

    &display_subsystem {

    	//drm驱动将解析drm_logo的buffer, 用于logo显示
    	memory-region = <&drm_logo>;
    	route {
    		//每一组route_xxx代表一路显示输出
    		route_hdmi: route-hdmi {

    			//在uboot loader阶段所使用的图片, 名称可根据resource.img打包的logo图片名称定义.
    			//当该属性留空或者指定的图片找不到时, uboot将不显示
    			logo,uboot = "logo.bmp";

    			//在uboot loader阶段所使用的图片, 名称可根据resource.img打包的logo图片名称定义.
    			//当该属性留空或者指定的图片找不到时, uboot将不显示
    			logo,kernel = "logo_kernel.bmp";

    			//支持两种显示模式: "fullscreen"全屏和"center'居中
    			logo,mode = "fullscreen";

    			//充电logo, 支持两种显示模式: "fullscreen"全屏和"center'居中
    			charge_logo,mode = "center";

    			//指定具体的显示通路, 如下, 为hdmi使用vopb输出
    			connect = <&vopb_out_hdmi>;
    		};
    		route_mipi: route-mipi {
    			logo,uboot = "logo_mipi.bmp";
    			logo,kernel = "logo_kernel_mipi.bmp";
    			logo,mode = "fullscreen";
    			charge_logo,mode = "center";
    			connect = <&vopb_out_mipi>;
    		};
    		route_edp: route-edp {
    			logo,uboot = "logo_edp.bmp";
    			logo,kernel = "logo_kernel_edp.bmp";
    			logo,mode = "fullscreen";
    			charge_logo,mode = "center";
    			connect = <&vopb_out_edp>;
    		};
    	};
    };
