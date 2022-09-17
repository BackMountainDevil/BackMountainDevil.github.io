# 从 windows 开始拥抱 linux
- date: 2020-11-14
- lastmod: 2022-09-17

# Introduction
计划从windows逐渐迁移到free世界 - Linux

# W L 区别

1. 软件安装

在 windows 上安装某软件的某个版本比linux简单，比如你可以在win10上安装ps的201x～2022的不同版本，但是你要是想在 linux 安装比较老或者比较新的软件版本，需要解决依赖问题，除非该版本把其依赖做了一个打包（如appimage、snap、flatpak）。 在 linux 管理软件比较方便，因为系统会带一个包管理器（如 apt、pacman、dnf），可以快速安装、升级软件。

2. 驱动

比较新的硬件，由于市场原因，软硬件首先会开发、发布windows驱动，以后可能后面会开发linux驱动，也可能得等大佬逆向驱动。比如小新13 pro ARE 2020 的官方驱动都是只提供 windows版。因此新设备安装 linux 可能使得某些模块无法正常工作（如指纹、蓝牙）。

3. [Using Manjaro for Windows users](https://wiki.manjaro.org/index.php/Using_Manjaro_for_Windows_users)
> windows 会自动挂载所有分区，linux 不会。windows每个版本都只有一个图形界面，linux一个版本可以自行选择不同的图形界面，不同图形界面都有之间的文件管理器、系统设置、分区管理器。linux有很多文件系统、内核，但ta没有注册表，通常是采取配置文件来进行配置。linux不会像windows那样覆盖别人的引导。

# 软件替换
## 官方支持 Linux
下面的软件官方有 linux 安装包

- 浏览器：Firefox Chromium Chrom
- 文档处理：WPS
- 游戏：Steam
- 桌面视频录制：OBS Studio
- 编程开发：VS Codium,Miniconda，VirtualBox，Filezilla，DBeaver
- P2P：qBittorrent

不太好归类的：微信(wechat-uos)、腾讯会议(wemeetapp)、爱思助手(i4tools)、Ffmpeg、Handbrake、VLC、Mpv、Kodi

## 替换方案
官方没有 Linux 支持，给出一些可行的替换方案

- 压缩：7zip - Ark

# 有意思的软件

- ScreenKey：在屏幕上显示按了啥按键
- Guvcview:摄像头
- kdenlve、openshot，：视频剪辑
- GNU Radio：无线电
- Filelight:硬盘文件用量分析
- KDE partitionmanager：硬盘分区
- KDE Connect：早期的万物互联

# References
- [个人电脑用的GNU/Linux完全Free发行版](https://www.gnu.org/distros/free-distros.en.html)
https://www.fsf.org/resources/
https://opensource.builders/
- [Ubuntu18.04 + GTK安装](https://www.jianshu.com/p/60c029928e11)
- [LibreOffice Linux 下的安装方法](https://zh-cn.libreoffice.org/get-help/install-howto/linux/)
[Chrome](https://www.google.cn/chrome/index.html)
[QT之问题1--QGtkStyle was unable to detect the current GTK+ theme](https://blog.csdn.net/djinglan/article/details/7676583)
[Tried Fail - QGtkStyle was unable to detect the current GTK+ theme](https://blog.csdn.net/sfe1012/article/details/16851095)