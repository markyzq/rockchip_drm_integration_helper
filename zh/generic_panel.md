# 屏配置

通用屏驱动:

    drivers/gpu/drm/panel/panel-simple.c

device-tree:

    Required properties:
      power-supply: regulator to provide the supply voltage

    Optional properties:
    - compatible: value maybe one of the following
                 "simple-panel";
                 "simple-panel-dsi";

    - ddc-i2c-bus: phandle of an I2C controller used for DDC EDID probing
    - enable-gpios: GPIO pin to enable or disable the panel
    - backlight: phandle of the backlight device attached to the panel

    Required properties when compatible is "simple-panel" or "simple-panel-dsi":
    - display-timings: see display-timing.txt for information

    Optional properties:
    - delay,prepare: the time (in milliseconds) that it takes for the panel to
                     become ready and start receiving video data
    - delay,enable: the time (in milliseconds) that it takes for the panel to
                    display the first valid frame after starting to receive
                    video data
    - delay,disable: the time (in milliseconds) that it takes for the panel to
                     turn the display off (no content is visible)
    - delay,unprepare: the time (in milliseconds) that it takes for the panel
                       to power itself down complete
    - bus-format:


    Optional properties when compatible is a dsi devices:
    - dsi,flags: dsi operation mode related flags
    - dsi,format: pixel format for video mode
    - dsi,lanes: number of active data lanes


#屏配置方式一: 使用短字符串匹配写死的timing

1, 把timings写在drivers/gpu/drm/panel/panel-simple.c中, 直接以短字符匹配,
该方式为upstream推荐的使用方式.

DeviceTree:

    panel: panel {
    	compatible = "cptt,claa101wb01";
    	ddc-i2c-bus = <&panelddc>;
    	power-supply = <&vdd_pnl_reg>;
    	enable-gpios = <&gpio 90 0>;
    	backlight = <&backlight>;
    	delay,prepare = <10>;
    	delay,enable = <10>;
    	delay,disable = <10>;
    	delay,unprepare = <10>;
    };

drivers/gpu/drm/panel/panel-simple.c:

    static const struct drm_display_mode lg_lp097qx1_spa1_mode = {
    	.clock = 205210,
    	.hdisplay = 2048,
    	.hsync_start = 2048 + 150,
    	.hsync_end = 2048 + 150 + 5,
    	.htotal = 2048 + 150 + 5 + 5,
    	.vdisplay = 1536,
    	.vsync_start = 1536 + 3,
    	.vsync_end = 1536 + 3 + 1,
    	.vtotal = 1536 + 3 + 1 + 9,
    	.vrefresh = 60,
    };
    static const struct panel_desc lg_lp097qx1_spa1 = {
    	.modes = &lg_lp097qx1_spa1_mode,
    	.num_modes = 1,
    	.size = {
    		.width = 320,
    		.height = 187,
    	},
    };
    static const struct of_device_id platform_of_match[] = {
    	[...]
    	}, {
    	  .compatible = "lg,lp097qx1-spa1",
    	  .data = &lg_lp097qx1_spa1,
    	}, {
    	[...]
    }

#屏配置方式二: 直接将timing写在dts文件中

2, 把使用display-timings结构, 直接把timing写在dts文件中, 这类适用于简单的屏配置

    	panel: panel {
    		compatible = "simple-panel-dsi";
    		ddc-i2c-bus = <&panelddc>;
    		power-supply = <&vdd_pnl_reg>;
    		enable-gpios = <&gpio 90 0>;
    		backlight = <&backlight>;
    		dsi,flags = <MIPI_DSI_MODE_VIDEO |
    			     MIPI_DSI_MODE_VIDEO_BURST |
    			     MIPI_DSI_MODE_VIDEO_SYNC_PULSE>;
    		dsi,format = <MIPI_DSI_FMT_RGB888>;
    		dsi,lanes = <4>;
    		delay,prepare = <10>;
    		delay,enable = <10>;
    		delay,disable = <10>;
    		delay,unprepare = <10>;

    		display-timings {
    			native-mode = <&timing0>;
    			timing0: timing0 {
    				clock-frequency = <160000000>;
    				hactive = <1200>;
    				vactive = <1920>;
    				hback-porch = <21>;
    				hfront-porch = <120>;
    				vback-porch = <18>;
    				vfront-porch = <21>;
    				hsync-len = <20>;
    				vsync-len = <3>;
    				hsync-active = <0>;
    				vsync-active = <0>;
    				de-active = <0>;
    				pixelclk-active = <0>;
    			};
    		};
    	};

#屏配置方式三: 使用edid

不填写任何timing, 直接使用edid来获取timing

    panel: panel {
    	compatible = "simple-panel-dsi";
    	//compatible = "simple-panel";
    	ddc-i2c-bus = <&panelddc>;
    	power-supply = <&vdd_pnl_reg>;
    	enable-gpios = <&gpio 90 0>;
    	backlight = <&backlight>;
    	delay,prepare = <10>;
    	delay,enable = <10>;
    	delay,disable = <10>;
    	delay,unprepare = <10>;
    };

Document:

    Documentation/devicetree/bindings/display/panel/simple-panel.txt
