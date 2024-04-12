# wayland xmodmap 键盘按键映射失效，转 evremap。wps fcitx5 输入法失效
- date: 2024-04-12

Plasma 升级时自动的把 x11 换成了 wayland，导致 xmodmap 的映射失效了

下面确认确实是 wayland

```bash
$ echo "$XDG_SESSION_TYPE"
wayland

$ env | grep -E -i 'x11|xorg|wayland'
XDG_SESSION_TYPE=wayland
MEMORY_PRESSURE_WATCH=/sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/session.slice/plasma-kwin_wayland.service/memory.pressure
WAYLAND_DISPLAY=wayland-0
QT_WAYLAND_RECONNECT=1
```


之前修改按键使用 [Xmodmap](https://wiki.archlinux.org/title/Xmodmap), KDE 把 x 升级到 Wayland 后，修改按键的方式就失效了，[stackexchange](https://unix.stackexchange.com/questions/292868/how-to-customise-keyboard-mappings-with-wayland) 说由于配置 [xkb](https://wiki.archlinux.org/title/X_keyboard_extension) 有点复杂（里面也有基本教程），回答里推荐用 [Input Remapper](https://aur.archlinux.org/packages/input-remapper-git) 这个软件

KDE Plasma 的 系统设置/输入输出-键盘/键盘-高级,可以勾选一些特殊按键的设置，比如大写按键设置为 ctrl,不过没有直接设置按键的，通过观察发现其对应的配置文件是 ~/.config/kxkbrc，[帮助](https://docs.kde.org/stable5/en/plasma-desktop/kcontrol/keyboard/advanced.html)里说“creating custom keyboard layouts for X11 using XKB.”，关键现在不是 X11 了啊。手动尝试修改这个配置文件也没成功把 alt_r 映射为 w

论坛里的[Remapping keys in Linux 2022](https://forum.endeavouros.com/t/remapping-keys-in-linux/24963/3)有这两方案：xmodmap、gnome setting-keyboard。尝试 [input-remapper](https://aur.archlinux.org/packages/input-remapper-git)，下载打开了GUI不知道咋用

## evremap

[evremap](https://aur.archlinux.org/packages/evremap-git) 比 input-remapper 少了 GUI，好在文档还是简单，可以查看设备列表和按键列表（对于w按键失效的人来说复制w比输入w更容易）。相比于 xmodmap，evremap 还可以指定设备。evremap 打包的老哥说自己不用这个了，ta 转用 [kmonad](https://github.com/kmonad/kmonad)，我没看懂怎么配置 kmonad

```bash
$ mkdir ~/.config/evremap
$ nano ~/.config/evremap/remap.toml # 我的配置就是把笔记本键盘的 alt_r 映射为 w
$ cat ~/.config/evremap/remap.toml
# The name of the device to remap.
# Run `sudo evremap list-devices` to see the devices available
device_name = "AT Translated Set 2 keyboard"

# Run `evremap list-keys` to see the keys available
[[remap]]
input = ["KEY_RIGHTALT"]
output = ["KEY_W"]

# 运行测试一下，ctrl+c 退出测试
$ evremap remap ~/.config/evremap/remap.toml

# yay 安装时已经将 evremap.service 拷贝好了：  /usr/lib/systemd/system/evremap.service
# 查看服务文件可知，其从 /etc/evremap.toml 加载配置，下面将配置文件移动一下然后开启服务
sudo mv ~/.config/evremap/remap.toml /etc/evremap.toml
sudo systemctl enable evremap.service --now
```

# Refer

- [Input remap utilities - Arch wiki](https://wiki.archlinux.org/title/Input_remap_utilities)
- [正确地在 wayland 下配置 KDE 的 Fcitx5](https://zhuanlan.zhihu.com/p/685231455)：不是很信服，还是得看 [Using Fcitx 5 on Wayland](https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland)
- [在KDE Wayland的Fcitx5建议设置下修复WPS Office的输入法问题 wszqkzqk on March 9, 2024](https://wszqkzqk.github.io/2024/03/09/WPS-Fcitx5/):修改 .desktop 文件确实有效。在菜单栏编辑wps相关的应用程序，在应用程序的指令前边加上 env Exec=env XMODIFIERS=@im=fcitx GTK_IM_MODULE=fcitx QT_IM_MODULE=fcitx SDL_IM_MODULE=fcitx GLFW_IM_MODULE=ibus  ，和后边的 /usr/bin 要用空格分开，然后 kde 会自动的把修改后的 .desktop 文件保存到 ~/.local/share/applicwpations/
    > 单独给WPS Office添加环境变量
