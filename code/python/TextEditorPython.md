---
title: "TextEditorPython"
date: 2020-11-20T19:31:05+08:00
lastmod: 2020-11-20T19:31:05+08:00
keywords: ['TextEditor', 'Python', 'tkinter']
description: "用python3写一个简单的文本编辑器"
tags: ['TextEditor']
categories: [Python]
author: "筱氚"
---
# Intro
用python3写一个简单的文本编辑器吧，参考[Create a Simple Python Text Editor!By PumpkinSmasher](https://www.instructables.com/Create-a-Simple-Python-Text-Editor/)，原文太久远，不适合py3，下面对py3进行了一丢更改
## 开发环境
Deepin 20 , python3 3.7.3
## 效果预览
照片好大。。
# 开动啦
## 安装tkinter
My Os: Deepin 20 , python3 3.7.3
这是我开发环境py3安装tkinter和测试的过程，参考[给python安装tkinter模块（及各种问题的解决：如 ModuleNotFoundError: No module named ‘_tkinter’）](https://blog.csdn.net/weixin_39278265/article/details/106651951)
```bash
$ sudo apt update
$ sudo apt install python3-tk
```
测试安装是否成功，出现tkinter窗体说明安装成功
```bash
$ python3 -m tkinter
```

## 显示一个窗口
```python
import tkinter as tk
root = tk.Tk()
root.mainloop()
```
这个窗口有点小，现在还不能输入文字
## 添加文本框
```python
import tkinter as tk
root = tk.Tk()
text = tk.Text(root)
text.grid()
root.mainloop()
```
现在窗口可以编辑文字啦，下面添加保存按钮吧
## 添加按钮

```python
import tkinter as tk

root = tk.Tk()
text = tk.Text(root)
text.grid()
button = tk.Button(root, text="Save")
button.grid()
root.mainloop()
```
这个时候按钮还没有添加功能，下面给它添加保存功能吧。
## 为按钮添加保存功能
```python
import tkinter as tk
from tkinter import filedialog
root = tk.Tk()
text = tk.Text(root)
text.grid()


def savetext():
    global text
    data = text.get("1.0", "end-1c")
    savelocation = filedialog.asksaveasfilename()
    if(savelocation):
        with open(savelocation, 'w+', encoding='utf-8') as f:
            f.write(data)
    else:
        print("empty filepath")


button = tk.Button(root, text="Save", command=savetext)
button.grid()
root.mainloop()
```
# 总结
这个文本编辑器功能还有点简陋，没有字体选择呀、打开文件呀。后面有机会再写，后续代码都放在[Github](https://github.com/BackMountainDevil/tkTextEdit/tree/main)
# FAQ
1. sys.python_version - AttributeError: module 'sys' has no attribute 'python_version'

将第一段代码修改为如下
```python
import sys

pv = sys.version[0]
if pv == 2:
    from Tkinter import *
else:
    from tkinter import *
```
2. from tkinter import * - ModuleNotFoundError: No module named 'tkinter'

没有正确安装tkinter模块，请查看安装tkinter部分

# References
- [Create a Simple Python Text Editor!By PumpkinSmasher](https://www.instructables.com/Create-a-Simple-Python-Text-Editor/)
- [tkinter — Python interface to Tcl/Tk¶](https://docs.python.org/3/library/tkinter.html)
- [给python安装tkinter模块（及各种问题的解决：如 ModuleNotFoundError: No module named ‘_tkinter’）](https://blog.csdn.net/weixin_39278265/article/details/106651951)
- [AttributeError: module 'tkinter' has no attribute 'filedialog'的解决之道](https://blog.csdn.net/happen_if/article/details/83998708)