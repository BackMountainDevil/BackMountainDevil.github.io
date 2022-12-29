# ssh shell 远程终端 公钥密钥
- date: 2022-12-29
- lastmod: 2022-12-29

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

`ssh-keygen -t ed25519 -C "$(whoami)@$(uname -n)-$(date -I)"`

`-t ed25519`: 要创建的密钥类型为 ed25519 格式，当然也可以使用4096位的RSA算法替代 `-t rsa -b 4096`

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

# 参考

[OpenSSH - Arch wiki](https://wiki.archlinux.org/title/OpenSSH)

[创建和管理 Azure 中的 Linux VM 用于身份验证的 SSH 密钥 2022/12/06 - 微软](https://learn.microsoft.com/zh-cn/azure/virtual-machines/linux/create-ssh-keys-detailed)
