---
title: "QtOnLinux"
date: 2020-11-24T11:54:39+08:00
lastmod: 2020-11-24T11:54:39+08:00
keywords: ['GUI','C++']
description: "在deepin 20 linux OS 下安装Qt"
tags: ['GUI','C++']
categories: [Qt]
author: "筱氚"
---
# Intro
所使用的OS是基于Debian的Deepin 20 commiunity amd64，自带gcc/g++ 8.3.0。记录安装Qt失败后成功的过程。
# Install
## requirements
gcc和g++其实Linux OS都会自带，以防万一你卸载了还是try吧，装了也不会覆盖安装。
```bash
$ sudo apt update
$ sudo apt install gcc g++
$ sudo apt install build-essential libgl1-mesa-dev
```
## qt
下载qt的安装包，我下载的从清华镜像下的[qt-opensource-linux-x64-5.14.2.run](https://mirrors.tuna.tsinghua.edu.cn/qt/archive/qt/5.14/5.14.2/)

```bash
$ sudo chmod +x qt-opensource-linux-x64-5.14.2.run
$ sudo ./qt-opensource-linux-x64-5.14.2.run
```
安装界面打开后，需要一个Qt帐号（没有就注册），组件默认只选择Qt Creator，我们还需要选择Desktop gcc，其它默认一路Next， 默认安装在 /opt/Qt5.14.2，这个地址保持默认就好了。
## lanch qt
我的OS安装完之后在开始菜单中有Qt Creator的快捷方式，单击即可启动Qt Creator，没有快捷方式的话可以按照下面的方式启动
```bash
$ cd /opt/Qt5.14.2/Tools/QtCreator/bin
$ ./qtcreator
```
# Uninstall
卸载方式，emm第一次装的时候我没有选择Desktop gcc，只能卸载了重新装。
```bash
$ cd /opt/Qt5.14.2
$ sudo ./MaintenanceTool
```
Next - Uninstall only - Next
# References
- [Qt - Ubuntu 搭建/卸载 Qt5.14 开发环境（转）](https://www.cnblogs.com/citrus/p/13279417.html)
- [linux下安装qt](https://www.cnblogs.com/xiangtingshen/p/12096699.html)
- [QT linux安装](https://blog.csdn.net/kunkliu/article/details/78872077)
- [Qt download](http://download.qt.io/archive/qt/)
- [Qt download清华Mirror](https://mirrors.tuna.tsinghua.edu.cn/qt/)
- [Qt for Linux/X11](https://doc.qt.io/qt-5/linux.html)



- []()
- []()
