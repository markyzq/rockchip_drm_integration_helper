## Component Framework

# 框架代码:

    drivers/base/component.c

# 功能:

  component framework作者Russell King对该机制的描述:

    Subsystems such as ALSA, DRM and others require a single card-level
    device structure to represent a subsystem.  However, firmware tends to
    describe the individual devices and the connections between them.

    Therefore, we need a way to gather up the individual component devices
    together, and indicate when we have all the component devices.

    We do this in DT by providing a "superdevice" node which specifies
    the components, eg:

        imx-drm {
                compatible = "fsl,drm";
                crtcs = <&ipu1>;
                connectors = <&hdmi>;
        };

    The superdevice is declared into the component support, along with the
    subcomponents.  The superdevice receives callbacks to locate the
    subcomponents, and identify when all components are present.  At this
    point, we bind the superdevice, which causes the appropriate subsystem
    to be initialised in the conventional way.

    When any of the components or superdevice are removed from the system,
    we unbind the superdevice, thereby taking the subsystem down.

  在dts中, 有一个总的设备节点, 所有的子设备信息都通过dts描述关联起来, 这样系统
  开机后, 就能统一的管理各个设备.
