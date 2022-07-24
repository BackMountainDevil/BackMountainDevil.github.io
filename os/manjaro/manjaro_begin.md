# Manjaro 入门
- date: 2021-09-05
- lastmod: 2022-04-8

manjaro-kde-21.1.1-minimal-210827-linux54.iso
lts 版本安装一直卡在 boot 之后的终端界面，到达 reach graphic interface 就不动了

manjaro-kde-21.1.1-minimal-210827-linux513.iso

pamac 具有Alpm、AUR、Flatpak和Snap支持的软件包管理器
這下子可以在同一個搜索框框裏直接搜索多個庫中的 pkg 了。有 AUR 了還要啥 alatpak 和 Flatpak和Snap

# 系统启动选择菜单 grub2

配置文件为 `/etc/default/grub`，每次修改后需要 `sudo update-grub` 使更新生效。

GRUB_TIMEOUT 等待用户操作的超时时间（秒），0 -> 10， -1 -> 无穷。目前我设置 1

# 语言

## 主目录下文件夹中文名称改为英文

- [Manjaro主目录下文件夹由中文改为英文. thepoy. 2020.01.31](https://www.jianshu.com/p/2f05a5583609)

```bash
sudo pacman -S xdg-user-dirs-gtk
export LANG=en_US
xdg-user-dirs-gtk-update	# 更新语言

sudo pacman -Rs xdg-user-dirs-gtk
```

## 中文输入法

- [Fcitx5](https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E8%BE%93%E5%85%A5%E6%B3%95%E6%A8%A1%E5%9D%97)
> `sudo pacman -S fcitx5-im fcitx5-chinese-addons`

不得不说 deian 默认配置好了中文输入法真舒服，manjaro 虽然安装选择了区域，但也只是做了语言包的下载替换没有整对于的输入法。

中文 wiki 里是这么做的，编辑 `~/.pam_environment` 文件，写入下面的内容

```bash
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx
INPUT_METHOD  DEFAULT=fcitx
SDL_IM_MODULE DEFAULT=fcitx
```


# konsole 该 zsh 为 bash

konsole 标题显示 zsh.。编辑配置方案显示命令是 /bin/zsh。但是根据环境变量显示是bash.。但是根据用户目录下的 history 文件显示 konsole 里敲的都记录在 `～/.zhistory`，而 vscodium（ 默认 linux 终端 bash） 敲的都记录在 `～/.bash_history.`。自己更偏向于 bash 多一些，所以就该了，当然使用 zsh 也没有啥太大问题。

echo $SHELL

cat /etc/shells

切换bash： chsh -s /bin/bash
切换zsh： chsh -s /bin/zsh
    在终端app的系统偏好设置里手动设置。

在配置文件方面：

    bash读取的配置文件：~/.bash_profile文件
    zsh读取的配置文件：~/.zshrc文件

# 开机启动时间分析

https://forum.manjaro.org/t/manjaro-booting-is-very-slow-40sec/32489/4

```bash
systemd-analyze	# 开机时间总览
systemd-analyze blame # 开机时间细节
systemctl disable --now lvm2-monitor.service	# 关闭该服务
systemctl mask lvm2-monitor.service
```

# 关机显示 Backlight Brightness

[AMD 安装 Manjaro KDE 驱动安装后续及BackLight:ACPI故障解决. grsharp. 2020-04-24](https://blog.csdn.net/grsharp/article/details/105735792)
> boot启动显示错误信息Failed to start Load/Save Screen Backlight Brightness of backlight:acpi_video0，虽然不影响正常体验（其实系统使用了systemd-backlight@backlight:amdgpu_b10来补充了)所以让报错消失的办法直接屏蔽该服务：
sudo systemctl mask systemd-backlight@backlight:acpi_video0

# pamac 显示无法锁定数据库 unable to lock database

[Pamac update: unable to lock database, Failed to synchronize databases ](https://forum.manjaro.org/t/pamac-update-unable-to-lock-database-failed-to-synchronize-databases/114762)
> pamac update --force-refresh
