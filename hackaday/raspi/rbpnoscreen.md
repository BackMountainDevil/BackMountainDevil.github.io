---
title: "树莓派无显示屏入门的方法总结"
date: 2021-04-27T18:43:27+08:00
lastmod: 2021-04-27T18:43:27+08:00
keywords: ['linux', 'raspberry']
description: "树莓派无无显示屏的方法总结：路由器、采集卡"
tags: ['linux', 'raspberry']
categories: [OS]
author: "筱氚"
---
# 无显示屏

树莓派的小屏幕买来着实鸡肋，看着眼瞎系列。显示器多好，字大亮度够，HDMI 直插。但是有一些人只有一台笔记本屏幕，HDMI接口不支持视频输入，这就引发了我们的问题，我不想再买一个屏幕哇。于是乎要寻找一个办法解决这个问题。

之前呢我是有个大屏幕，宿舍里玩耍很不错，但是接上了树莓派，主机画面和树莓派画面二选一；其次是在实验室的时候没法用这个大屏幕；后来大屏幕卖掉了，只有笔记本屏幕也没法使用，还需要个小屏幕。

## 总结

总结了集中办法如下，精力和财力有限，只测试了手机热点，还是不成功那种。ssh成功了，自动连接网络失败了，目前推测是硬件问题（树莓派2 + 无线网卡），待升级之后再做测试。

# 解决办法

查看了不少的帖子，由于树莓派默认是不开启 SSH 的，因此基本都是用文件的方式开启 SSH 和配置网络，之后用其他办法查看树莓派的 IP。

## 网线直连

用网线将树莓派和电脑连接起来，然后在电脑上查看树莓派的 IP。由于我的电脑没有常规网线接口，此办法不适合我。

## 路由器

用网线将树莓派和路由器连接起来 或者  连接路由器的 WiFi（可用文本配置网络信息），电脑也连接上这个路由器的网络，然后用电脑登陆路由器的管理页面，查看树莓派的 IP。

此办法适合家庭用户，并且知道路由器的管理员帐号密码。由于校园网基本全覆盖，宿舍没装路由器，此办法对我不好使（装一个路由器也是可以的）。

### 电脑开热点

一般来说，电脑通过网卡连接 wifi 之后是无法开启热点的，就好像手机连接 wifi 就不能开热点，因为热点和 wifi 走同一条道路。但是电脑可以曲线联网啊，比如网线、USB 共享网络、sim卡等，然后给树莓派开热点，这样就可以在电脑上查看树莓派的 IP。

将此办法归类到“路由器”的原因是此时的电脑相当于一个路由器。可能有人说，手机开热点行不行呢？主要的问题是手机怎么查看树莓派的 IP,本着打破砂锅问到底的精神，测试了 iphone、coolpad 自带的“设置”是无法查看子设备的ip的，iphone 可以查看有几个子设备，coolpad 可以查找子设备名称及其对于的 mac 地址。

### 手机热点

但是[树莓派4上手(无显示屏)](https://blog.csdn.net/weixin_43895902/article/details/100919851#t5)中有提到一种不需要 ip 的办法，测试四次之后发现不行。。ta 没有自动连接我的手机热点，但是插上显示器显示 ssh 已经被开启，注意这不是我在终端中开启的，而是ssh文件配置开启的，至于为什么没有连接我的wifi不得而知，莫非是硬件问题，在用电脑屏幕后收到连接我的手机热点，查看 ip 之后 ssh连接了树莓派。

## 视频采集卡

之前在弄 howdy 的时候见识了 vlc 的视频流采集功能，随后了解到了视频采集卡，一种将 HDMI 视频输出信号转为 USB 信号输入，借助potplayer、vlc等软件来采集视频画面，这样就能将电脑屏幕作为树莓派的屏幕使用了。

优点是电脑和树莓派之间的画面可以同时存在，但是不能像 ssh 一样 cv 主机代码到树莓派，但是可以查看 ip 后用 ssh。缺点是为了查看 ip 要花钱买个采集卡。

# 文件配置

在[树莓派入门第一步 - 装系统并配置镜像、SSH](https://anidea.gitee.io/post/raspberrypi/)一文中，是通过外接屏幕开机后开启 SSH 和连接 wifi 的，但是本文是特殊情况。在一台已经用显示器配置过的树莓派上我在相同位置没看到下面要配置的两个文件，不知为何没有。按照下面没有配置过的树莓派开机后查看/boot下居然这两个文件不见了。。。

## ssh

插上刚写完系统的 SD 卡，在 boot 根目录（不是rootfs）下新建一个名称为 `ssh` （小写无后缀）的空白文件，这样系统商店后就会自动开启 ssh 服务了。测试后确实是可以开启 ssh 的。


## wifi

在 boot 根目录（不是rootfs）下新建一个名称为 `wpa_supplicant.conf` 的文本文件，将下面的内容修改一下粘贴进去即可。此办法本人测试四次未成功。。。第五次尝试直接编辑`/etc/wpa_supplicant/wpa_supplicant.conf`也没有成功

```bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CN
network={
    ssid="WiFi-名称"
    psk="WiFi-密码"
    key_mgmt=WPA-PSK
    priority=3
}
```

从一台已经配置过的树莓派中，在`/etc/wpa_supplicant/wpa_supplicant.conf`中看到了正常的配置

```txt
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CN

network={
	ssid="wifi-name"
	psk="wifi-password"
	key_mgmt=WPA-PSK
}

network={
	ssid="wifi-name-2"
	psk="wifi-password"
	key_mgmt=WPA-PSK
}
```

## 小结

我是 arch用户，可以终端操作，也可以图形化直接编辑。。

```bash
$ pwd
/run/media/kearney/boot

$ nano ssh
# 啥也不敲， Ctrl + S 保存， Ctrl + X 退出

$ nano wpa_supplicant.conf
# Ctrl + Shift + V 粘贴， Ctrl + S 保存， Ctrl + X 退出
```

# 参考

[树莓派4上手(无显示屏)](https://blog.csdn.net/weixin_43895902/article/details/100919851#t5)：文件配置 ssh 和 wifi，至于如何获取 IP 在文中描述不清楚，究竟需不需要路由器？ 不知道ip的办法测试失败。
>随后即可通过登录路由器找到树莓派的 IP 地址
>ssh pi@raspberrypi.local
>该方法的好处是不用知道树莓派的实际IP地址

[树莓派3B无显示屏安装系统及远程登录](https://zhuanlan.zhihu.com/p/35161674)：文件配置 ssh，网线连接路由器，路由器看IP

[Raspberry Pi的首次使用——远程桌面显示树莓派系统](https://blog.csdn.net/qq_35682844/article/details/78600138)：电脑开热点给树莓派或者网线直连，dos 或者 Advanced IP Scanner 获取ip

[树莓派入门第一步 - 装系统并配置镜像、SSH](https://anidea.gitee.io/post/raspberrypi/)

[ 树莓派首次开机一根网线的控制（ssh与配置文件） 发表于2021 2019-10-06 ](https://www.dazhuanlan.com/2019/10/06/5d99941132ce1/)：未验证，毕竟我的派连网络都不会自己连接
>打开U盘中的cmdline.txt文件，在结尾加入语句ip=...（IP地址）。然后在把自己的网段配成和cmdline.txt里一样的网段，就可以连接树莓派了
