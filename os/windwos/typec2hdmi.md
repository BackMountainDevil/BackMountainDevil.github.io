# USB-C 2 HDMI 系统对比
- date: 2024-01-12

[Lenovo USB-C 3-in-1 Hub](https://support.lenovo.com/us/en/accessories/acc500080-lenovo-usb-c-3-in-1-hub-overview-and-service-parts)，有三种型号：日本、除中日之外、中国专卖，[拆解显示](https://post.smzdm.com/p/apvpzgkw/) 转换芯片是微驱科技EP9632C、usb芯片是威锋VL211-Q4

小新13p 外接 typeC 转hdmi，接显示器打游戏没问题，接会议室投影就黑屏（其它人的电脑直插投影是正常的），会议室的hdmi线很长，由好几根连在一起的，怀疑信号衰减，然后在两根HDMI线连接处接上，有画面显示了，但是会疯狂雪花抖动，这个时候我就怀疑是转接器的问题了，如果其输出的信号强度差，输出大概会花屏，为了排除系统的原因，我从linux切换到win11,好家伙，画面可以了，不花屏，在HDMI延长线末端接也不花屏，说明之前花屏、黑屏的原因大概率是系统问题，当然更多推测是驱动。当然如果仅是日常追剧、打游戏使用的话，这个 bug 影响不大。
