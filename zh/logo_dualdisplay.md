#开机logo 双屏异显配置

arch/arm64/boot/dts/rockchip/rk3399-android.dtsi

### 两路输出显示不同的内容:

如下, route_hdmi使用logo_hdmi.bmp, route_mipi使用logo_mipi.bmp

    &display_subsystem {
    	route {
    		route_hdmi: route-hdmi {

    			logo,uboot = "logo_hdmi.bmp";
    			logo,kernel = "logo_kernel_hdmi.bmp";
    			connect = <&vopl_out_hdmi>;
    		};
    		route_mipi: route-mipi {
    			logo,uboot = "logo_mipi.bmp";
    			logo,kernel = "logo_kernel_mipi.bmp";
    			logo,mode = "fullscreen";
    			charge_logo,mode = "center";
    			connect = <&vopb_out_mipi>;
    		};
    	};
    };

hdmi和mipi就将显示不同的图片内容.

### 两路输出同时显示:

不同route使用不同的vop即可实现显示的独立.
例如:

    route_hdmi的connect配置成vopl_out_hdmi,
    route_mipi的connect配置成vopb_out_hdmi

hdmi和edp就将使用不同的vop独立显示
