# linux下的流量监控之应用程序防火墙
- date: 2022-10-03
- lastmod: 2022-10-03

今天我发现使用 wps 打开 pdf 文件的时候，硬盘扩展坞会被激活并且硬盘发出的声音就好像被扫描了一边目录（使用文件管理器打开休眠的硬盘时也是这个声音），刚开始我不太确信，一番操作反复试验后得到结果就是会激活硬盘扩展坞，至于原理和过程不清楚。

然后想起了在 windows 下用火绒流量监控封锁 wps 云服务的操作，看了下 火绒在 linux 端现在还是只有企业版，于是得找替代品了，endevaourOS 默认安装的 firewalld 研究了一下发现 gui 也不太会设置，就弄懂了一个一键断网。。。

# gufw 

有人在论坛回复说 opensnitch 可以试一试，我发现 yay 时安装体积庞大，将近 500 M，不太合理，第一反应就是通过搜索引擎查找 opensnitch 的替代品，于是就找到了 [gufw](https://costales.github.io/projects/gufw/)，其它的替代品要么是找不到安装包，要么就是官网都变成广告页面了。

添加新的防火墙规则，默认是预配置规则，应用程序中可以搜到mysql、jellyfin、qBittorrent、0AD、Skype等，就是没有 firefox、wps。report 中有 avahi-daemon, NetworkManager, qbittorrent，但是没有抓到 firefox 咧？？

<details>
<summary>默认可选的程序</summary>


```bash
$ sudo ufw app list
  AIM
  Bonjour
  CIFS
  DNS
  Deluge
  IMAP
  IMAPS
  IPP
  KTorrent
  Kerberos Admin
  Kerberos Full
  Kerberos KDC
  Kerberos Password
  LDAP
  LDAPS
  LPD
  MSN
  MSN SSL
  Mail submission
  NFS
  POP3
  POP3S
  PeopleNearby
  SMTP
  SSH
  Socks
  Telnet
  Transmission
  Transparent Proxy
  VNC
  WWW
  WWW Cache
  WWW Full
  WWW Secure
  XMPP
  Yahoo
  qBittorrent
  svnserve
```
</details>

- [Uncomplicated Firewall - ArchWiKi](https://wiki.archlinux.org/title/Uncomplicated_Firewall)

- [如何在Linux上使用GUFW设置防火墙 码农家园](https://www.codenong.com/f-set-up-firewall-gufw/):包含禁止mysql的案例

# opensnitch

总结：可以像火绒流量监控模块那样有新进程访问网络时会自动弹窗询问是否禁止，默认十秒不回答就禁止。谁在发送数据、发送去哪都很清晰。

待优化之处：
1. 流量记录，周报、月报、总和
2. ui 中的列无法设置显示哪些数据项
3. 弹窗拦截 ui 左下角的按钮宽度无法完全显示文本内容

第一反应首先是 yay 安装呗，然后发现编译出 bug，一看是网络问题啊，那不太好解决，科学要掏钱的，于是自己到 [github releases](https://github.com/evilsocket/opensnitch/releases) 手动下载最新稳定版的 deb，又想起以前的 deb2zst，一顿操作猛如虎就安装好了。

启动了 opensnitch-ui 发现没有啥窗口出现，原来是默认摆在任务栏里了，但是打开界面也没有看到啥数据啊，结果一会就没有反应了，用 ps 查也不是僵尸进程，还处在 Ss 状态，但是 Plasma 都询问我是否终止该程序了。。点终止也无效啊，直接在管理器里结束进程。

AUR 评论中没有看到我这类网络问题，于是到 [github wiki](https://github.com/evilsocket/opensnitch/wiki/) 看了一下安装步骤，发现我只安装了 gui，没有安装 daemon。。。于是又去下载 daemon 的 deb 转码再安装

```bash
$ yay opensnitc
==> 正在开始 build()...
go: github.com/golang/protobuf/protoc-gen-go@latest: module github.com/golang/protobuf/protoc-gen-go: Get "https://proxy.golang.org/github.com/golang/protobuf/protoc-gen-go/@v/list": dial tcp 172.217.163.49:443: i/o timeout
==> 错误： 在 build() 中发生一个错误。    正在放弃...

$ yay debtap
$ debtap -q python3-opensnitch-ui_1.5.2-1_all.deb 
$ sudo debtap -u
$ debtap -q python3-opensnitch-ui_1.5.2-1_all.deb 
$ sudo pacman -U python3-opensnitch-ui-1.5.2-1-any.pkg.tar.zst
$ debtap opensnitch_1.5.2-1_amd64.deb
$ sudo pacman -U opensnitch-1.5.2-1-x86_64.pkg.tar.zst
$ sudo systemctl enable --now opensnitch
$ sudo systemctl start opensnitch
```

# 相关阅读

- [RHCSA 系列（一）: 回顾基础命令及系统文档 Gabriel Cánepa 译者： LCTT 白宦成 2015-09-02](https://linux.cn/article-6133-1.html)：查firewalld封控流量发现的，挺好的一个系列

- [How to use firewalld to block wps? 2022.10.3 anidea](https://forum.endeavouros.com/t/how-to-use-firewalld-to-block-wps/32421):我在论坛的提问
> I don’t think that Firewalld blocks applications, it blocks network traffic.

	You might try opensnitch. I don’t use it but I believe it is capable of application level blocking.
