# 有线耳机没声 前后面板插孔无法设置 驱动问题
- date: 2024-12-14

台式机，插了有线耳机，结果进腾讯会议发现听不见别人说话，我说话别人可以听见。于是考虑可能是耳机麦克风好的，耳机喇叭坏的，最后把耳机插到笔记本电脑上，耳机喇叭也是好的，于是想起了很多年前电脑耳机默认输出到背后的耳机孔而不是前面。于是把耳机插背后的孔，这下有声音了，诡异的是找不着那个螃蟹声音设置了，c文件夹里面也没有，更诡异的是我插背后的孔时，不是识别为耳机插入，而是使用扬声器的输出，虽然声音输出到耳机里，而插前面板的孔时会识别到耳机。

期间还尝试连蓝牙耳机，发现声音可以正常通过蓝牙耳机输出，说明这电脑的音频输出至少是正常的。电脑设备前几天升级系统前还在正常使用，更新系统后没怎么听音乐，也就没发现这个问题。

系统更新里选择可选更新，安装了一个看起来像驱动的样子的更新： "HP Inc. Firmware 2.21.0.0"，在其安装过程中才意识到这个更新的是 BIOS。

[Realtek HD Audio Manager – Download](https://realtekhdaudiomanager.com/download/) 下载了 168 MB 的 0009-32bit_Win7_Win8_Win81_Win10_R282.exe（SHA256 为 4DBEB838B2E6650D0B57F0DB6EABE0DD8445A144DC64E3B38E3DF39BE1D41DAD），安装时显示“无法找到可支援的驱动程序”。

最后尝试的方案是驱动精灵，扫描后可升级驱动里面有螃蟹的，选择升级之后耳机插前面板就有声音了，此时控制面板还是没有多出来小螃蟹。

# 参考

[电脑插入耳机检测不到没反应怎么办 细语浅言绕丝轻](https://zhuanlan.zhihu.com/p/620210110)

[Realtek高清晰音频管理器找不到解决方法 2022-07-01 辰奕](https://www.xitongzhijia.net/xtjc/20220609/245502.html)

[Realtek HD Audio Manager – Download](https://realtekhdaudiomanager.com/download/): google drive. 已上传到百度云盘（链接: https://pan.baidu.com/s/1fSSZAnZP2tf-yV1jlBQhQw 提取码: vifx ）
