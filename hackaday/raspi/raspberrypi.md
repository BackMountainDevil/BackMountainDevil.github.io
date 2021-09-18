---
title: "树莓派入门第一步 - 装系统并配置镜像、SSH"
date: 2021-04-27T11:21:15+08:00
lastmod: 2021-09-17T11:21:15+08:00
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
[官方系统下载地址](https://www.raspberrypi.org/software/operating-systems/)，当然推荐[TUNA 镜像的树莓派系统地址（armhf）](https://mirrors.tuna.tsinghua.edu.cn/raspberry-pi-os-images/raspios_armhf/images/)和[UTSC 镜像的树莓派系统地址（armhf）](http://ipv6.mirrors.ustc.edu.cn/raspberry-pi-os-images/raspios_armhf/images/)，打开

lite(0.5 M):Raspberry Pi OS Lite - 纯控制台、无桌面环境
desktop(1.x G):Raspberry Pi OS with desktop - 提供桌面环境
full(2.x G):Raspberry Pi OS with desktop and recommended software - 提供桌面环境和推荐的软件

 > 关于架构问题，2021-09 时有 armhf、arm64,17号尝试了一下 arm64 架构，utsc 没有其镜像，然后是许多包没有该架构的发行分支。折腾了一天默默的换回 armhf 架构。因此暂时不推荐 arm64 架构版本的树莓派 OS，建议采取比较稳的 armhf。因此上面的链接都是指向 armhf 架构的。

这里以中庸的简洁桌面版为例，打开上面的镜像链接，选择日期最新的点进去，下载那个 1.1 G 左右的 *.zip 系统文件和 *.zip.sha1 校验文件。

 > 当然也可以直接从 [TUNA 主页](https://mirrors.tuna.tsinghua.edu.cn/) 右侧的 “下载链接”-“获取下载链接”，在弹窗中左下侧选择 Raspberry Pi OS (原 Raspbian) 就会出现其系统文件，点击 2021-05-07 (armhf, buster) 即可下载中庸版（非 lite、非full），这样比较方便下载，但是不方便文件校验。

```bash
2021-05-07-raspios-buster-armhf.zip
2021-05-07-raspios-buster-armhf.zip.sha1
```

### 文件校验
很多人觉得这一步没有啥必要，我自己也不会对每一个下载进行校验（包管理器帮我校验了）。当然文件校验不是必须的，这里我校验完全是因为生产安全需要。

```bash
sha1sum 2021-03-04-raspios-buster-armhf.zip
```

将校验出来的结果和 `*.zip.sha1` 文件的内容进行对比，如果一样说明镜像文件没有被修改，不一样说明改镜像文件可能被人做了手脚（参考互联网上的各种win）。不一样的话换个地方下载，官方下的总不能校验不通过吧

> KDE 下的文件管理器 Dolphin 自带校验功能，右键 zip 文件-“属性“，新窗口中有校验（Checksums）一栏，把 sha1 文件中的内容复制到校验栏目下的输入框，校验通过会显示 Checksums match，不通过显示 Invalid checksum,不通过说明下载的文件有问题，至于问题有多大不好说。

## 烧录到 SD 卡

安装一个官方的烧录软件 [Raspberry Pi Imager ](https://www.raspberrypi.org/software/) ，启动该软件，选择自定义镜像，选择刚才下载的zip系统文件，之后选择 SD 卡，然后写入镜像，写入完成之后软件会自动检测（Verify）是否正确烧录，喝几杯茶聊会天差不多就好了(20多分钟)。

这个软件本身提供了下载系统文件的功能，奈何它还没有只能到可以切换镜像下载，速度感人，所以需要我们手动下载系统文件。还有一点是最后这个 SD 卡不想做树莓派的硬盘了，就想插手机正常使用，也是需要这个软件进行恢复，选择 SD 卡，操作选择擦除，它就会变正常了。通过格式化不一定能完全恢复，亲测只恢复了几百M,完全恢复还是得靠这些烧录软件。

# 配置 & 镜像
## WiFi

这里配置是为了让其开机后自动连接我们的 wifi（热点），无屏幕用户必做，有屏幕用户可选做。

用文件管理器打开刚才已经烧录好的 sd 卡，默认打开的位置是 /boot ，新建一个名称为 wpa_supplicant.conf 的文本文件，内容如下，修改 wifi名称 和 wifi密码 即可，注意要保留双引号。

```bash
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
 
network={
  ssid="wifi名称"
  psk="wifi密码"
  key_mgmt=WPA-PSK
  priority=1
}
```

## ssh

用文件管理器打开刚才已经烧录好的 sd 卡，默认打开的位置是 /boot ，直接在里面新建一个空白文本文件，名称为 ssh （小写无扩展名）。这样做的目的是让其开机后开启 ssh 服务，这个服务允许我们远程连接树莓派.


## 开机初始化设置
插卡，通电之后会自动开机（想关机就直接断电），默认帐号 pi ,密码为 raspberry。

- 无屏幕用户
开机之后树莓派会根据上面的设置连接热点并开启 ssh，这个时候你需要通过路由器管理界面或者 IP 扫描来获取树莓派的 IP，根据 [使用 arp-scan 快速扫描局域网 IP -> raspberry pi ssh vnc](rasip.md) 的结果这里直接使用其结论：安装 arp-scan，通过 sudo arp-scan -l 进行扫描。扫描结果很明显可以知道树莓派的 IP 是啥，假设是 172.20.10.5

然后通过 ssh pi@172.20.10.5 以 pi 用户进行远程加密连接（注意需要更换ip），输入密码登陆后就进入了树莓派系统（虽然是终端模式）。下一步和有屏幕用户一样。

- 有屏幕用户

  进入桌面之后会提示选择地区（China-中国）、美式键盘（Use US keyboard）、语言、新密码、wifi、是否屏幕有黑边等，注意会提示更新系统，这里建议先不更新，等下面更换了速度更快的国内镜像之后在更新系统。

### vnc

vnc 是用来进行远程桌面连接的，如果你觉得 ssh 就可以了，那这一步可以跳过。以前 pi 需要自己整 vnc 服务端、客户端，现在 pi 直接内置了 vnc viewer 的服务端，我们只需要开启 vnc 服务即可。

在树莓派的终端中敲入 sudo raspi-config 回车即可看到一个菜单，上下选择 Interfacing Options，回车进去之后选择 VNC,回车选择 YES 开启（也可以顺带开启你需要的功能，比如 Camera）。开启完成之后回到主界面，左右选择 Finish 回车。

之后在我们电脑上安装 VNC Viewer，安装完成打开后在其地址栏输入树莓派的 IP，创建连接输入账号密码。

## root 

此步骤非必须，需要 root 权限可以加上 sudo 暂时提权进行替代。
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

提高通过 apt 下载软件、更新系统的速度，默认设置的服务器在国外，下载速度哭天喊娘。参见[TUNA Raspbian 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)、[UTSC Raspbian 源使用帮助(暂时没有 Arm64 架构的包镜像)](http://mirrors.ustc.edu.cn/help/raspbian.html)

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

pip（pip3） 镜像需要更换的可以参考[更换 pip 镜像](../../code/python/pip.md)得知

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

装了 2020-08-20-raspios-buster-armhf.zip 、 2021-03-04-raspios-buster-armhf.zip 、2021-03-04-raspios-buster-armhf.zip 之后发现这两装的都是 python 2.7 和 3.7。装别的版本要编译安装（时间感人）

# FAQ
1. ssh 连接 报错 `Host key verification failed`

如果之前树莓派在我们的热点下分配过 IP 并且已经使用过 ssh 登陆，那么上面会报错 `Host key verification failed`，删除在 /home/$user/.ssh/known_hosts 中有关于树莓派 IP 的几行再 ssh 就可以了。

2. VNC viewer 连接显示黑屏，提示 cannot currently show the desktop

ssh 能正常使用，vnc viewer 无法显示的情况下，根据[树莓派入门操作及VNC显示 cannot currently show the desktop 解决方法.Jacen_L 2020-06-05](https://blog.csdn.net/LlHilo/article/details/106577069)和亲身测试表明，进入 ssh 用 sudo raspi-config 设置一下分辨率(Display Options - Resolution)，我将其设置为 640x480 重启后 vnc 就可以了。 

# 相关参考

- [零基础学树莓派Raspberry Pi - 基于Pi 2 Imager -从清华镜走起到设置镜像源后远程连接桌面。Kearney form An idea 2020-10-22](https://blog.csdn.net/weixin_43031092/article/details/109136614)：一年前写的 v0，有 LED 状态解读，使用 windows 自带的远程桌面进行连接

- [Raspbian Repository Mirrors](http://www.raspbian.org/RaspbianMirrors)：从这里可以查阅世界上所有的树莓派软件仓库镜像站点
