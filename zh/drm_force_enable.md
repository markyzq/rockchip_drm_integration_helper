
不管hdmi接没接， 强行使能hdmi

    echo on > /sys/devices/platform/display-subsystem/drm/card0/card0-HDMI-A-1/status

关闭hdmi

    echo off > /sys/devices/platform/display-subsystem/drm/card0/card0-HDMI-A-1/status

监测hdmi插拔，正常情况以就是在这个状态

    echo detect > /sys/devices/platform/display-subsystem/drm/card0/card0-HDMI-A-1/status
