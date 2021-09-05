---
title: "Pyqt5"
date: 2020-12-03T15:57:37+08:00
lastmod: 2020-12-03T15:57:37+08:00
keywords: []
description: ""
tags: []
categories: [python]
author: "筱氚"
---
# intro
Python3（一下简称python）能用的跨平台GUI库无非tkinter、pyqt5。但是tkinter执行慢颜值不高呀
## 我的环境
deepin 20 amd64  
python 2.7.16  
python 3.7.3
# 安装
## pyqt5
```bash
$ pip3 install PyQt5
.....
FileNotFoundError: [Errno 2] No such file or directory: '/tmp/pip-install-to4q7kth/PyQt5/setup.py'
    
    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-install-to4q7kth/PyQt5/
```
我去啥情况！！！查了一下pip3根本没有upgrade这个指令。。。
```bash
$ sudo apt install python3-setuptools
$ sudo apt install python3-pyqt5
```
## 测试第一个窗体
cv下面的代码，出现一个窗体说明没毛病哈哈哈哈
```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget
from PyQt5.QtGui import QIcon


class App(QWidget):
    def __init__(self):
        super().__init__()
        self.title = 'PyQt5 simple window - pythonspot.com'
        self.left = 10
        self.top = 10
        self.width = 640
        self.height = 480
        self.initUI()

    def initUI(self):
        self.setWindowTitle(self.title)
        self.setGeometry(self.left, self.top, self.width, self.height)
        self.show()


if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = App()
    sys.exit(app.exec_())
```
## 安装图形化编程连接工具-pyqt5-tools
安装pyqt5之后可以在代码里直接使用qt库，但是没有傻瓜拖动布局啊，下面安装能把Qt Designer的ui文件转化为py文件的工具pyqt5-tools
```bash
$ pip3 install pyqt5-tools
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-install-i0ut44da/pyqt5/

$ sudo apt install pyqt5-dev-tools
```
测试一下就是终端敲一下`pyuic5`看有没有这个命令。你说用说明傻瓜编辑UI？当然是Qt Designer了啊，咋安装？？看[Qt-2-安装教程 4分钟教会正确你下载Qt！！非脑残安装全部组件 怎么安装 Qt community？？](https://www.bilibili.com/video/BV117411o7e1)
之后的编辑ui见面就是傻瓜式了，编辑好ui文件用pyuic5将ui文件转成一个python类的py文件，然后编辑一个入口文件（main.py）就可以调用这个ui类啦

# References
- [PyQt5 5.15.2](https://pypi.org/project/PyQt5/)
- [Building PyQt5 with configure.py](https://www.riverbankcomputing.com/static/Docs/PyQt5/building_with_configure.html#installing-prerequisites)
- [PyQt5（designer）入门教程](https://blog.csdn.net/AzureMouse/article/details/90338961)
- [PyQt5 window](https://pythonspot.com/pyqt5-window/)
- [Command “python setup.py egg_info” failed with error code 1 in /tmp/pip-install-xtrlkujj/pyaudio/](https://stackoverflow.com/questions/61515201/command-python-setup-py-egg-info-failed-with-error-code-1-in-tmp-pip-install/65122006#65122006)
- [Learn how you can create a Python GUI in 2020. ](https://build-system.fman.io/pyqt5-tutorial)
- [Qt-2-安装教程 4分钟教会正确你下载Qt！！非脑残安装全部组件 怎么安装 Qt community？？](https://www.bilibili.com/video/BV117411o7e1)
