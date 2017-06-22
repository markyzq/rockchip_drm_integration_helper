
查看显示时钟:

    (shell)# cat /sys/kernel/debug/clk/clk_summary | grep vop
      dclk_vop0                    2            2   135000000          0 0
      dclk_vop1                    0            1           0          0 0
      aclk_vop0                    2            3   594000000          0 0
      aclk_vop1                    0            2   594000000          0 0
      hclk_vop0                    2            3   198000000          0 0
      hclk_vop1                    0            2   198000000          0 0

需要关注的显示时钟为:

dclk_vop:

  即pixel clock, 像素时钟, 该时钟由具体的显示timing决定, 如果dclk不正确,
  可能导致fps不对或直接不显示. edp, mipi, lvds等显示接口对应dclk的容忍性较好,
  有些偏差也不影响正常显示. 但hdmi, dp等高清显示接口, 是有严格要求的,
  这类显示接口的频率要给的很精准.

aclk_vop:

  如果该时钟频率太低, 可能会导致显示出现抖动, 另外如果aclk
  没有使能的话, 访问vop的寄存器也可能引发总线挂死

hclk_vop:

  如果该时钟未使能, 不能访问vop的寄存器, 一但访问vop寄存器, 会造成总线挂死.
