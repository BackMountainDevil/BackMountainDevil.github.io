# EndeavourOS 初体验 REISUB
- date: 2022-09-23
- lastmod: 2022-10-19

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
    /etc/modprobe.d/nobeep.conf

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
