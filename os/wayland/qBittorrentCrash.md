# x11 转 wayland 后 qBittorrent 启动闪退
- date: 2024-04-12

在启动中心（菜单栏）点击 qBittorrent 无法启动，没有错误信息，没有启动画面，就像什么都没有发生。从终端启动也是什么都没有发生。

最近系统进行了升级，可能是 x11 升级到了 wayland

解决办法：删除配置文件夹 ～/.config/qBittorrent/ 即可

```bash
$ printenv |grep way
XDG_SESSION_TYPE=wayland
MEMORY_PRESSURE_WATCH=/sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/session.slice/plasma-kwin_wayland.service/memory.pressure
WAYLAND_DISPLAY=wayland-0
$ qbittorrent -v
qBittorrent v4.6.3
$ qbittorrent
```

[Qbittorrent won’t launch after update to Plasma 6. 2024.3.7](https://forum.endeavouros.com/t/qbittorrent-wont-launch-after-update-to-plasma-6/52047/10)：qt6-wayland 6.6.2已安装；我这里不存在 /usr/local/etc/rc.d/qbittorrent,因为/usr/local/etc是空的；使用 journalctl -f 监听时，启动 qb 依旧失败，监听显示两行（反复尝试启动时，这两行变化的只有 1.176）

    kded6[906]: Registering ":1.176/StatusNotifierItem" to system tray
    kded6[906]: Service  ":1.176" unregistered 

如果在终端尝试带参数启动（参数是从启动中心属性发现的）,启动显示qb弹窗错误信息，标题是“无效 Torrent”，内容是 “加载 Torrent 失败： %U.错误：打开文件出错。文件：“%U”. 错误：“此文件或目录不存在””，有一个确定按钮。点击按钮后弹窗关闭，在任务栏小图标区域（system tray）发现 qb,点击可显示窗口。此时监听显示三行内容，是上面两行的重复，退出qb后监听就显示多一行，是上面两行信息中的最后一行

```bash
$ qbittorrent %U
```