---
title: "树莓派入门第一步 - 装系统并配置镜像、SSH"
date: 2021-04-27T11:21:15+08:00
lastmod: 2021-04-27T11:21:15+08:00
keywords: ['linux', 'raspberry']
description: "树莓派入门第一步 - 装系统并配置镜像、SSH，第二版"
tags: ['raspberry']
categories: [OS]
author: "筱氚"
---
# 简介

树莓派入门第一步 - 装系统并配置镜像、SSH

# 装系统

## 下系统镜像

官方镜像地址：[Raspberry Pi OS Images](https://www.raspberrypi.org/software/operating-systems/)

推荐下一个 32 位桌面版（大概 1.1 G），以后无聊了在入手 Lite 控制台版本吧，当然这里给出[清华镜像的树梅派镜像地址](https://mirrors.tuna.tsinghua.edu.cn/raspberry-pi-os-images/raspios_armhf/images/)，挑一个日期最新的点进去，下载那个 1.1 G 左右的 *.zip 文件和 *.zip.sha1 校验文件。

我下载之后的文件是
```bash
2021-03-04-raspios-buster-armhf.zip
2021-03-04-raspios-buster-armhf.zip.sha1
```

### 文件校验

```bash
sha1sum 2021-03-04-raspios-buster-armhf.zip
```

将校验出来的结果和 `*.zip.sha1` 文件的内容进行对比，如果一样说明镜像文件没有被修改，不一样说明改镜像文件可能被人做了手脚（参考互联网上的各种win）。不一样的话换个地方下载，官方下的总不能校验不通过吧

## 写入 SD 卡

下一个 [Raspberry Pi Imager ](https://www.raspberrypi.org/software/) 并安装，windows、ubuntu、macOS版本的都有，arch 安装 rpi-imager 参考 [用 debtap 将 deb 转换成 arch 的软件包格式，以 Raspberry Pi Imager 为例](https://backmountaindevil.github.io/post/debtap/)

启动该软件，选择自定义镜像，选择刚才下载并通过校验的镜像，之后选择 SD 卡，然后写入镜像，写入完成之后软件会自动检测（Verify）是否正确烧录，此步骤共耗时大概 20 mins。

# 配置 & 镜像

## 关于 config.txt

在[零基础学树莓派Raspberry Pi - 基于Pi 2 Imager -从清华镜走起到设置镜像源后远程连接桌面](https://blog.csdn.net/weixin_43031092/article/details/109136614?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161943859216780264042429%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161943859216780264042429&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-109136614.pc_v2_rank_blog_default&utm_term=%E6%A0%91%E6%A2%85%E6%B4%BE#t6)一文中，我当时时尝试了不少办法无法启动，请教学姐改了 config.txt 才能正常启动，然而最近几次反复装系统的结论告诉我 - 不需要自行配置  config.txt，你已经是一个成熟的系统了。

## 开机

插卡，通电之后会自动开机，默认帐号 pi ,密码为 raspberry。

进入桌面之后会提示选择地区、语言、新密码、wifi、是否屏幕有黑边等，设置完重启

## root

下面这个命令的作用是重置 root 密码，默认 root 账户无密码被禁用，需要重置它并启用

`sudo passwd root`

没有初始化 root 密码是无法通过偶 root 鉴定的，即使输入空密码也会鉴定失败。因此需要重置其密码，重置之后会自动解禁 root 账户。

| 命令 | 说明 |
|:---|:---|
| sudo passwd root | 重置 root 密码|
| sudo passwd pi | 重置 pi 密码|
| sudo passwd --unlock root | 解禁 root 账户 |
| sudo passwd --lock root | 锁定 root 账户 |
| su root | 切换到 root 账户，推荐使用 sudo |
| su pi | 切回pi用户 |
| sudo command | 以管理员权限执行 command 命令|


## apt镜像

```bash
sudo nano /etc/apt/sources.list
# 用 # 注释原文本的内容，把下面两行粘贴进去，保存退出
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib rpi

sudo nano /etc/apt/sources.list.d/raspi.list
# 用 # 注释原文本的内容，把下面一行粘贴进去，保存退出
deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui

# 更新软件源列表
sudo apt update
```

## pip 镜像

pip 与 pip3 都有的，将下面的 pip 换为 pip3 在 cv 一遍即可

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
pip config set install.trusted-host https://pypi.tuna.tsinghua.edu.cn
```

## ssh

ssh 是为了远程登陆 raspberry pi 使的，再也不需要屏幕了

```bash
# 开启树莓派的 ssh 服务
sudo /etc/init.d/ssh start
# 查看树莓派 ip
# 大概在 wlan0 的第二行，类似 192.168.xxx.xxx
ifconfig
```

例如我的电脑和树莓派都连接了同一个局域网（WiFi、手机热点等），然后就可以在电脑上远程连接树莓派啦


```bash
# pi 为用户名，类似的可以用 root 用户登陆，不推荐
# ip 就是上面看出来 xxx.xxx.xxx.xxx
ssh pi@ip
```

# 总结

多整几次，前几次会耗时以小时为单位，后面就是半小时了。

装了 `2020-08-20-raspios-buster-armhf.zip` 和 `2021-03-04-raspios-buster-armhf.zip` 之后发现这两装的都是 python 2.7 和 3.7。装别的版本要编译安装（时间感人）

# 参考

[零基础学树莓派Raspberry Pi - 基于Pi 2 Imager -从清华镜走起到设置镜像源后远程连接桌面](https://blog.csdn.net/weixin_43031092/article/details/109136614?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161943859216780264042429%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161943859216780264042429&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-109136614.pc_v2_rank_blog_default&utm_term=%E6%A0%91%E6%A2%85%E6%B4%BE#t6)：好些日子前写的第一版

[Raspberry Pi OS Images](https://www.raspberrypi.org/software/operating-systems/)

[清华镜像的树梅派镜像地址](https://mirrors.tuna.tsinghua.edu.cn/raspberry-pi-os-images/raspios_armhf/images/)

[Raspberry Pi Imager ](https://www.raspberrypi.org/software/) 

[用 debtap 将 deb 转换成 arch 的软件包格式，以 Raspberry Pi Imager 为例](https://backmountaindevil.github.io/post/debtap/)

[Python 永修修改pip设置国内默认镜像源，提高下载速度](https://blog.csdn.net/weixin_43031092/article/details/108690238?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161949784416780265479627%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161949784416780265479627&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-108690238.pc_v2_rank_blog_default&utm_term=python+pip)

[Raspbian Repository Mirrors](http://www.raspbian.org/RaspbianMirrors)

[Raspbian 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)
