---
title: "Xiaoxin13ProWinUbuOS"
date: 2020-11-11T16:48:33+08:00
lastmod: 2020-11-11T16:48:33+08:00
keywords: ['Linux', 'Ubuntu', 'OS']
description: "小新13pro锐龙版安装win+Ubuntu双系统。"
tags: ['Linux', 'Ubuntu']
categories: [OS]
author: "筱氚"
---
# Introduction
小新13pro锐龙版默认安装的是Windows10，作为资深搞机佬的弟弟，一直在谋划着装一个Linux系统在小新上面，常见的Linux有Centos、Ubuntu、Debian等，在GNU官网还看到了完全自由的linux操作系统一览表，其中并不包含我们常见的三巨头。

# How to do
下面以小新13pro锐龙版安装win+Ubuntu双系统为例。
## 下载软件
- Ubuntu 20.04 LTS的iso镜像文件（推荐清华镜像下载）
- [Ventoy](https://www.ventoy.net/cn/index.html)：启动U盘的开源工具
## 压缩磁盘
在win系统里进入磁盘管理，我把D盘压缩了130个G的空间出来，压出来的空间区域没有划分盘符和格式，默认为黑色的。
## 载入镜像
1. win下面的Ventory解压即可使用，插上U盘（自觉备份U盘数据哇），在Ventory中选择对应的U盘，安装即可。
2. 然后把下载好的iso镜像拷贝到U盘即可。
## 暂时修改BIOS
关机状态下，用插针插入小新右侧小孔，进入BIOS设置，把Security Boot设置为Disable，保存退出。装完系统后再改回Enable。
> 之所以这样做是因为，我在Boot Menua里选择U盘启动后还是进入了win，尝试好几次都是这样，把Security Boot暂时关掉就好了。无法识别U盘的我没遇到。

## 安装Ubuntu
关机状态下，用插针插入小新右侧小孔，在Boot Menu中选择U盘，然后就会进入安装界面，Ubuntu安装还是很友好的，选择安装语言时拉到最底下选简体中文即可，其它保持默认（会保留windwos，启动时可以选择进入哪一个操作系统）。不得不说安装速度快的一匹，比我的台式机SSD装的还要快。。。这是为什么呢？

安装完之后重启前要拔掉U盘，然后在开机界面就会有选择Ubuntu还是win的界面。
## 还原BIOS
关机状态下，用插针插入小新右侧小孔，进入BIOS设置，把Security Boot设置为Enable，保存退出。
# Conclusion
1. Ventoy真不错，再也不用一种系统格式化一次了。
## ubuntu bug
1. 小新pro 2.5K 的分辨率真的是鸡肋。。。支持2.5K的视频不多，大多数都是1080P，软件等也很好支持1080P，有的没有做缩放，2.5K下看上去及其不协调。。。比如装完双系统的选择界面。。。字好小。Ubuntu分辨率只能缩放100/200/300，没有140.。。
2. 屏幕亮度无法调节。
3. 粘贴文件没有进度条，咋知道移动完没有。
4. 软件商店大部分时候用不了，下载下来的还可能是功能阉割版本。
# References
- [Ventoy](https://www.ventoy.net/cn/index.html)
- [联想小新13pro安装ubuntu双系统心得（解决无法识别启动U盘等问题）](https://blog.csdn.net/weixin_44950479/article/details/108502983)