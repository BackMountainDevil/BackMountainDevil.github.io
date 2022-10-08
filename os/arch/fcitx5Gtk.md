# fcitx5 在 coidum 中输入中文会飘字母的问题
- date: 2022-10-08
- lastmod: 2022-10-08

# 问题描述

fcitx5 在 coidum 中输入中文时会出现拼音乱跳，比如我想打出“输入法”，得到的确是“r书法”，并且得到的结果极其不确定，不知道那一个拼音会突然跳出去，但是在其它软件（firefox、mousepad、dolphin、wps）中并无此类问题。

结论： 安装 fcitx5-gtk

## codium

中午刚搜索这个问题，系统升级了一下，但是没有完全升级，AUR 的由于网络更新有点问题就没更。结果更晚企图几个包就好了，没想到晚上这个 bug 又来了。中午的时候它的版本还是 1.71.2.22258-1，下午到 tuna 上下载镜像包进行了更新

```bash
$ sudo pacman -Qi vscodium-bin  # 依赖查询
版本           : 1.72.0.22279-1
依赖于         : fontconfig  libxtst  gtk3  python  cairo  alsa-lib  nss  gcc-libs  libnotify  libxss  glibc>=2.28-4
可选依赖       : gvfs: For move to trash functionality
                 libdbusmenu-glib: For KDE global menu
依赖它         : 无被可选依赖     : 无与它冲突       : vscodium
```

上面的查询结果表面 vscodium 依赖于 gtk3

## diagnose

```bash
$ fcitx5-diagnose   # 运行诊断程序，这里只显示异常部门，下面一样
3.  XIM encoding:

    **Your LC_CTYPE is set to en whose encoding is not UTF-8. You may have trouble committing strings using XIM.**

3.  Qt IM module files:

    **Cannot find fcitx5 input method module for Qt4.**

2.  `gtk-query-immodules`:

    1.  gtk 2:

        Found `gtk-query-immodules` for gtk `2.24.33` at `/usr/bin/gtk-query-immodules-2.0`.
        Version Line:

            # Created by /usr/bin/gtk-query-immodules-2.0 from gtk+-2.24.33

        **Failed to find fcitx5 in the output of `/usr/bin/gtk-query-immodules-2.0`**

        **Cannot find fcitx5 im module for gtk 2.**

    2.  gtk 3:

        Found `gtk-query-immodules` for gtk `3.24.34` at `/usr/bin/gtk-query-immodules-3.0`.
        Version Line:

            # Created by /usr/bin/gtk-query-immodules-3.0 from gtk+-3.24.34

        **Failed to find fcitx5 in the output of `/usr/bin/gtk-query-immodules-3.0`**

        **Cannot find fcitx5 im module for gtk 3.**

3.  Gtk IM module cache:

    1.  gtk 2:

        Found immodules cache for gtk `2.24.33` at `/usr/lib/gtk-2.0/2.10.0/immodules.cache`.
        Version Line:

            # Created by /usr/bin/gtk-query-immodules-2.0 from gtk+-2.24.33

        **Failed to find fcitx5 in immodule cache at `/usr/lib/gtk-2.0/2.10.0/immodules.cache`**

        **Cannot find fcitx5 im module for gtk 2 in cache.**

    2.  gtk 3:

        Found immodules cache for gtk `3.24.34` at `/usr/lib/gtk-3.0/3.0.0/immodules.cache`.
        Version Line:

            # Created by /usr/bin/gtk-query-immodules-3.0 from gtk+-3.24.34

        **Failed to find fcitx5 in immodule cache at `/usr/lib/gtk-3.0/3.0.0/immodules.cache`**

        **Cannot find fcitx5 im module for gtk 3 in cache.**

    3.  gtk 4:

        **Cannot find immodules cache for gtk 4**

        **Cannot find fcitx5 im module for gtk 4 in cache.**

$ sudo pacman -Qi fcitx5
版本           : 5.0.19-1
组             : fcitx5-im
依赖于         : cairo  enchant  iso-codes  libgl  libxkbcommon-x11  pango  systemd  wayland  wayland-protocols  xcb-imdkit  xcb-util-wm
                 libxkbfile  fmt  gdk-pixbuf2  unicode-cldr-annotations
可选依赖       : 无依赖它         : fcitx5-qt  libime
被可选依赖     : 无与它冲突       : fcitx
```

参考一表示重新编译安装gtk-query-immodules，但是这个包在 yay 下没有结果，找了不少发现参考二，可能是缺少 fcitx5-gtkx,用pacman试了一下，只有
fcitx5-gtk 这个包，安装完成重启 coidum、fcitx5 后问题就解决了。此时再运行诊断看一下还有一些是红的，如下所示，但是由于没有影响到我正常使用，忽略。

```bash
3.  XIM encoding:

    **Your LC_CTYPE is set to en_US whose encoding is not UTF-8. You may have trouble committing strings using XIM.**

3.  Qt IM module files:

    **Cannot find fcitx5 input method module for Qt4.**

3.  gtk 4:

    **Cannot find immodules cache for gtk 4**

    **Cannot find fcitx5 im module for gtk 4 in cache.**
```

# 参考

- [fcitx5 中文输入在 chrome/vscode 等应用中的问题及解决 yuzx2008 2022-08-11 ](https://blog.csdn.net/yuzx2008/article/details/126288531)：刚看完这个，貌似全天更新系统后这个问题就消失了，aur软件暂时未更新，就有点离谱，结果晚上这个bug又出现了
> 解决办法，先通过 fcitx5-diagnose 命令查看诊断日志 会提示 gtk query cache 相关问题 编译安装gtk-query-immodules

- [[SOLVED] my fcitx doesn't work 2017-02-16 oldz](https://bbs.archlinux.org/viewtopic.php?id=223189)
    ```bash
    pacman -S gtk2
    pacman -S gtk3
    pacman -S fcitx-gtk2
    ```

## 无效文章

- [ [SOLVED] gtk-query-immodules not detecting fcitx 2019-10-08 thehungryturnip](https://bbs.archlinux.org/viewtopic.php?id=249753):restart Chromium to take effect

- [fcitx5 only works on default application, and cannot work on the third app #384 16 Nov 2021](https://github.com/fcitx/fcitx5/issues/384): 学到了一个 lsb_release -a

- [ fcitx-diagnose结果显示“无法找到 gtk 3 的 fcitx 输入法模块” lanyun7112 2020-09-22](https://bbs.deepin.org/post/202673#mod=viewthread&tid=202673&extra)
> 等以后deepin修复的这个问题再说吧
