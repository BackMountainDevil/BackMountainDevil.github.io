# EndeavourOS 初体验 REISUB
- date: 2022-09-23
- lastmod: 2023-5-25

## 设置
### grub

EndeavourOS 并不会主动探测 windows，有点离谱，参考[Grub dual booting](https://discovery.endeavouros.com/grub-and-refind/grub-dual-booting/2021/07/)设置下即可

    sudo pacman -S --needed os-prober
    sudo nano /etc/default/grub # 编辑文档，加入GRUB_DISABLE_OS_PROBER=false
    sudo grub-mkconfig -o /boot/grub/grub.cfg

想调节[grub分辨率、字体大小，看wiki](https://wiki.archlinux.org/title/HiDPI#GRUB)

### 蓝牙

进入桌面在任务栏找到蓝牙按钮开启后进行配对，但是搜索不到设备，查看蓝牙服务状态发现是disable,结果[Enable and setup Bluetooth devices](https://discovery.endeavouros.com/bluetooth/bluetooth/2021/03/)说了由于风险和省电原因默认禁用了，作为一个蓝牙深度用户，自然要开机自启
    sudo systemctl enable bluetooth
    sudo pacman -S --needed bluez bluez-utils pulseaudio-bluetooth
    
### 输入法

参照 arch 时进行安装配置，结果任务栏出现了输入法图标，也有拼音输入，就是没法输入，后面桌面换到 kde 直接就能输入了。。。

第二次给在台式机上装 endeavour 的时候，屏幕是 1080p,这就好办了，尝试下安装时看起来美爆的 xfce,但是在这个输入法问题还在，通过`fcitx5-diagnose`和环境变量之间的不断设置重启，发现 xfce 下的输入法环境变量要设置在 `~/.xprofile`，而在 kde 下 `~/.bash_profile` 设置是可以的。

### 蜂鸣器

1. 在浏览器（如firefox）中查找不到对应词汇时会发出 bee 声音
2. 虚拟终端（ctrl+alt+F2）中无输入时按下 Backspace、Delete
3. 重启/关机

台式机整 BIOS 老是 bee,开盖直接拔掉喇叭输出，接耳机使或者装个蓝牙网卡连蓝牙耳机。

xset b off 试验过重启后失效

[PC speaker Arch Wiki](https://wiki.archlinux.org/title/PC_speaker#Globally):这个办法测试可行

    sudo rmmod pcspkr
    sudo  nano /etc/modprobe.d/nobeep.conf
    加入下面这一行
    blacklist pcspkr

[Manjaro XFCE disable system beep](https://forum.manjaro.org/t/manjaro-xfce-disable-system-beep/102538)

    echo "blacklist pcspkr" | sudo tee /etc/modprobe.d/nobeep.conf After a reboot

[How to disable beep tone in xfce when the delete button is pressed? 2015](https://unix.stackexchange.com/questions/214607/how-to-disable-beep-tone-in-xfce-when-the-delete-button-is-pressed)

     uncommenting set bell-style none (reload command: bind -f ~/.inputrc)? If yes, then try one of the mentioned methods. E.g. by unloading pcspkr module: rmmod pcspkr or by xset b off

### xfce 快捷键

在 xfce 默认设置下，按 win 键（Super、Meta）是没法打开菜单栏的，锁屏快捷键也不一样。我以为是键盘有问题，然后到 设置-窗口管理器-键盘 找一个空的位置试验了下发现按下 win 键得到的是 Super L。我以为是键盘的问题，然后在论坛里搜索了一下，看到一个 [Using Windows (Super L) key for Whisker menu AND tiling shortcuts](https://forum.endeavouros.com/t/using-windows-super-l-key-for-whisker-menu-and-tiling-shortcuts/27784)，里边提到了 设置-键盘-应用程序快捷键 ，`xfce4-popup-whiskermenu` 指的就是弹出菜单栏，默认快捷键是 super+space ，锁屏默认是 Ctrl+Alt+L. 我把菜单栏设置为 super 的时候，按下 super+E 就有点问题了，我想要出现文件管理器，但是现在却会同时出现菜单栏并且聚焦在菜单栏，那还是使用默认快捷键吧。

# 系统维护

[A Complete Idiot’s Guide To Endeavour OS Maintenance / Update / Upgrade](https://forum.endeavouros.com/t/a-complete-idiots-guide-to-endeavour-os-maintenance-update-upgrade/25184)
    更新镜像和系统
    reflector --protocol https --verbose --latest 25 --sort rate --save /etc/pacman.d/mirrorlist
    eos-rankmirrors --verbose
    yay -Syyu
    一个月清除一次i日志
    journalctl --vacuum-time=4weeks
    清除安装包缓存 /var/cache/pacman/pkg
    paccache -r

## xflock4 锁屏失效

更新系统之后无法锁屏，无论是菜单栏锁屏还是快捷键都无效，[wiki](https://wiki.archlinux.org/title/Xfce)显示可以设置锁屏的指令，我的包历史记录显示我之前用的是 xfce4-screensaver，然后我尝试安装并设置 light-locker，还是无法锁屏。说一下锁屏流程，快捷键调用的是 xflock4 这个脚本，这个脚本又会去调用对应的会话管理器（这个就有很多），后者才是真正的锁屏一把手。我敲 xfce4-screensaver-command -l 的时候得到的输出告诉我 xfce4-screensaver 没有在运行，让我先运行它，可是我尝试用 systemctl 启动这个服务却被告知二米有这个服务，于是我输入 xfce4-screensaver 并回车并没有得到什么东西，就在那里不动弹。后来我打开了另外一个终端，我发现锁屏又可以正常工作了，然后当我把前边那个不动弹的关掉之后，锁屏功能又不工作了，因此结论是前面那个不动弹的就是在运行中的守护进程，也就是系统开机的时候并没有启动这个守护进程，于是在 设置管理器-会话和启动-应用程序自启动 中，勾选“屏幕保护”，我是很久之前就取消了这个自启动，因为我不用屏保，咱这也不是显像管显示器时代了，没想到这个屏幕保护程序还兼职“锁定程序”

# 从 xfce 切换到 Plasma KDE

在 live 的时候感觉这个 DE 蛮不错的唉，图标、字体大小调节之后还是存在一些问题，比如fcitx5一直没法工作、任务栏不容易美化设置，还是想用回KDE，但又不想重装系统，于是想着 DE 独立于内核，那保留系统更换 de 理论上是可行的

在论坛搜索发现了相同的问题-[How I can remove XFCE and install KDE (last version)?2021](https://forum.endeavouros.com/t/how-i-can-remove-xfce-and-install-kde-last-version/14123/3)

[How to install Desktop Environments next to your existing ones by adminMarch 24, 2021](https://discovery.endeavouros.com/desktop-environments/how-to-install-desktop-environments-next-to-your-existing-ones/2021/03/)：重启时就可以选择进入plasma还是xfce了，这个时候有两de

    sudo pacman -S plasma-desktop
    sudo pacman -R qt5ct
    sudo nano /etc/environment
    #QT_QPA_PLATFORMTHEME=qt5ct

[Removing a Desktop Environment](https://discovery.endeavouros.com/desktop-environments/removing-a-desktop-environment/2021/03/)

    sudo pacman -Syu
    sudo pacman -Rs arc-gtk-theme-eos endeavouros-skel-xfce4 endeavouros-xfce4-terminal-colors blueberry file-roller galculator gvfs gvfs-afc gvfs-gphoto2 gvfs-mtp gvfs-nfs gvfs-smb network-manager-applet parole ristretto thunar-archive-plugin thunar-media-tags-plugin xdg-user-dirs-gtk xed xfce4 xfce4-battery-plugin xfce4-datetime-plugin xfce4-mount-plugin xfce4-netload-plugin xfce4-notifyd xfce4-pulseaudio-plugin xfce4-screensaver xfce4-screenshooter xfce4-taskmanager xfce4-wavelan-plugin xfce4-weather-plugin xfce4-whiskermenu-plugin xfce4-xkb-plugin

卸载xfce后发现kde下系统设置里缺少蓝牙，[Enable and setup Bluetooth devices](https://discovery.endeavouros.com/bluetooth/bluetooth/2021/03/)
    sudo pacman -S bluedevil    # For KDE

sudo pacman -R xterm    # 卸载 xterm,这个 terminal 对高分屏支持不友好

https://github.com/endeavouros-team/EndeavourOS-packages-lists/blob/master/plasma

    sudo pacman -S sddm-kcm xsettingsd plasma-nm plasma-pa powerdevil # 一些设置管理器的图形版
    sudo pacman -S dolphin konsole ark khotkeys # 文件管理器、终端、压缩软件、热键
    sudo pacman -S gwenview mousepad spectacle # 看图 文本编辑器 截图

kwrite 和 kate 合并在一个包了，捆绑安装让我厌烦，找了一个简单的文本编辑器 mousepad

# tty

我在笔记本和台式机上都装了 endevaourOS，软件上的不同在于一个 DE 是 Kde Plasma，另外一个是 XFCE。起初这也没啥，直到我在台式机上不小心进入了 tty2 再返回 tty1 的时候发现 xfce 桌面不见了，top 也没有发现僵尸进程，而在笔记本上这完全没有问题，笔记本玩的花进入 tty2 急救的情况比较多，回到 tty1 也很正常。后面在社区发帖 [A weird XFCE desktop disappear when return from tty2   2022-10-18](https://forum.endeavouros.com/t/a-weird-xfce-crash-when-return-from-tty2/32951)，得知 xfce 下 DE 是 tty7，而我熟知的 tty1=DE 在 kde 下有效。同时还有坛友建议使用 REISUB 替代强制关机。

注：不是DE决定了 tty 几对于桌面，我在 xfce 下安装 sddm 后 tty1 就是 桌面了，因此是 sddm、lightdm （Plasma、Xfce 分别默认使用）决定了桌面在哪一个 tty

## tty font

从 ctrl+alt+f2 切换到虚拟终端，发现中文无法正常显示，但是在桌面环境下的终端显示中文也都是正常的，是不是哪里配置错了呢？不过搜索了一番后发现 tty 本身不支持显示中文，需要借助别的东西比如说 Fbterm。不过实际使用中很少会用 tty,急救系统的话可以将环境变量设置为英文

想改的话在 [arch wiki](https://wiki.archlinux.org/title/Linux_console) 中有指南

[How to install a virtual console font on Arch? Asked 7 years,](https://unix.stackexchange.com/questions/206040/how-to-install-a-virtual-console-font-on-arch)

	The console doesn't use TTF fonts, like Inconsolata.
	You can use an application like Fbterm that can draw text with freetype2 to allow you to use TTF/OTF type fonts.


[To change font in Virtual Console .May 2010](https://www.linuxquestions.org/questions/linux-newbie-8/to-change-font-in-virtual-console-819721/)

    Read "setfont --help" output... You can just boot the SystemRescueCD and write the font it uses to HDD, and then use it from file.

# REISUB

当电脑变得没有反应时，可以优雅的重新启动，而不必按下硬件复位键或断电，这样做有破坏系统和丢失数据的可能。启用这个玩意也有安全风险，在 wiki 中说是“可以被用来转储CPU寄存器的内容，这在理论上可能会暴露敏感信息。”，这个风险对于一个不涉及保密任务的电脑还是可以接受的

```bash
# enable
echo 'kernel.sysrq=1' | sudo tee /etc/sysctl.d/99-reisub.conf 

# disable
sudo rm /etc/sysctl.d/99-reisub.conf
```

使用方法：当电脑卡死，tty2还有效，救急失败之后，考虑此办法重启。按住 Alt+PrtSc 保持住不动，随后逐个按下 R E I S U B。 

- [[Tip] Enable Magic SysRq Key (REISUB) ](https://forum.endeavouros.com/t/tip-enable-magic-sysrq-key-reisub/7576)： enable、diasble 方法简单明了

- [Kernel_(SysRq) Arch wiki](https://wiki.archlinux.org/title/Keyboard_shortcuts#Kernel_(SysRq))

# 内存与显存

free 查看内存的默认单位是字节，参数 `-m` 把单位切换为兆字节，下面显示内存总的11.8kM字节，也就是12G，通过 `glxinfo -B` 可以看到有 4G 显存，符合我把 16 G 内存中的 4G 通过 UMA 分给了核显。

```bash
[kearney@82dm ~]$ free -m
               total        used        free      shared  buff/cache   available
内存：      11815        2263        7932          64        1619        9225
交换：          0           0           0

[kearney@82dm ~]$ glxinfo -B
Memory info (GL_NVX_gpu_memory_info):
    Dedicated video memory: 4096 MB
    Total available memory: 10003 MB
    Currently available dedicated video memory: 3236 MB
```

# 最近文件

在 KDE 设置中，工作区-工作区行为-最近文件，保存历史记录只可以选择记录几个月或永久，无法关闭这个历史记录，可以手动清除历史记录，测试表面，手动清除历史记录功能不会删除 `~/.local/share/RecentDocuments/` 中的内容。在这个设置页面点击帮助后显示没有找到应用“kcontrol/kcm_recentFiles”对应的用户手册

针对系统级别的记录，不同桌面的方式不太一样，如 [Disabling GNOME’s recently-used file list, the better way. Alex. 2020](https://alexcabal.com/disabling-gnomes-recently-used-file-list-the-better-way)

应用级别的记录，只能在应用设置内关闭，如VLC。

暂未发现Ark如何关闭历史记录
