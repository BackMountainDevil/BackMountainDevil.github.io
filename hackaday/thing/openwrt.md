# OpenWrt 路由器系统
- date: 2022-10-16
- lastmod: 2022-10-16

第一次使用 openwrt 是在 [小米路由器Pro R3p 刷机 Breed Padavan OpenWrt](https://backmountaindevil.github.io/#/hackaday/thing/xmr3pOpenwrt) 中，一个给路由器制定的 linux 系统。

# 设备支持与安装

OpenWrt所支持的设备可以到 [我的路由器是否受OpenWrt支持？](https://openwrt.org/zh/toh/start) 中进行搜索，通常设备页面会有安装办法。管理页面默认是 http://192.168.1.1/ ，默认用户名 root，，默认没有设置密码，可以直接留空登录

# 基础设置
## ssh

默认关闭，需要到 system - Administration - ssh access 中进行打开

## wifi

wifi 默认关闭，在 Network - Wireless - Edit 设置名称密码后点击 Save&Apply

## 镜像

两种办法二选一
- web 方法：system - software - configure opkg - /etc/opkg/distfeeds.conf 。 把路径都修改为 tuna 的
- ssh 方法（我使用这个）

    ```bash
    sed -i 's_downloads.openwrt.org_mirrors.tuna.tsinghua.edu.cn/openwrt_' /etc/opkg/distfeeds.conf
    opkg update
    ```

- [OpenWRT (LEDE) 镜像使用帮助 TUNA](https://mirror.tuna.tsinghua.edu.cn/help/openwrt/)
> 将其中的 downloads.openwrt.org 替换为 mirrors.tuna.tsinghua.edu.cn/openwrt 即可。

## 中文

1. 安装软件包（System -software）：luci-i18n-base-zh-cn、luci-i18n-opkg-zh-cn
2. 设置中文：System - system - Language

## 备份

系统 - 备份与升级 - 生成备份

### mtd

系统 - 备份与升级 - 保存 mtdblock 内容

```bash
# cat /proc/mtd 
dev:    size   erasesize  name
mtd0: 00040000 00020000 "Bootloader"
mtd1: 00040000 00020000 "Config"
mtd2: 00040000 00020000 "Bdata"
mtd3: 00040000 00020000 "factory"
mtd4: 00040000 00020000 "crash"
mtd5: 00080000 00020000 "crash_syslog"
mtd6: 00040000 00020000 "reserved0"
mtd7: 00400000 00020000 "kernel_stock"
mtd8: 00400000 00020000 "kernel"
mtd9: 0f580000 00020000 "ubi"
```

# 进阶玩法
## ftp

## bt

# 相关阅读

[OpenWrt有哪些实用的插件？](https://www.zhihu.com/question/29637794):上网行为管理、广告净化、下载

[penWrt 安装完整管理界面中文语言包 彧繎叔叔，路由刷机，2022-02-16](https://opssh.cn/luyou/191.html)

## 校园网认证

总的来说，如果是要每次都输入账号密码就写脚本开机启动，如果是认证一次之后就靠 MAC 认证，改路由器 MAC 就行 

[openwrt下实现校园网的web认证 a1094174619 2018-10-16](https://blog.csdn.net/a1094174619/article/details/83068307)：curl后总结写了个web

[openwrt下实现校园网的web认证 ccpd_1 2021-08-09](https://blog.csdn.net/qq_30763587/article/details/119523231)：如何从 firefox F12 获取 curl 参数

[ruijanlee/h3cc](https://github.com/ruijanlee/h3cc)：H3c connect - 强力插入你的校园网

[使用OpenWrt +mentohust打造锐捷校园网无线路由 weixin_33872660 2011-04-08](https://blog.csdn.net/weixin_33872660/article/details/93273582)
> 要用openwrt上校园网，只要把libpcap和mentohust装上路由就好