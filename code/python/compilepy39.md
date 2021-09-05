---
title: "在树莓派2上非全局编译安装 python39"
date: 2021-04-27T22:26:58+08:00
lastmod: 2021-04-27T22:26:58+08:00
keywords: ['compile', 'raspberry', 'python', 'linux']
description: "在树莓派2上非全局编译安装 python39"
tags: ['compile', 'raspberry', 'python']
categories: [OS]
author: "筱氚"
---
# 简介

目地： 自定义编译安装 python39 到当前用户目录下
硬件： 树莓派2
软件： 2021-03-04-raspios-buster-armhf
时长： 将近一个半小时，一个多小时在编译

# 安装依赖

看了不少帖子，每个帖子说的依赖都不尽相同，如下所示（ apt-get 已被我更换为 apt），找了官方文档也没有找到 Python 编译安装的官方文档，因此这一步我啥依赖也没有安装

```bash
sudo apt  update
sudo apt install build-essential libsqlite3-dev sqlite3 bzip2 libbz2-dev
sudo apt install build-essential libncurses-dev libreadline-dev libsqlite3-dev libssl-dev libexpat1-dev zlib1g-dev libffi-dev
sudo apt build-dep python
sudo apt install libffi-dev libgdbm-dev libsqlite3-dev libssl-dev zlib1g-dev
```

# 源码

不同的格式对于不同的解压代码，二选一即可。

我选择的是先从[python ftp](https://www.python.org/ftp/python)下载好 Python-3.9.2.tar.xz，然后拷贝到 SD 卡里，比 wget 快多了

```bash
# 下载源代码压缩包
wget https://www.python.org/ftp/python/3.9.2/Python-3.9.2.tar.xz
# 解压
tar xvJf Python-3.9.2.tar.xz

# 上面下好了就不要在下了，不同压缩格式罢了。
wget https://www.python.org/ftp/python/3.9.2/Python-3.9.2.tgz 
tar zxvf Python-3.9.2.tgz
```

# 编译安装

由于想保留原来的两个 py（默认 27 和 37 ），只是尝试在自己目录下编译安装而已，因此用 `--prefix` 指定了安装目录

```bash
# 进入解压目录
cd Python-3.9.2

./configure --prefix=/home/pi/opt/python3.9  --enable-optimizations

# 4线程编译，可修改为cpu核心 * 2，树莓派2代 make 了 72 mins
make -j4
```

## 插曲

编译了一个多小时，人都傻了，编译 linux 内核都比这快，刷刷的不断流出中间过程，就不能只显示warning或者error吗？后面看到了一个致命错误说编译终止，但是ta还是继续编译下去了。。。这？？

```bash
******************
/home/pi/Python-3.9.2/Modules/_ctypes/_ctypes.c:107:10: fatal error: ffi.h: 没有那个文件或目录
 #include <ffi.h>
          ^~~~~~~
compilation terminated.
******************

Python build finished successfully!
The necessary bits to build these optional modules were not found:
_bz2                  _curses               _curses_panel      
_dbm                  _gdbm                 _hashlib           
_lzma                 _sqlite3              _ssl               
_tkinter              _uuid                 readline           
To find the necessary bits, look in setup.py in detect_modules() for the module's name.


The following modules found by detect_modules() in setup.py, have been
built by the Makefile instead, as configured by the Setup files:
_abc                  atexit                pwd                
time                                                           


Failed to build these modules:
_ctypes                                                        


Could not build the ssl module!
Python requires an OpenSSL 1.0.2 or 1.1 compatible libssl with X509_VERIFY_PARAM_set1_host().
LibreSSL 2.6.4 and earlier do not provide the necessary APIs, https://github.com/libressl-portable/portable/issues/381

running build_scripts
copying and adjusting /home/pi/Python-3.9.2/Tools/scripts/pydoc3 -> build/scripts-3.9
copying and adjusting /home/pi/Python-3.9.2/Tools/scripts/idle3 -> build/scripts-3.9
copying and adjusting /home/pi/Python-3.9.2/Tools/scripts/2to3 -> build/scripts-3.9
changing mode of build/scripts-3.9/pydoc3 from 644 to 755
changing mode of build/scripts-3.9/idle3 from 644 to 755
changing mode of build/scripts-3.9/2to3 from 644 to 755
renaming build/scripts-3.9/pydoc3 to build/scripts-3.9/pydoc3.9
renaming build/scripts-3.9/idle3 to build/scripts-3.9/idle3.9
renaming build/scripts-3.9/2to3 to build/scripts-3.9/2to3-3.9
make[1]: 离开目录“/home/pi/Python-3.9.2”
```

提示缺少OpenSSL，那就安装一个呗 - `sudo apt install openssl`

```bash
# 继续安装
make altinstall
```

## PATH

```bash
nano ~/.bashrc
export PATH="/home/pi/opt/python3.9/bin"
source ~/.bashrc
```

# 问题

py39确实装上了。但是原本的 pip 和 python 都失效了？nano 和 ls都没有了？？

```bash
$ python3.9 -V
Python 3.9.2
$ python3 -V
-bash: python3：未找到命令
$ python -V
-bash: python：未找到命令
$ pip3 -V
-bash: pip3：未找到命令
$ pip -V
-bash: pip：未找到命令

$ nano ~/.bashrc
-bash: nano：未找到命令
$ pwd
/home/pi
$ ls
-bash: ls：未找到命令
```

删掉刚才加进去的 path 之后，一切回复正常了。

# 总结

虽然加入 PATH 中使得系统变得不正常了，去掉即可，毕竟安装这个 py39 只是为了一个 flask 项目而已，在这个项目中指定 py39 的路径即可。

领略了 CPU 速率的重要性，这编译速度人都傻了。

# 参考

[python ftp](https://www.python.org/ftp/python)

[树莓派 Linux 编译安装 Python 3.8 最新版 ZhouZHou 2019-10-17 ](https://zwc365.com/2019/10/17/%E6%A0%91%E8%8E%93%E6%B4%BE-Linux-%E7%BC%96%E8%AF%91%E5%AE%89%E8%A3%85-Python-3-8-%E6%9C%80%E6%96%B0%E7%89%88/)：主要参考此文；path不对劲，3B+ 编译耗时 15 mins

[Linux 管理环境变量PATH & Demo Kearney form An idea 2021-02-07](https://blog.csdn.net/weixin_43031092/article/details/113740660)
