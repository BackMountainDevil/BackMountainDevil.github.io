# ufw 让防火墙变得方便使用
- date: 2024-12-24

ufw 是 Uncomplicated Firewall 的缩写，翻译为 简单的防火墙，原因是 iptables 配置有点复杂，ufw 再封一层用来简化操作

ufw 默认是关闭状态，开启后使用的默认规则是禁止了大部分入站规则。

- 入站：外部网络访问主机，比如访问部署在主机的网页服务
- 出站：主机访问外部网络，比如 bing

需要注意的是，若是 ssh 远程登录开启 ufw，很有可能造成之后 ssh 登录不上，在 wsl2 里面测试会提示“Command may disrupt existing ssh connections. Proceed with operation (y|n)? ”，此时建议输入n之后回车，测试表明输入y之后不会马上断开 ssh，新建 ssh 连接则不行了，趁 ssh 还在赶紧新增规则放行 ssh。不过依然建议输入n之后回车，比较这样更保险，同时测试表明，可以先在关闭防火墙状态下修改规则放行ssh的端口，然后再开启防火墙。

```bash
$ sudo ufw status verbose   # 查看防火墙状态，不加 verbose 也可以，内容会少一些
Status: inactive

$ sudo ufw enable           # 开启防火墙状态
Firewall is active and enabled on system startup

$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

$ sudo ufw allow 8000           # 开放 8000 端口
Rule updated
Rule updated (v6)

$ sudo ufw status verbose       # 可以看到刚加的规则
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
8000                       ALLOW IN    Anywhere
8000 (v6)                  ALLOW IN    Anywhere (v6)

$ sudo ufw delete allow 8000    # 删除“开放 8000 端口”的规则
Rule deleted
Rule deleted (v6)

$ sudo ufw app list
Available applications:
  OpenSSH
$ sudo ufw allow OpenSSH
Rule added
Rule added (v6)
$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
```

规则文件默认存放在 `/etc/ufw/*.rules`，app 的规则文件放在 `/etc/ufw/applications.d`，默认有一个 22 端口的 OpenSSH，可以直接修改配置文件里的端口

关闭防火墙状态可使用命令 sudo ufw disable

limit 可以用来限制访问频率，在 wsl2 上测试不太行，在真机上测试却可以，如下所示：

```bash
# wsl2 中的 ubuntu

$ sudo ufw limit 2222
Skipping unsupported IPv4 'limit' rule
Skipping unsupported IPv6 'limit' rule
$ sudo ufw limit OpenSSH
Skipping unsupported IPv4 'limit' rule
Skipping unsupported IPv6 'limit' rule
$ ufw version
ufw 0.36.1
Copyright 2008-2021 Canonical Ltd.

# ufw limit SSH
ERROR: Could not find a profile matching 'SSH'
# ufw limit ssh
Skipping unsupported IPv4 'limit' rule
Skipping unsupported IPv6 'limit' rule

# 真机测试
$ ufw version
ufw 0.36
Copyright 2008-2015 Canonical Ltd.
$ sudo ufw limit ssh
Rule added
Rule added (v6)
$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
111/tcp                    ALLOW       Anywhere                  
22/tcp                     LIMIT       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
111/tcp (v6)               ALLOW       Anywhere (v6)             
22/tcp (v6)                LIMIT       Anywhere (v6)             

$ sudo ufw delete limit ssh
```

# refer

[UFW](https://help.ubuntu.com/community/UFW)

[Uncomplicated Firewall](https://wiki.archlinux.org/title/Uncomplicated_Firewall)：IP名单、禁用ping

[UFW won't add rate-limiting](https://forums.raspberrypi.com/viewtopic.php?t=264515)
