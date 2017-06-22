CONFIG_DRM_LOAD_EDID_FIRMWARE

    allow loading an EDID as firmware to override broken monitor

    Broken monitors and/or broken graphic boards may send erroneous or no
    EDID data. This also applies to broken KVM devices that are unable to
    correctly forward the EDID data of the connected monitor but invent
    their own fantasy data.

    This patch allows to specify an EDID data set to be used instead of
    probing the monitor for it. It contains built-in data sets of frequently
    used screen resolutions. In addition, a particular EDID data set may be
    provided in the /lib/firmware directory and loaded via the firmware
    interface. The name is passed to the kernel as module parameter of the
    drm_kms_helper module either when loaded
      options drm_kms_helper edid_firmware=edid/1280x1024.bin
    or as kernel commandline parameter
      drm_kms_helper.edid_firmware=edid/1280x1024.bin

    It is also possible to restrict the usage of a specified EDID data set
    to a particular connector. This is done by prepending the name of the
    connector to the name of the EDID data set using the syntax
      edid_firmware=[<connector>:]<edid>
    such as, for example,
      edid_firmware=DVI-I-1:edid/1920x1080.bin
      edid_firmware=eDP-1:edid/1280x480.bin,DP-2:edid/1920x1080.bin
    in which case no other connector will be affected.

    The built-in data sets are
    Resolution    Name
    --------------------------------
    1024x768      edid/1024x768.bin
    1280x1024     edid/1280x1024.bin
    1680x1050     edid/1680x1050.bin
    1920x1080     edid/1920x1080.bin

    They are ignored, if a file with the same name is available in the
    /lib/firmware directory.

    The built-in EDID data sets are based on standard timings that may not
    apply to a particular monitor and even crash it. Ideally, EDID data of
    the connected monitor should be used. They may be obtained through the
    drm/cardX/cardX-<connector>/edid entry in the /sys/devices PCI directory
    of a correctly working graphics adapter.

