---
title: "树莓派无屏幕连接之 VNC realvnc-vnc-viewer"
date: 2021-07-18T10:08:46+08:00
lastmod: 2021-07-18T10:08:46+08:00
keywords: ['vnc', 'raspberry', 'realvnc-vnc-viewer', 'tigervnc', '远程桌面']
description: "树莓派无屏幕连接之 VNC realvnc-vnc-viewer"
tags: ['raspberry']
categories: [OS]
author: "筱氚"
---
# 背景
在 [Remote desktop - Arch Wiki](https://wiki.archlinux.org/title/List_of_applications/Internet#Remote_desktop) 中列出了许多远程桌面软件，先后尝试了 KRDC、TigerVNC，最后又回到了最初的起点 realvnc-vnc-viewer。
自己大部分时间用的都是 ssh 远程操纵，偶尔用一下 win10 的远程桌面亮机，这次自己有个需求，想远程桌面整调试，七寸的太小，屏幕我外接电脑用，和树莓派来回切换太麻烦了。树莓派最近装的官方系统自带 vnc。 

## KRDC
于是我看 KRDC 简介上说支持 RDP 和 VNC 协议，下载尝试了好久都没能连接上树莓派，最后被我卸掉了。。。通往失败的方式减一

## tigervnc
CSDN 上有不少帖子都在说用这一个，于是一通操作在树莓派上安装 tightvncserver ，电脑上也装了客户端，在树莓派上跑一下获得一个序号，然后在客户端轻松连接上了。起初啥事也没有，但随着程序的运行才知道问题大了，无法运行程序。原因是啥也不知道，通过 ssh 启动程序也是类似的错误，无法获得窗口载体，还有点奇怪的是通过 tiger 操作的画面和树莓派 HDMI 输出的画面毫无关系，相当于新开了一个桌面。

# realvnc-vnc-viewer
由于 tightvncserver 无法跑起来我需要的程序，便被卸载了，于是试了一下原装的到底可以不可以，最后成功按照想要的样子运行起来。和上面不同的是，它更像是远程接管桌面（类似 teamviewer），所有的操作会同步到 HDMI 输出上；其次是不需要重新设置，开机自动运行，输入 IP 就能连接，tiger 还需要 ip 后面的序号。

## 安装
`yay -S realvnc-vnc-viewer`  
运行之后输入树莓派的 IP 就能愉快的玩耍了，局域网内快速获得树莓派 IP 可以参考使用  [arp-scan](../rasip) | `sudo arp-scan -l`  | 来获取树莓派 IP

## 问题之摄像头画面

GUI 程序成功跑起来之后，获取摄像头画面的时候却没法同步到 VNC 之后，在 HDMI 输出上却清晰可见，查了一下解决了这个问题。在 [树莓派摄像头在桌面不显示 | 树莓派VNC摄像头实时显示。BlainWu 2021-02-02](https://blog.csdn.net/qq_22945165/article/details/113541514) 一文中，作者给出了图文并茂的答案，这里简单总结一下：

VNC Connect 右上角 三条杠 的按钮，点击选择 Options，然后左边选择 Troubleshooting，然后再在右边勾选 Enable direct capture mode，右下角点击 OK（确认）即可。

## 问题之分辨率
[VNC Viewer 设置屏幕分辨率. 淋哥 2019-04-11](www.cnblogs.com/xuchunlin/p/10692957.html)
  > vncserver -geometry 1280x1024
    vncserver -kill:1

我使用无论是 tigervnc 还是现在用的 real vnc viwer，屏幕设置里的分辨率调到最大也只是 800x480（还有一个是 640x480），有的时候显示不全，比如上面设置摄像头的时候，标题栏看不到（没法移动），底部的确认按钮看不到，最后还是靠 Tab 和 回车搞定的。。。

```bash
# 不推荐使用这个办法
$ vncserver -geometry 1280x1024
VNC(R) Server 6.7.2 (r42622) ARMv6 (May 13 2020 19:34:20)
OS: Raspbian GNU/Linux 10, Linux 5.4.79, armv7l
Running applications in /home/pi/.vnc/xstartup
VNC Server catchphrase: "Algebra pencil anvil. Iron samba genesis."
             signature: b-----------------e

Log file is /home/pi/.vnc/raspberrypi:1.log
New desktop is raspberrypi:1 (172.20.10.2:1)
```

然后在打开一个新的 vnc 会话，地址不再是 ip，而是上面给出的 172.20.10.2:1，输入了好几此密码都没对，裂开，第四次输入才登陆成功，分辨率终于整大了。

重启之后发现 vnc 无法正常使用了。。`cannot currently show the desktop`，看来上面的不是一个好办法。。不过 ssh 还是可以用，`sudo raspi-config` 重新设置了分辨率重启又生龙活虎了。所以还是用它来设置吧。

# 参考
- [Remote desktop - Arch Wiki](https://wiki.archlinux.org/title/List_of_applications/Internet#Remote_desktop)
- [realvnc-vnc-viewer. Arch AUR](https://aur.archlinux.org/packages/realvnc-vnc-viewer/)
- [Download VNC® Viewer](https://www.realvnc.com/en/connect/download/viewer/linux/)
- [树莓派摄像头在桌面不显示 | 树莓派VNC摄像头实时显示。BlainWu 2021-02-02](https://blog.csdn.net/qq_22945165/article/details/113541514)
- [VNC Viewer 设置屏幕分辨率. 淋哥 2019-04-11](www.cnblogs.com/xuchunlin/p/10692957.html)
