# Arch 新手的基本知识
- date: 2021-05-28
- lastmod: 2022-09-23

# 安装系统
## 镜像+网络安装

详细请看[Arch Linux多系统安装安装记录和蓝牙、fcitx5输入法-win\deepin\arch Kearney form An idea 2021-02-21](https://blog.csdn.net/weixin_43031092/article/details/113881097)

# 基础配置

## 输入法

这里示范的是 fcitx5，主要是 fcitx 装完有点问题，搜狗输入法界面不喜欢

```bash
sudo pacman -S fcitx5-im fcitx5-chinese-addons

# 修改配置
nano ~/.pam_environment

# 粘贴进下面的内容，保存退出
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
SDL_IM_MODULE DEFAULT=fcitx
```

## 蓝牙

> 类似于 hcitool 和 hciconfig 的工具已经停止开发，bluez-utils 里也没有包括，所以尽量不要使用它们。

```bash
# 安装蓝牙相关的包
sudo pacman -S bluez bluez-utils
# 设置登陆成功自动开启蓝牙服务
systemctl enable bluetooth.service

# 设置开机后就启动蓝牙服务
sudo nano /etc/bluetooth/main.conf
# ctrl + w 搜索 AutoEnable，去掉 # 注释并将其值修改为 true
AutoEnable=true
```

实际上到第二步`自动开启蓝牙服务`就可以满足基本使用了，登陆成功才能使用蓝牙键盘鼠标。但是如果是蓝牙键盘输入密码的话，那就需要第三步了，不然在登陆的时候无法使用蓝牙键盘，还要身子前倾多尴尬。

# 进阶配置

## 人脸识别 - howdy

[在 Arch Linux 上使用人脸识别(howdy)来登陆和认证 ](https://backmountaindevil.github.io/post/linuxhello/)

## 蓝牙

多 OS 下蓝牙配对问题解决办法看[这里](https://blog.csdn.net/weixin_43031092/article/details/115298442)

## AUR - yay

see [Arch Pacman & Yay & 更新中无法满足依赖关系的解决办法](https://backmountaindevil.github.io/#/os/arch/pacman) 中关于 yay 的安装和使用部分

# 参考

[Arch Linux多系统安装安装记录和蓝牙、fcitx5输入法-win\deepin\arch Kearney form An idea 2021-02-21](https://blog.csdn.net/weixin_43031092/article/details/113881097)

[双系统蓝牙键盘的共享配对解决办法的简要步骤：win + arch～IRK、LTK、ERand、EDIV Kearney form An idea 2021-03-29](https://blog.csdn.net/weixin_43031092/article/details/115298442)

[在 Arch Linux 上使用人脸识别(howdy)来登陆和认证 ](https://backmountaindevil.github.io/#/os/linux/linuxhello)

[Turn on bluetooth on login screen 2015](https://unix.stackexchange.com/questions/197212/turn-on-bluetooth-on-login-screen)

[Bluetooth - Auto power-on after boot Arch Wiki](https://wiki.archlinux.org/title/Bluetooth)


# End