# modetest使用指南

modetest是libdrm源码自带的调试工具, 可以对drm进行一些基础的调试.

获取:

  android平台:

    mmm external/libdrm/tests

  linux平台 - ARM64

    git clone git://anongit.freedesktop.org/mesa/drm && cd drm
    //ARM64
    CC=aarch64-linux-gnu-gcc ./autogen.sh --host=aarch64-linux --disable-freedreno --disable-cairo-tests --enable-install-test-programs
    //ARM32
    CC=arm-linux-gnueabihf-gcc ./autogen.sh --host=arm-linux --disable-freedreno --disable-cairo-tests --enable-install-test-programs
    make -j4 && make install DESTDIR=`pwd`/out

modetest帮助信息:

    (shell)# modetest -h
    usage: modetest [-cDdefMPpsCvw]
     Query options:
    	-c	list connectors
    	-e	list encoders
    	-f	list framebuffers
    	-p	list CRTCs and planes (pipes)
     Test options:
    	-P <crtc_id>:<w>x<h>[+<x>+<y>][*<scale>][@<format>]	set a plane
    	-s <connector_id>[,<connector_id>][@<crtc_id>]:<mode>[-<vrefresh>][@<format>]	set a mode
    	-C	test hw cursor
    	-v	test vsynced page flipping
    	-w <obj_id>:<prop_name>:<value>	set property
     Generic options:
    	-d	drop master after mode set
    	-M module	use the given driver
    	-D device	use the given device
    	Default is to dump all info.

使用案例:

  modetest不带参(有非常多的打印,这边截取部分关键的):

    //由于rockchip driver的一些配置未upstream到libdrm上, 所以从libdrm upstream
    //下载编译的modetest默认不带rockchip支持, 需要在使用的时候加个-M rockchip.
    (shell)# modetest -M rockchip
    Encoders:
    id	crtc	type	possible crtcs	possible clones	
    71	27	TMDS	0x00000001	0x00000000
    73	0	TMDS	0x00000002	0x00000000
    Connectors:
    id	encoder	status		name		size (mm)	modes	encoders
    72	71	connected	HDMI-A-1       	410x260		19	71
      modes:
    	name refresh (Hz) hdisp hss hse htot vdisp vss vse vtot)
      1440x900 60 1440 1520 1672 1904 900 903 909 934 flags: nhsync, pvsync; type: preferred, driver
      1280x1024 75 1280 1296 1440 1688 1024 1025 1028 1066 flags: phsync, pvsync; type: driver
      [...]
    74	0	disconnected	DP-1           	0x0		0	73
    CRTCs:
    id	fb	pos	size
    27	0	(0,0)	(1280x1024)
    64	0	(0,0)	(0x0)
    Planes:
    id	crtc	fb	CRTC x,y	x,y	gamma size	possible crtcs
    23	0	0	0,0		0,0	0       	0x00000001
    [...]

  如上modetest -M rockchip的信息,我们知道当前系统有两个vop, 有两个输出设备:

测试显示输出:
  测试显示输出时要把当前系统其他的显示关掉, 因为drm只能允许一个显示输出程序.

    android: adb shell stop
    linux ubuntu: service lightdm stop

  显示输出命令

    (shell)# modetest -M rockchip -s 72@27:1440x900 -v
    setting mode 1440x900-60Hz@XR24 on connectors 72, crtc 27
    freq: 60.53Hz

    屏幕上即可看到闪烁的彩条显示,
    如需使用dp输出,将命令中的connector的id换成dp的即可.
    如需使用另一个crtc输出, 将命令中的crtc的id换成另一个crtc的id即可
    如需使用别的分辨率输出, 将命令中1440x900换成connectors modes里面别的分辨率即可
