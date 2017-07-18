#DRM DEVICE TREE 解析

总的drm device结构如下:

	// rockchip drm core 设备
	display_subsystem: display-subsystem {
		compatible = "rockchip,display-subsystem";
		ports = <&vopl_out>, <&vopb_out>;
	};

	// 显示控制器vop驱动
	vopl: vop@ff8f0000 {
		compatible = "rockchip,rk3399-vop-lit";
		vopl_out: port {
			vopl_out_edp: endpoint@1 {
				reg = <1>;
				remote-endpoint = <&edp_in_vopl>;
			};
		};
	};

	// edp驱动
	edp: edp@ff970000 {
		compatible = "rockchip,rk3399-edp";
		ports {
			edp_in: port@0 {
				edp_in_vopl: endpoint@1 {
					reg = <1>;
					remote-endpoint = <&vopl_out_edp>;
				};
			};
			edp_out: port@1 {
				edp_out_panel: endpoint@0 {
					reg = <0>;
					remote-endpoint = <&panel_in_edp>;
				};
			};
		};
	};

	edp屏驱动
	edp_panel: edp-panel {
		compatible = "lg,lp079qx1-sp0v", "panel-simple";
		//通过短字符串"lg,lp079qx1-sp0v"来匹配写死在kernel源码中的timing

		ports {
			panel_in_edp: endpoint {
				remote-endpoint = <&edp_out_panel>;
			};
		};
	};
