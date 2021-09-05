---
title: "使用 arp-scan 快速扫描局域网 IP -> raspberry pi ssh vnc"
date: 2021-07-16T23:23:32+08:00
lastmod: 2021-07-16T23:23:32+08:00
keywords: ['linux', 'raspberry','arp-scan']
description: "树莓派无屏幕连接的一种方法，使用 arp-scan 快速扫描局域网 IP"
tags: ['raspberry']
categories: ['os']
author: "筱氚"
---
# 简介
在使用 ssh/vnc 对树莓派进行远程操作的时候，都需要获取树莓派的 IP。常见的办法是路由器管理页面或者使用 Advanced IP Scanner。但是我既没有路由器，也没搞懂这个软件怎么用。

## 树莓派无屏连接步骤

1. 开始 ssh
2. 写入 wifi 账号密码
3. 获取树莓派 IP

其中第一步、第二步比较简单，关键在于第三步，宿舍装路由器的毕竟是少数，校园网 WiFi 也不支持局域网连接（打局域网游戏就会发现），一般我是手机开个热点，电脑和树莓派都处在热点网络里，然后给树莓派插上屏幕获取 ip，之后就可以使用 ssh 直接远程操作了，说好的没有屏幕呢？上面说的是正常情况下，没有屏幕的时候，就得借助一些软件进行局域网 IP 扫描了。

有点手机热点会显示有几个用户，有的还会显示 MAC，那这样说不就是也可以显示 IP 吗？但是我还没有发现它有。。。换一个角度思考，现在的手机热点不就是相当于路由器 wifi吗？有没有类似的路由器管理页面呢？我也还没有发现。

# 局域网 IP 扫描软件
找了不少的文章和软件，大体上都是支持网段扫描，关键在于我咋知道我要扫描那一个网段呢？然后查了一下局域网的 IP 范围，下面是一个常见的结果，也就是说我得逐个局域网网段扫描，但是我本着一切从简的原则，找到了一个一行命令就可以解决的办法。

- [局域网IP段.2016-09-17 21:26  mvpbang](https://www.cnblogs.com/xiaochina/p/5823451.html)
  > C类：192.168.0.0-192.168.255.255  
    B类：172.16.0.0-172.31.255.255    #小型的局域网  
    A类：10.0.0.0-10.255.255.255         #一般大型局域网用的

下面的三个软件（同源归为一个）中，arp-scan 最简单好用，也不用输入网段，就可以得到局域网内的主机对应的 IP，而 nmap, IP Scanner都需要填网段，因此推荐使用 arp-scan。安装方法因操作系统的包管理方式有所区别，这里以 Arch Linux 为例。

## arp-scan
这是我从中找到的最简单、最方便的一个，安装运行就完事了。
```bash
# 安装 arp-scan
sudo pacman -S arp-scan
# 扫描本地局域网的 IP+MAC+
sudo arp-scan -l
# 网段扫描
sudo arp-scan 192.168.1.0/24     #Scans 192.168.1.0 255.255.255.0
sudo arp-scan 192.168.1.1-192.168.1.254     #Scans the obvious range
```
这是我电脑和树莓派连接手机热点时的情况（其中 MAC 地址已打码），可以明显知道树莓派的 IP 是 `172.20.10.2` 
```bash
$ sudo arp-scan -l
Interface: wlp1s0, type: EN10MB, MAC: 2-------------b, IPv4: 172.20.10.4
Starting arp-scan 1.9.7 with 16 hosts (https://github.com/royhills/arp-scan)
172.20.10.1     2-------------4       (Unknown: locally administered)
172.20.10.2     d-------------1       Raspberry Pi Trading Ltd
172.20.10.2     d-------------1       Raspberry Pi Trading Ltd (DUP: 2)
```

## namp
这个可以对指定的网段进行扫描，但问题的关键在于我怎么知道是哪个网段呢？一个个试一试。。
```bash
# 安装 nmap，无图形界面
sudo pacman -S nmap
# 或者安装 zenmap（带图形化界面的 namp）
yay -S zenmap
sudo nmap -sP 192.168.1.0/24     #Scans 192.168.1.0 255.255.255.0
sudo nmap -sP 192.168.1.1-254     #Scans the obvious range
```

## ipscan
这个是 Angry IP Scanner，和 Advanced IP Scanner 差不多的东西，带图形界面，还是那个老问题。
```bash
# 安装 ipscan，从 github 下，需要网速靠谱
yay -S ipscan
```

# 参考
- [Namp Arch Wiki](https://wiki.archlinux.org/title/Nmap):yay 安装 zenmap 失败：不到所有需要的包:libglade, python2-gobject2
  > Nmap has a GUI called zenmap
- [arp-scan Arch pkg](https://archlinux.org/packages/community/x86_64/arp-scan/)
- [ipscan. Arch AUR](https://aur.archlinux.org/packages/ipscan/):Angry IP Scanner (or simply ipscan)
- [Top 3 IP Scanners for Linux.  ingram on Mon, 12/12/2011](http://www.itswapshop.com/articles/top-3-ip-scanners-linux):arp-scan, nmap, Angry IP Scanner 
- [如何在 Linux 中查看 IP 地址. 2019-8-12. Linux公社](https://m.linuxidc.com/Linux/2019-08/159986.htm): arch 上默认只有第一种
  > ip addr  
  hostname -I  
  ifconfig  
- [局域网IP段.2016-09-17 21:26  mvpbang](https://www.cnblogs.com/xiaochina/p/5823451.html)
- [树莓派入门第一步 - 装系统并配置镜像、SSH](../raspberrypi)
- [树莓派无显示屏入门的方法总结](../rbpnoscreen)