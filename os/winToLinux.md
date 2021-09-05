---
title: "WinToLinux"
date: 2020-11-14T11:14:08+08:00
lastmod: 2020-11-14T11:14:08+08:00
keywords: []
description: ""
tags: []
categories: [Blog]
author: "筱氚"
---
# Introduction
计划从windows逐渐迁移到free世界 - Linux

# software on win
微信
腾讯会议
云盘
知云文献翻译
CAJViewer
格式工厂
μTorrent
Chrome
Office
7zip
VS Code
Hugo
Git
gcc/g++
Java
Python
Anaconda
Android Studio
Raspberry Pi Image
VirtualBox
Matlab


# Ubuntu
上述无linux版的软件：微信、腾讯会议、知云文献翻译、格式工厂
Office - LibreOffice

系统采用用户友好的Ubuntu。
由于在软件安装商店安装的软件存在功能阉割（VS Code商店下载的就不能输入中文）
## Code, Hugo, gcc, python
在官网下载好deb安装包
```bash
sudo dpkg -i code_1.51.1-1605051630_amd64.deb
sudo dpkh -i hugo_extended_0.78.1_Linux-64bit.deb
sudo apt install git
sudo apt install gcc
sudo apt install python3
```
## CAU Cloud
农大云盘linux安装办法，下载好gz包(参考QQ群文件)
```bash
tar xvzf linux64-1.7.20.gz
cd cloud-1.7.20.linux-amd64
./run.sh
# 报错缺乏gtk+环境
sudo apt install build-essential
sudo apt install libgtk-3-dev
sudo apt install pkg-config
pkg-config --modversion glib-2.0
pkg-config --modversion gtk+-3.0
# 安装完gtk+还是报错 QGtkStyle was unable to detect the current GTK+ theme.

```
## LibreOffice
https://mirrors.tuna.tsinghua.edu.cn/libreoffice/libreoffice/stable/7.0.3/deb/x86_64/
```bash
# 解压主程序、中文语言包、中文帮助文档
tar xvzf LibreOffice_7.0.3_Linux_x86-64_deb.tar.gz
tar xvzf LibreOffice_7.0.3_Linux_x86-64_deb_langpack_zh-CN.tar.gz
tar xvzf LibreOffice_7.0.3_Linux_x86-64_deb_helppack_zh-CN.tar.gz
# 安装主程序、中文语言包、中文帮助文档
sudo dpkg -i ./LibreOffice_7.0.3.1_Linux_x86-64_deb/DEBS/*.deb
sudo dpkg -i ./LibreOffice_7.0.3.1_Linux_x86-64_deb_langpack_zh-CN/DEBS/*.deb
sudo dpkg -i ./LibreOffice_7.0.3.1_Linux_x86-64_deb_langpack_zh-CN/DEBS/*.deb
```
# Deepin
## advantage
## root
```bash
sudo passwd root
```
# References
- [个人电脑用的GNU/Linux完全Free发行版](https://www.gnu.org/distros/free-distros.en.html)
https://www.fsf.org/resources/
https://opensource.builders/
- [Ubuntu18.04 + GTK安装](https://www.jianshu.com/p/60c029928e11)
- [LibreOffice Linux 下的安装方法](https://zh-cn.libreoffice.org/get-help/install-howto/linux/)
[Chrome](https://www.google.cn/chrome/index.html)
[QT之问题1--QGtkStyle was unable to detect the current GTK+ theme](https://blog.csdn.net/djinglan/article/details/7676583)
[Tried Fail - QGtkStyle was unable to detect the current GTK+ theme](https://blog.csdn.net/sfe1012/article/details/16851095)