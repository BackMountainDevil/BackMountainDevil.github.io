# 键盘工作原理与制作
- date: 2022-12-03
- lastmod: 2024-07-19

# 原理

视频：[【科普】机械键盘、薄膜键盘的结构和原理 2023-03-30 做视频的洛唐](https://www.bilibili.com/video/BV13g4y1G7uk/)

键盘的本质是开关，短暂按下会让电路中产生一个冲激信号，串口控制芯片识别信号，告诉系统哪个按键按下了。

薄膜键盘结构上有两层导电薄膜夹一层不导电薄膜，此时的按键胶帽不带导电材料，胶帽的一个作用是往下挤压导电薄膜使其导通，胶帽的另一个作用是回弹。还有一种是锅仔片结构，这种结构的胶帽是导电的，胶帽底部有导电橡胶（一般是黑色的），当然还有用金属锅仔片替代胶帽的，这种结构不需要多层结构，一层即可搞定，在一层板印上不相连的叉子对插电路，胶帽按下时，胶帽底部的导电胶使电路导通，此时电流会经过胶帽，这是和前一种的主要区别。

# USB HID 芯片

[CH9326](https://www.wch.cn/product/CH9326.html):南京沁恒微电子有一堆接口转换芯片

[ USB键鼠类](https://www.wch.cn/products/categories/32.html?pid=1)
> CH9328 串口转HID键盘芯片
> CH9329 串口转HID键盘/鼠标/自定义HID芯片


[HT42B564](https://www.szlcsc.com/info/12597.html)

[ATMEGA32U2-MU]()

# 键盘方案

[Pitaya Go](https://github.com/makerdiary/pitaya-go): nRF52840(BLE, 2.4gHz...) + ATWINC(wifi)

[XS40](https://gitee.com/play565/XS40): CH9328  + N76E003

[ dactyl-keyboard](https://github.com/tshort/dactyl-keyboard):Arduino Pro Micros

# 相关参考

[ijprest/keyboard-layout-editor](http://www.keyboard-layout-editor.com/#/):键盘在线布局

[ 部分开源客制化项目汇总，附一张手上键盘的尺寸对比  slzmxh 2018.5.26](https://www.zfrontier.com/app/flow/2qy607gk5AR2)