manjaro-kde-21.1.1-minimal-210827-linux54.iso
lts 版本安装一直卡在 boot 之后的终端界面，到达 reach graphic interface 就不动了

manjaro-kde-21.1.1-minimal-210827-linux513.iso
# grub2
sudo nano /etc/default/grub
# 0 -> 10
# -1 -> 无穷
GRUB_TIMEOUT=0

sudo grub-mkconfig -o /boot/grub/grub.cfg
# lan

Manjaro主目录下文件夹由中文改为英文
thepoy
操作系统： Manjaro Linux
KDE Plasma 版本： 5.22.4
KDE 框架版本： 5.85.0
Qt 版本： 5.15.2
内核版本： 5.13.12-1-MANJARO (64-位)
图形平台： X11
处理器： 12 × AMD Ryzen 5 4600U with Radeon Graphics
内存： 15.0 GiB 内存
图形处理器： AMD RENOIR


2020.01.31 22:24:2
https://www.jianshu.com/p/2f05a5583609

sudo pacman -S xdg-user-dirs-gtk
export LANG=en_US
xdg-user-dirs-gtk-update


(process:3133): Gtk-WARNING **: 09:14:18.294: Locale not supported by C library.
        Using the fallback 'C' locale.
Moving DESKTOP directory from 桌面 to Desktop
Moving DOWNLOAD directory from 下载 to Downloads
Moving TEMPLATES directory from 模板 to Templates
Moving PUBLICSHARE directory from 公共 to Public
Moving DOCUMENTS directory from 文档 to Documents
Moving MUSIC directory from 音乐 to Music
Moving PICTURES directory from 图片 to Pictures
Moving VIDEOS directory from 视频 to Videos


sudo pacman -Rs xdg-user-dirs-gtk

https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E8%BE%93%E5%85%A5%E6%B3%95%E6%A8%A1%E5%9D%97
https://www.cnblogs.com/aaronbin/p/14670569.html

中文輸入法，不得不说 deian 默认配置好了中文输入法，man 则没有
https://www.cnblogs.com/fatalord/p/13850072.html

sudo pacman -S fcitx5-im fcitx5-chinese-addons

中文 wiki 里是这么做的
nano ~/.pam_environment

GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx
INPUT_METHOD  DEFAULT=fcitx
SDL_IM_MODULE DEFAULT=fcitx

英文 wiki 里是这么做的，自己试验了一下，两个都可以

sudo nano /etc/environment

GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx

INPUT_METHOD  DEFAULT=fcitx
SDL_IM_MODULE DEFAULT=fcitx

區域設置 輸入法 中州韻 fcitx5-rime
繁体设置为中文要修改文件，无法调整页大小
https://blog.csdn.net/chougu3652/article/details/100656237
https://www.cnblogs.com/fatshen/p/14961367.html
https://zhuanlan.zhihu.com/p/91129641

https://wiki.debian.org/InputMethodBuster
https://wiki.debian.org/zh_CN/I18n/Fcitx5
Debian 使用 im-config 来自动配置输入法需要的环境变量。注意一定要安装这个软件包。

pamac 具有Alpm、AUR、Flatpak和Snap支持的软件包管理器
這下子可以在同一個搜索框框裏直接搜索多個庫中的 pkg 了。有 AUR 了還要啥 alatpak 和 Flatpak和Snap

# lantern

https://aur.archlinux.org/packages/lantern-bin/
https://aur.archlinux.org/packages/lantern-beta/

```bash
    /usr/lib  lantern                                                                                 1 ✘
Running installation script...
/usr/lib/lantern/lantern-binary: 成功
/home/kearney/.lantern/bin/lantern: error while loading shared libraries: libpcap.so.0.8: cannot open shared object file: No such file or directory

# 糟糕的解决办法，不过目前就只好先这样了
cd /usr/lib
sudo ln -s libpcap.so.1 libpcap.so.0.8
lantern
```

# arduino
Error downloading https://downloads.arduino.cc/packages/package_index.json

/home/kearney/.arduino15/logs/application.log

ERROR c.a.c.p.ContributionInstaller:304 [Timer-0] 下载 https://downloads.arduino.cc/packages/package_index.json 时出错
java.lang.Exception: 下载 https://downloads.arduino.cc/packages/package_index.json 时出错
	at cc.arduino.contributions.DownloadableContributionsDownloader.download(DownloadableContributionsDownloader.java:149) ~[arduino-core.jar:?]
	at cc.arduino.contributions.DownloadableContributionsDownloader.downloadIndexAndSignature(DownloadableContributionsDownloader.java:165) ~[arduino-core.jar:?]
	at cc.arduino.contributions.packages.ContributionInstaller.updateIndex(ContributionInstaller.java:302) [arduino-core.jar:?]
	at cc.arduino.contributions.ContributionsSelfCheck.updateContributionIndex(ContributionsSelfCheck.java:215) [pde.jar:?]
	at cc.arduino.contributions.ContributionsSelfCheck.run(ContributionsSelfCheck.java:75) [pde.jar:?]
	at java.util.TimerThread.mainLoop(Timer.java:555) [?:1.8.0_292]
	at java.util.TimerThread.run(Timer.java:505) [?:1.8.0_292]
Caused by: java.io.IOException: Received invalid http status code from server: 403
	at cc.arduino.utils.network.FileDownloader.openConnectionAndFillTheFile(FileDownloader.java:239) ~[arduino-core.jar:?]
	at cc.arduino.utils.network.FileDownloader.downloadFile(FileDownloader.java:182) ~[arduino-core.jar:?]
	at cc.arduino.utils.network.FileDownloader.download(FileDownloader.java:129) ~[arduino-core.jar:?]
	at cc.arduino.contributions.DownloadableContributionsDownloader.download(DownloadableContributionsDownloader.java:147) ~[arduino-core.jar:?]

Caused by: java.io.IOException: Received invalid http status code from server: 403
这一行指出了错误原因，服务器接受但拒绝了请求。。。我 firefox 都能正常访问。。。esp两个开发板都能正确获取到，也就是说原因出在了 arduinocc的服务器上？不对吧，浏览器都可以。。在昨晚安装的多系统的 Debian 中的 arduino 日志中发现了同样的错误。。。说明不是 Manjaro 系统的问题，但是浏览器却可以，说明不是 ip 封锁啊。。看着窗外的夕阳还有插座上的数据线，想起来了或许是网络问题，从移动宽带切换切换到电信 4G 网络，果然这个问题就没有了，成功下载官方开发板的 pkg.json，不过诞生了不能下载 8266 的错误，说明每个网络都有各自的问题。。。切换回宽带之后啥事也没有了。

总结一下，错误 `Error downloading https://downloads.arduino.cc/packages/package_index.json` 尽管相同，各自的原因却不一定相同， arduino cc 和 cn 里有一些相同的错误，看了下原因不同我就没有摘录了，还有的是外链失效也不记录了。

- [](https://blog.csdn.net/xutianruo/article/details/86591189?depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)：删除 arduino15 目录下的 *.tmp 文件，如 package_index.json.tmp、package_esp8266com_index.json.tmp
- [ Web Editor Error 403 Forbidden #5846 ](https://github.com/arduino/Arduino/issues/5846):原因是还不支持 Chromebook，后续更新会支持
- [ Board Manager JSON (http status code from server: 403) #8494 ](https://github.com/arduino/Arduino/issues/8494)：相同的报错。由于某些原因服务器开启了验证码导致的错误，已经关闭验证码了，可以正常使用了。（所以就不知道我的错误咋解决了）
- [2016. Error downloading http://downloads.arduino.cc/packages/package_index.json #5188 ](https://github.com/arduino/Arduino/issues/5188)：错误原因和我不同，但结果相同。它改了 IP 协议就好。不过里面 agdl  说了可以尝试清空  .Arduino15/staging/packages，不过实验了一下无效
    > Caused by: java.net.SocketException: Address family not supported by protocol family: connect

# shell

KDE 标题显示 zsh.。编辑配置方案显示命令是 /bin/zsh。但是根据环境变量显示是bash.。但是根据用户目录下的 history 文件显示 KDE 里敲的都记录在 .zhistory，而 vscodium（ 默认 linux 终端 bash） 敲的都记录在 .bash_history.

echo $SHELL

cat /etc/shells


切换bash： chsh -s /bin/bash
切换zsh： chsh -s /bin/zsh
    在终端app的系统偏好设置里手动设置。

在配置文件方面：

    bash读取的配置文件：~/.bash_profile文件
    zsh读取的配置文件：~/.zshrc文件

https://www.jianshu.com/p/a891af6f87e0


# boot slow

启动时间分析

```bash
$ systemd-analyze
Startup finished in 2.615s (firmware) + 1.263s (loader) + 2.822s (kernel) + 1.274s (userspace) = 7.975s 
graphical.target reached after 793ms in userspace
```

systemctl disable --now lvm2-monitor.service
systemctl mask lvm2-monitor.service

https://forum.manjaro.org/t/manjaro-booting-is-very-slow-40sec/32489/4

# grub

grub-mkconfig -o /boot/grub/grub.cfg

# 关机显示 Backlight Brightness

[AMD 安装 Manjaro KDE 驱动安装后续及BackLight:ACPI故障解决. grsharp. 2020-04-24](https://blog.csdn.net/grsharp/article/details/105735792)
> boot启动显示错误信息Failed to start Load/Save Screen Backlight Brightness of backlight:acpi_video0，虽然不影响正常体验（其实系统使用了systemd-backlight@backlight:amdgpu_b10来补充了)所以让报错消失的办法直接屏蔽该服务：
sudo systemctl mask systemd-backlight@backlight:acpi_video0


- []()
- []()
- []()
- []()
- []()
- []()
- []()
