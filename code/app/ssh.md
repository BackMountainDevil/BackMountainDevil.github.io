# ssh shell 远程终端 公钥密钥
- date: 2022-12-29
- lastmod: 2024-12-11

ssh 是一种加密网络协议。应用上可以理解为远程终端。它具体实现有几个应用，常用的是 openssh，通常在 linux 系统上会默认安装。

# 安装

`sudo pacman -S openssh`

根据包管理器进行安装即可，一般都默然安装。不过不一定默认启用，服务端上启动该服务的方式如下

```bash
sudo systemctl enable sshd --now    # 启动并开机自启
```

## 公钥认证

这种方式比密码登陆安全一些。要在服务端修改配置，然后把公钥整到服务器上。

### 密/公钥生成

`ssh-keygen -t ed25519 -N '' -C "$(whoami)@$(uname -n)-$(date -I)"`

`-t ed25519`: 要创建的密钥类型为 ed25519 格式，当然也可以使用4096位的RSA算法替代 `-t rsa -b 4096`

`-N ''`： 表示不使用密码短语（passphrase），加入密码短语时使用密钥登录则需要再次输入密码，相当于多一重密码

`-C "$(whoami)@$(uname -n)-$(date -I)"`: 公钥文件末尾注释，通常用邮件地址用作注释，这里用生成密钥的主机名和用户名加上日期作为备注

举个例子

```bash
ssh-keygen -t ed25519 -C "mifen@qq.com-$(date -I)"
# 三次回车
    # 确认秘钥的保存路径（如果使用默认路径则直接回车）；如果已经有秘钥文件，则需要确认是否覆盖（如果之前的秘钥不再需要则直接回车覆盖，如需要则手动拷贝到其他目录后再覆盖）；
    # 创建密码（如果不需要密码则直接回车）,强烈建议使用密码！； 
    # 确认密码；
# 在 ~/.ssh（默认路径）下会生成两个新文件：id_ed25519(私钥)和id_ed25519.pub(公钥)
# 查看公钥
cat ~/.ssh/id_ed25519.pub

# 修改文件权限，避免 WARNING:UNPROTECTED PRIVATE KEY FILE!
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
chown -R $USER ~/.ssh
```

这下子把公钥分发出去，只有拥有密钥的才能和公钥匹配，哪如果有人把你的密钥复制了一份怎么办？别忘了上面不是有给私钥设置密码的步骤嘛，如果觉得还不够安全，那只好再加上双因素认证了

### 服务端配置

```bash
sudo nano /etc/ssh/sshd_config  # 编辑配置文件，把下面四行配置复制进去

PasswordAuthentication no       # 不允许密码登陆
AuthenticationMethods publickey # 公钥认证
Port 39901                      # 修改端口号，记得开放防火墙
AllowUsers    user1             # 只允许 user1 进行登陆

sudo systemctl restart sshd     # 重启 sshd，使新配置生效

nano ~/.ssh/authorized_keys     # 编辑公钥认证文件，把上面生成的公钥cv进去即可
```

# 客户端使用

常规通过账号、密码进行登录，默认端口（port）为 22

```bash
ssh -p port user@server-address
```

## 配置

这样做的目的是方便登陆和记忆，不用每次输入账号密码

```bash
nano ~/.ssh/config  # 编辑配置文件

# 下面就是文件内容了
Host hp # 主机别名
    Hostname 2001:2001:2001:2001::2:1   # ip 或者 域名
    Port     222                        # ssh 服务端口
    User zhangsan                       # 用户名
    IdentityFile ~/.ssh/id_ed25519_zs   # 认证密钥文件

Host hw
    Hostname 101.101.101.101
    Port     1922
    User lisi
    IdentityFile ~/.ssh/id_ed25519_ls
```

这样就可以直接 ssh hp 或者 ssh hw 登录对应的服务端了，不再需要输入一长串的内容

# 端口转发

```bash
$ python -m http.server --bind :: 8000 # “8000” 是指端口号，参数 “--bind ::” 是指同时监听 ipv6 地址的访问

$ curl localhost:8000 # 正常会返回 html 内容

# TODO test
ssh -L 9999:localhost:80 user@remote-server
当你访问本地计算机的9999端口时，实际上是在访问远程服务器的80端口

将远程服务器的8888端口转发到本地计算机的80端口：
ssh -R 8888:localhost:80 user@remote-server

创建一个监听在本地1080端口的SOCKS代理：
ssh -D 1080 user@remote-server
```
https://wty-yy.xyz/posts/44423/

# FAQ

1. WARNING:UNPROTECTED PRIVATE KEY FILE! Permission denied

```bash
[edifier@hp .ssh]$ $ ssh server
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/home/edifier/.ssh/server' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/home/edifier/.ssh/server": bad permissions
edifier@10.11.12.13: Permission denied (publickey).

[edifier@hp .ssh]$ ls -l
总计 48
-rw-r--r-- 1 edifier edifier  419  6月 3日 21:56 server
-rw------- 1 edifier edifier 3381 2023年11月 1日 5090_edifier
-rw-r--r-- 1 edifier edifier  313 2023年10月19日 authorized_keys
-rw-r--r-- 1 edifier edifier  828  6月 3日 21:59 config
-rw------- 1 edifier edifier  411 2023年 7月29日 id_ed25519
-rw------- 1 edifier edifier 2888  6月 3日 21:58 known_hosts
```

原因是密钥的文件权限过于宽松，可以看到上面 server 密钥的权限是 644，改成 600 就好了

    chmod 600 server

2. 新发的密钥无法连接

如果新用户在之前有用过旧账户连接过该服务器，有可能会因为新密钥和旧认证方式不同导致无法登录，此时考虑是 known_hosts 冲突，考虑删除对应的数据行。

[Linux 中的known_hosts 文件是什么](https://cn.linux-console.net/?p=19739):config 的 Host 缩写不太管用，Hostname 测试可以
> 如果您知道系统的主机名或 IP 地址，则可以从 known_hosts 获取相关条目：
> ssh-keygen -l -F <server-IP-or-hostname>
>
> 如果您想从known_hosts 文件中删除特定条目，并且您知道远程系统的主机名或IP，则可以执行此操作。
>
> ssh-keygen -R server-hostname-or-IP

# 参考

[OpenSSH - Arch wiki](https://wiki.archlinux.org/title/OpenSSH)

[创建和管理 Azure 中的 Linux VM 用于身份验证的 SSH 密钥 2022/12/06 - 微软](https://learn.microsoft.com/zh-cn/azure/virtual-machines/linux/create-ssh-keys-detailed)

[通过ssh动态端口转发共享校园资源（附带干货）By 苏剑林 | 2016-03-07](https://kexue.fm/archives/3651)