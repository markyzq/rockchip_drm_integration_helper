#步骤一

参考《屏配置的几个方式》章节，先使用edid的方式，在dts里面写入:

    panel: panel {
    	compatible = "simple-panel";
    	ddc-i2c-bus = <&panelddc>;
    	power-supply = <&vdd_pnl_reg>;
    	enable-gpios = <&gpio 90 0>;
    	backlight = <&backlight>;
    };

如果屏的edid正常， power, gpio, backlight配置正常，应该就能看到显示了。

#步骤二

由于屏的edid是有机率损坏的，做产品是有风险的，所以建议把timing写死到代码里面:

完成步骤一后，可以从uboot串口上获取当前的屏的信息:

    Maximum visible display size: 26 cm x 17 cm
    Power management features: no active off, no suspend, no standby
    Estabilished timings:
    Standard timings:
            2400x1600       59 Hz (detailed)
    Monitor name: LQ123P1JX31
    Detailed mode clock 252750 kHz, flags[a]
        H: 2400 2448 2480 2560
        V: 1600 1603 1613 1646
    bus_format: 1009

    参考《屏配置的几个方式》章节, 屏配置方式一, 将该timing写死到kernel中.

