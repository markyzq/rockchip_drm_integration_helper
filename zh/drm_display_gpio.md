
    (shell)# cat /sys/kernel/debug/gpio

    GPIOs 0-31, platform/pinctrl, gpio0:
    GPIOs 32-63, platform/pinctrl, gpio1:
    GPIOs 64-95, platform/pinctrl, gpio2:
     gpio-90  (                    |bt_default_wake     ) out hi
    GPIOs 96-127, platform/pinctrl, gpio3:
     gpio-98  (                    |enable              ) out hi
     gpio-111 (                    |mdio-reset          ) out hi
    GPIOs 128-159, platform/pinctrl, gpio4:

  drm panel驱动默认使用enable做为屏的gpio使能脚, 所以这边重点看一下enable的状态,
  是否与预期的一致.
