# OpenWrt 路由器系统
- date: 2022-10-16
- lastmod: 2022-10-29

第一次使用 openwrt 是在 [小米路由器Pro R3p 刷机 Breed Padavan OpenWrt](https://backmountaindevil.github.io/#/hackaday/thing/xmr3pOpenwrt) 中，一个给路由器制定的 linux 系统。

纯官方原安装的 openwrt 是比较简洁的，ftp 管理、tr 做种都需要增加安装配置，比较繁琐，懒人还是建议使用别人配置好的 openwrt、padavan

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

1. 安装软件包：vsftpd 或者 vsftp-tls
2. 开机启动：系统-启动项-vsftpd 启用

ftp 也有一个缺点，就是不能直接播放服务端的视频，打开编辑服务端的文件。

### Q&A

1. 便捷管理用户密码、可浏览目录、读写权限

    luci-app-vsftpd 或者 luci-app-tn-vsftpd. 这两个包长期无人维护被官方剔除了，需要自己到第三方插件库安装。

2. 只允许特定用户登陆

    2.1 创建 ftp 用户

    [create_a_non-privileged_user_in_openwrt](https://openwrt.org/docs/guide-user/security/secure.access)：不给 ssh 权限、不给 sudo 权限、不给 sheel 权限

        opkg install shadow-useradd
        useradd ftpuser
        passwd ftpuser
        mkdir /home/ftpuser -p
        vi /etc/passwd

    [Vsftpd usb3 f2fs setup 2019 richjoh](https://forum.openwrt.org/t/vsftpd-usb3-f2fs-setup/40369/4):  In /etc/passwd you will find something like

        ftpuser:x:1000:1000:GROUP:/mnt/usb:/bin/false

    2.2 编辑 /etc/vsftpd.conf， 开启仅白名单可登录

        userlist_deny=NO
        userlist_enable=YES
        userlist_file=/etc/vsftpd/vsftpd.users

    2.3 编辑 /etc/vsftpd/vsftpd.users，一行一个用户名，只有这里面的用户可以登录 ftp
    

### 相关阅读

- [FTP servers openwrt](https://openwrt.org/docs/guide-user/services/nas/ftp.overview)

- [OpenWrt路由开启FTP服务访问文件 秋之颂 2021-02-23](https://blog.csdn.net/weixin_42708321/article/details/113995786)： luci-app-vsftpd 在 21.02、19.07、18.06 都没有，换个插件源试试；ext3 不是很理解，现在我的硬盘都是 ext4； ntfs的驱动选择 kmod-fs-ntfs 还是 ntfs-3g

- [vsftpd Ubuntu doc](https://help.ubuntu.com/community/vsftpd)：To allow just some users to login

- [vsftp.conf 、user_list、ftpusers 配置文件 往前的娘娘 2017-08-07](https://blog.csdn.net/m0_37355951/article/details/76851098)

- [vsftpd Arch wiki](https://wiki.archlinux.org/title/Very_Secure_FTP_Daemon)： vsftpd 3.0.3-7 在 2021-10-03 被标记为过期

## bt

我没有安装 transmission-remote ,远程管理的不是很需要，可以登陆路由器的tr界面。但是这样有一个问题就是能够连接上这个路由器网络的人都可以访问到这个 tr 管理界面，怎么整 tr 权限管理？

1. 安装 tr 软件包组 和 设备驱动

```bash
opkg update # 更新软件包列表
opkg install transmission-daemon    # 在后台运行的下载程序，没有任何形式的视觉界面
opkg install transmission-cli   # 通过命令操作 daemon
opkg install transmission-web   # 通过 web 页面操作 daemon
opkg install luci-app-transmission  # 路由器管理端-网络 增加 tr 管理入口
opkg install luci-app-transmission-zh-cn    # 上面包的中文翻译

opkg install kmod-usb2 # 安装usb2驱动 ，usb3现在是默认安装
opkg install kmod-usb-ohci  # 安装usb ohci控制器驱动
opkg install kmod-usb-storage   # 安装usb存储设备驱动
opkg install kmod-fs-vfat   # fat32
opkg install kmod-fs-ntfs   # ntfs
opkg install mount-utils    #挂载卸载工具
opkg install block-mount    # 在 luci web 界面的 系统 栏目添加 “挂载点” 设置，需要重启
```

2. 设置开机启动：系统-启动项-transmission
3. 设置 tr 配置文件存储目录、下载默认目录：服务-tr
4. 使用 tr：tr web 界面：路由器ip:9091

### Q&A

1. transmission-daemon 是一个程序， /etc/init.d/transmission 是一个操作前者的脚本

2. 默认全局配置文件：/etc/config/transmission

3. 自定义配置文件：$DIY/transmission/setting.json, $DIY/transmission-daemon/setting.json,

4. [webgui](http://192.168.1.1:9091) 显示 `403: Forbidden Unauthorized IP Address`. 确认三个配置文件里面的 白名单/主机白名单 都关闭。想开启的话检查网段

5. `Error: Unable to save torrent file: No such file or directory` 。这个关系到能不能开机后自动续种，不然重启后续种信息、下载信息全无

    [Transmission openwrt](https://openwrt.org/docs/guide-user/services/downloading_and_filesharing/transmission) Note 有说是因为权限问题，确认有效

    ```
    # settings.json is created with root permissions by default. It can cause an error:
    # Error: Unable to save resume file: No such file or directory
    # root     root          1818 Dec 27 21:00 settings.json
    
    # this command can help:
    chown transmission:transmission settings.json
    
    #Also you can prevent this error if you set correct permissions for the download directory... Make sure that transmission is the owner of every path that is set in /etc/config/transmission 
    chown -R transmission:transmission /transmission/
    ```

    [Configuration-Files.md](https://github.com/transmission/transmission/blob/main/docs/Configuration-Files.md):如果 添加新种子 resume 中没有新纪录，说明重启就凉凉

        torrents/
        这个子文件夹存放着已经添加到Transmission中的.torrent文件。这个文件夹中的文件是用torrent的名字（以使其可被人类阅读）和torrent的一部分SHA1哈希值（以避免类似名字的torrent造成的文件名冲突）组合命名的。
        resume/
        这个子文件夹保存着关于特定torrent的信息的.resume文件，例如哪些部分已经被下载了，下载的数据存储在哪个文件夹里，等等。这些文件采用了与torrent子文件夹中的文件相同的命名方式。


6. 重载设置

    [Editing-Configuration-Files.md](https://github.com/transmission/transmission/blob/main/docs/Editing-Configuration-Files.md)：恩山里漏了 -HUP

        You can make the daemon reload the settings file by sending it the SIGHUP signal. Or simply run either of the following commands:

        $ killall -HUP transmission-daemon

        Or:

        $ pkill -HUP transmission-daemon

7. tr web GUI 添加账号密码：开启 PRC 授权认证，设置用户名密码

8. 日志文件，刚开始配置的时候老是报错提示信息也不是很丰富，也没找到日志，最后在源码仓库里全局搜索 log 然后过滤掉代码只看文档在 [/docs/Headless-Usage.md](https://github.com/transmission/transmission/blob/4.0.0-beta.1/docs/Headless-Usage.md) 找到了一个设置日志文件的办法，看起来不像是长期设置，而且这个文档在当前稳定版 3.00 tag 中不存在，同时 3.00-18 中没有 --logfile 这个参数选项

## webdav

[WebDAV Share](https://openwrt.org/docs/guide-user/services/nas/webdav)
    [Lighttpd webserver](https://openwrt.org/docs/guide-user/services/webserver/lighttpd)

[OpenWRT配置Webdav(s)共享文件 By HKL 2020-07-22](https://cloud.tencent.com/developer/article/1666367)

# Q&A

1. [批量更新 OpenWRT 软件包](https://panfake.com/2022/05/upgrade-openwrt-opkg/)：先 opkg list-upgradable 再考虑要不要升级

    ```
    # 更新软件包源
    opkg update
    # 仅更新LuCI相关软件包
    opkg list-upgradable | grep luci- | cut -f 1 -d ' ' | xargs opkg upgrade
    # 更新全部可更新软件包，包含OpenWRT内核等
    opkg list-upgradable | cut -f 1 -d ' ' | xargs opkg upgrade
    ```

2. 相关论坛有哪些？

- [OpenWrt Forum](https://forum.openwrt.org)
- [koolcenter (原koolshare)](https://www.koolcenter.com/)
- [恩山无线论坛](https://www.right.com.cn/FORUM/forum.php)

3. OpenWRt 如何开启  UART/TTL？以防万一用来救砖

    ```bash
    fw_setenv boot_wait on
    fw_setenv uart_en 1
    ```

# 相关阅读

[OpenWrt有哪些实用的插件？](https://www.zhihu.com/question/29637794):上网行为管理、广告净化、下载

[penWrt 安装完整管理界面中文语言包 彧繎叔叔，路由刷机，2022-02-16](https://opssh.cn/luyou/191.html)

[我的家庭媒体服务器 篇一：科普向:一文搞懂什么是串流、硬解、转码，你的nas真的需要硬解吗？2022-10-16](https://post.smzdm.com/p/alldz028/)：自用暂时不需要

[VLC -- 使用VLC串流播放视频 第一序列丶于 2017-07-17](https://blog.csdn.net/csdn_of_coder/article/details/75268937):每次 C/S 端都需要设置

- [ L大PandoraBox19.06安装的Transimission种子在重启路由后丢失 toseeme 2019-7-31](https://www.right.com.cn/forum/thread-845858-1-1.html)：确实该延迟 tr 启动，我重启后 ftp、tr、ssh 都开好了，usb 挂载点能看到，但是 ftp 中显示不出来 usb 内容，说明挂载的时间有点慢

    1. 将/etc/init.d/transmission文件里，在START=99后一行加入sleep 30, 因为挂载的硬盘启动时会比tr要晚，所以推迟一点启动程序。
    2。将配置文件路径修改到挂载的硬盘下，不要放在tmp目录下，因为tmp目录重启后会清空的。（cp -r /tmp/transmission /挂/载/目/录）
    3. 重启路由器。记得这里有一个坑(不要在恢复校验种子的时重启，会损坏下载的文件，检验中的文件都要重新下载。我现在就在下100G的之前已经下载过的文件

- [Transmission出现403的解决办法 2020-03-12 发布在 软路由](https://3mile.github.io/archives/2020/0312182618/):option rpc_host_whitelist 改网段。测试不行，我的网段也确认过了在1

- [OpenWrt 路由器如何让 lan 口主机获得 ipv6 网络访问？](https://www.zhihu.com/question/29667477/answer/47149165):修改/etc/config/dhcp ,测试无效
## 校园网认证

总的来说，如果是要每次都输入账号密码就写脚本开机启动，如果是认证一次之后就靠 MAC 认证，改路由器 MAC 就行 

[openwrt下实现校园网的web认证 a1094174619 2018-10-16](https://blog.csdn.net/a1094174619/article/details/83068307)：curl后总结写了个web

[openwrt下实现校园网的web认证 ccpd_1 2021-08-09](https://blog.csdn.net/qq_30763587/article/details/119523231)：如何从 firefox F12 获取 curl 参数

[ruijanlee/h3cc](https://github.com/ruijanlee/h3cc)：H3c connect - 强力插入你的校园网

[使用OpenWrt +mentohust打造锐捷校园网无线路由 weixin_33872660 2011-04-08](https://blog.csdn.net/weixin_33872660/article/details/93273582)
> 要用openwrt上校园网，只要把libpcap和mentohust装上路由就好