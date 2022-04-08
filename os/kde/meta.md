# 修复 KDE Plasma 下 Meta/win 键无法打开菜单/程序启动器的问题
- date: 2022-04-8
- lastmod: 2022-04-8

## 前言

在 windows OS 中，按下 win 就可以启动菜单栏去选择要启动的程序。在 KDE 桌面下这个东西叫做程序启动器（也有好几个替代品），同时这个 win 键换了一个名称 Meta 或者 Super。刚开始装好的 KDE 默认是可以按下 Meta 启动程序启动器的，如果右键该程序启动器可以从配置里看到快捷键对应的设置。

## 问题来源

今天看到 KDE 美化系列，发现 latte-dock 挺好看的，下载尝鲜，很新奇啊，和 mac 风格一致的程序菜篮子，顶部任务栏映射菜单，华而不实一会儿就被我卸载了。但是由于配置 latte 的时候把自己原来配置好的菜单栏删除了，又按照记忆重新配置了一遍，但是 Meta 却无法启动程序启动器了。。。只能鼠标点击启动或者设置两个组合及以上的快捷键。

当然 alt+space 可以打开 krunner 输入程序名然后启动，但是还是想一键到位，然而程序启动器却无法设置单独按键的快捷键...

## 解决办法

在 manjaro forum 和 bing 搜索 super meta shortcut launcher 得到的差不多都是快捷键设置为 Meta+F1 或者 Alt+F1。但是我设置之后却无效（注销和重启神技已经叠加）。

最后不得已，参照 csdn 的帖子修改 `~/.config/kwinrc`。发现是 latte 的锅，它修改了 Meta 的值，卸载之后还是 latte 的设置，一点都不人性，不想 guesture 在修改系统级别的设置前都会先提示用户备份谨慎操作。

1. 将程序启动器的快捷键设置为 Meta+F1 或者 Alt+F1
2. 注销重新登陆，测试 meta 单独摁下是否生效。生效即可结束，不生效继续第三步
3. 修改 `~/.config/kwinrc` 配置文件中的 Meta，使其为 `org.kde.plasmashell,/PlasmaShell,org.kde.PlasmaShell,activateLauncherMenu`
4. 注销重新登陆，测试 meta 单独摁下是否生效。生效即可结束，不生效到论坛反馈。

# 参考
- [KDE Meta key doesn’t open menu after update](https://forum.manjaro.org/t/kde-meta-key-doesnt-open-menu-after-update/74991)
>     selecting Configure Application Launcher,
>    in Keyboard Shortcuts add Meta + F1

- [kde plasma 中使用Meta/Super/win 键打开应用程序启动器（Launcher）. ToolPaPa. 2020-02-15 ](https://blog.csdn.net/blog_zihao/article/details/104327682)
> 1、编辑~/.config/kwinrc下的的配置文件
> 2、加入下面两行（已有的话替换）
> [ModifierOnlyShortuts]
> Meta=org.kde.plasmashell,/PlasmaShell,org.kde.PlasmaShell,activateLauncherMenu
> 3、重启

- [Super key shortcuts don’t seem to work anymore. 2021](https://forum.manjaro.org/t/super-key-shortcuts-dont-seem-to-work-anymore/91457)：issue给出的解决办法：killall -9 gsd-media-keys，我查了我一下我没有这个进程...