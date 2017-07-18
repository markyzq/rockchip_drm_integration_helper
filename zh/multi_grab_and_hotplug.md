#多屏抢占及热拔插

    &display_subsystem {
      route {
          route_hdmi: route-hdmi {
              connect = <&vopb_out_hdmi>;
          };

          route_mipi: route-mipi {
              connect = <&vopb_out_mipi>;
          };

          route_edp: route-edp {
              connect = <&vopl_out_edp>;
          };
      };
    };

###抢占

route的节点是有顺序优先关系的, 如上, route_hdmi在route_mipi之前, 且它们都使用vopb做为显示输出,
当hdmi和mipi同接入时, hdmi会先将vopb抢走, 这样mipi就分配不到vop了, 现象为: hdmi显示, mipi不显示

###热拔插

如上抢占内容可知, 当hdmi插入时, 现象为hdmi显示, mipi不显示.

但当hdmi处于拔出状态时, route_hdmi这一路将不会工作, 也即可以实现: hdmi不显示, mipi显示.

由此实现同一配置, 插入hdmi和拔出hdmi启动过程通路状态不同.
