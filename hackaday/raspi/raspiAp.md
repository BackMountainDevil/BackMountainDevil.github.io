---
title: "树莓派上开热点（AP）的三种办法实践结果"
date: 2021-08-02T12:12:34+08:00
lastmod: 2021-08-02T12:12:34+08:00
keywords: ['raspberry pi', 'ap', 'wifi']
description: ""
tags: ['raspberry']
categories: [dev-init]
author: "筱氚"
---
# 背景
UC2 项目中树莓派大脑和子模块有两种方式连接方式，一种是采用 I2C 总线通过 Arduino 做主从，一种是走 WIFI 使用 MQTT 协议与子模块通信。为了减少线数，采用了后者。实际调试中都是自己手机开热点给 pi、esp、pc,因为校园网不知道不知道为啥无法在一个子网中，也不能说是不在，只是无法进行内网通信。

但是考虑到实际使用中，也会有不存在 wifi 网络覆盖的情况，因此需要某个设备来开启热点来搭建一个 WIFI 子网，esp32 具有这种功能，实现上也比较简单，但是为了便于管理和维护，最后我想的是让树莓派开启热点，其它设备接入它，这样子由于 AP 模式带来的功耗和发热就转移到了树莓派上。

此外，树莓派的 wifi 模块是可以开热点的，但是官方系统中没有默认加入这个功能，需要自己动动那个小手。

## 总结
三种方式中，有官方的也有非官方的，最后实践结果表明官方的文档比 rasap 要繁琐一点，更容易出错，create_ap 团队表示不再维护，而且实践表明在 raspi 4 上行不通，因此这里推荐使用 [rasap](https://raspap.com/#quick)

# 方法汇总
## rasap
最推荐的办法 - [rasap](https://raspap.com/#quick)，一行代码搞定。

```bash
curl -sL https://install.raspap.com | bash
```

回答问题不懂的就一直 Y 回车就行。装完之后需要重启，重启完之后在 pi 上打开浏览器访问 10.3.141.1:80 就能进入管理界面，初始账号密码的初始值在[网页](https://raspap.com/#quick)中有写（admin, secret）。


进去修改下 wifi 的名称和密码，重启 wifi 即可，实测就是改了好几次才连上。

## 官方文档的办法
[树莓派官方文档 - AP](https://www.raspberrypi.org/documentation/configuration/wireless/access-point-routed.md)，文档的办法是在 3B 上测试过的，在 4B 上我自己弄的感觉不太靠谱，偶尔靠运气能连上，大部分时候就是个摆设。。。其中有一部是设置路由转发，如果不打算给子网接入互联网的功能可以跳过那一步。

在编辑 hostapd.conf 的时候需要修改国家代码为中国 -  CN，`country_code=CN`，热点名称和密码不需要加上引号。

在论坛里有个案例（参考四）是由于 iptables 和 ufw 两个冲突导致无法正常使用，看了下自己的 pi 里没有这个冲突，就很迷。

然后使用了这个办法之后是只有 AP 模式了，无法使用原来正常连接 wifi 的功能了，怎么还原我还没发现，瞎逆向捣鼓了一下发现行不通。

## create_ap
按照其[文档](https://github.com/oblique/create_ap)进行安装即可，安装完实测如下，大致意思就是此路不通。

```bash
$ sudo create_ap wlan0 wlan0 test 12345678
WARN: brmfmac driver doesn't work properly with virtual interfaces and
      it can cause kernel panic. For this reason we disallow virtual
      interfaces for your adapter.
      For more info: https://github.com/oblique/create_ap/issues/203
WARN: Your adapter does not fully support AP virtual interface, enabling --no-virt
ERROR: You can not share your connection from the same interface if you are using --no-virt option.
```
卸载办法也比较简单，当初安装是在对应目录下 `make install`，卸载就是在当初那个目录里 `make uninstall`。

# 参考
- [rasap](https://raspap.com/#quick)
- [oblique/create_ap](https://github.com/oblique/create_ap)
- [树莓派官方文档 - AP](https://www.raspberrypi.org/documentation/configuration/wireless/access-point-routed.md)
- [[SOLVED] Wifi Access Point (AP) not handing out IP addresses](https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=315866&p= 1893796& hilit=AP#p1893796)
   > $ sudo apt remove iptables-persistent
    $ sudo ufw allow bootps
    $ sudo ufw allow 53/udp
    $ sudo ufw allow 53/tcp