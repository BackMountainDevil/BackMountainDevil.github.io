# wine 基础知识：让 windows 软件在 linux 上跑起来
- date: 2022-04-16
- lastmod: 2022-04-16

# 安装

参照 [Arch Wiki](https://wiki.archlinux.org/title/Wine) 安装 wine, wine-gecko, wine-mono.

# 使用
## 运行安装在 windows 上的软件

这个比较简单，找到软件在 windows 安装路径中的启动文件，然后用 wine 运行即可，双击运行或者使用命令行运行（如 `wine /run/media/kearney/a/winApp/PotPlayer/PotPlayerMini.exe`）。此方法我尝试过 Potplayer、Heidisql、微信，都是可以勉强使用；也有的软件在此办法下根本无法运行，比如腾讯会议。

## 将 windows 平台上的软件安装到 linux 并运行

这个也很简单，下载某软件的 windows 安装包（如 BaiduNetdisk_7.14.2.9.exe），使用 wine 运行该安装包就会把软件安装到 linux 系统里，还会添加到菜单栏中。BaiduNetdisk 官方其实有 linux 版本，但是功能上一般是落后于 win 版本的（微信、qq 同理），于是各路神仙就会想办法让 win 版软件在 linux 上跑起来。

# FAQ
## wine 中文显示为框框

wine 运行安装在 windows 系统的软件的时候，输入的中文或界面上本该是中文的地方，显示为方框。比如说运行 winecfg 选中第三个标签页 “显示”，在下方调节 “屏幕分辨率” 的滑动条下方显示的文字是“这是使用 10 号 Tahoma 字体的示例文本“，但经常出现没有配置字体显示为方框。

解决办法：把中文字体复制到 wine 的配置目录下（如把 fake_simsun.ttc 复制到 ～/.wine/drive_c/windows/Fonts/）

# 参考

- [[Share Experiences] 解决QQ(wine)因字体卡死&宋体发虚太难看的一种方法](https://bbs.deepin.org/en/post/213530)：自制 fake_simsun.ttc
