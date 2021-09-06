 
```bash
kearney@debian:~$ uname -a
Linux debian 5.10.0-8-amd64 #1 SMP Debian 5.10.46-4 (2021-08-03) x86_64 GNU/Linux
kearney@debian:~$ pwd
/home/kearney
kearney@debian:~$ ls -ah
.     图片  .bash_history  .fonts.conf  .profile
..    文档  .bash_logout   .gtkrc-2.0   .Xauthority
公共  下载  .bashrc        .kde         .xsession-errors
模板  音乐  .cache         .local
视频  桌面  .config        .mozilla

```
看了下 `apt list --installed > installed.txt` 得到的结果是一共安装了 1908 的软件包、262款字体，Python 3.9.2、git version 2.30.2、nano 5.4-2, apt show 中显示 gcc 4:10.2.1-1、vim 2:8.2.2434-3，但这两在终端中却显示未找到命令。除了预装 Plasma 的一些附带软件外，还装好了LibreOffice、Firefox ESR、中文输入法、蓝牙、网络管理器（和 Ubuntu、Fedora、Deepin 安装完差不多，Arch 还需要自己整输入法等等）。

操作系统： Debian GNU/Linux 11
KDE Plasma 版本： 5.20.5
KDE 框架版本： 5.78.0
Qt 版本： 5.15.2
内核版本： 5.10.0-8-amd64
操作系统类型： 64-位
处理器： 12 × AMD Ryzen 5 4600U with Radeon Graphics
内存： 15.1 GiB 内存
图形处理器： AMD RENOIR

根据 [KDE - deb wiki](https://wiki.debian.org/zh_CN/KDE) 的记录表示 Debian KDE 默认安装的是 kde-full 包，但是通过 `apt list *kde* --installed` 查询的结果中并无 kde-full,有 kde-standard、kde-plasma-desktop,说明我看 wiki 有误解，从结果来看 Debian KDE 默认安装的应该是 kde-standard（依赖于 kde-plasma-desktop）。从 [kde-standard - deb pkg](https://packages.debian.org/bullseye/kde-standard) 中可以看出 kde-standard 比其依赖包 kde-plasma-desktop 增加了 15 个包，归档工具 ark、图像查看器 gwenview、截图 spectacle、文档浏览 okular 是自己之前在 Arch KDE 中用的比较多的，虽然多了也些很少使用的包（有一个原因是不知道用来干啥、咋用，更多是没需求）。其中自己不需要的有视频播放器 dragonplayer、音频播放器 juk、高级文本编辑器 kate、邮件 kmail、钱包管理 kwalletmanager、磁盘刻录 k3b、系统清理器 sweeper,播放器可以整个 vlc,国内的在线音乐可以用 listen1,文本编辑器 KDE 中还有的 Kwrite。

和在上次玩的 Arch KDE 少了些东西，也多了些东西，比如这次的没有预装 Qt，而在软件管理上多了一个  Apper（Discover 一言难尽）、在终端模拟器上多了一个 termit。

从 Apper 中卸载 k3b，当我尝试从 Apper 或者 Discover 中卸载 sweeper 或者 kmail 的时候，软件会将 task-kde-desktop 和 kde-standard 两个软件包一并删除。。。

# apt

```bash
$ sudo apt update

我们信任您已经从系统管理员那里了解了日常注意事项。
总结起来无外乎这三点：

    #1) 尊重别人的隐私。
    #2) 输入前要先考虑(后果和风险)。
    #3) 权力越大，责任越大。

[sudo] kearney 的密码：
对不起，请重试。
[sudo] kearney 的密码：
kearney 不在 sudoers 文件中。此事将被报告。
```

这个问题可以在设置-用户-帐号类型中解决，将“标准”改为“管理员”

```bash
$ sudo apt update
$ sudo apt upgrade
sudo apt remove task-kde-desktop kde-standard --purge
# emm 播放器还在。。
sudo apt remove dragonplayer juk kaddressbook kate khelpcenter kmail kwalletmanager plasma-wallpapers-addons sweeper --purge
sudo apt remove --purge konqueror kontrast korganizer goldendict
```

https://www.cnblogs.com/samgg/p/7501199.html

可以借助 Discover 下载 vlc、arduino, listen1 可以直接在 FF 浏览器插件中安装。 

## Discover
有些软件在 Discover、Apper 不好找，命令行安装方式也比较烦琐（如vs code），可以在 Discover 中搜索 discover,安装 Snap backend、Flatpak backend 了，Discover 会安装对于的软件包，之后就可以直接在 Discover中搜索 flathub、snap 里的包了。要是再来个 AUR 后端就好了。

https://flathub.org/apps/details/com.vscodium.codium

https://snapcraft.io/install/codium/debian

观察和搜索了下 appimage、flathub、snap 的几个跨发行版软件仓库，snap 打包完比较庞大，appimage 目前是最方便的

也不喜欢 snap 在跟目录下额外创建了一个文件目录，opt 不行吗

## DUR

https://mpr.hunterwittenborn.com/

# codium
https://vscodium.com/#install
https://github.com/VSCodium/vscodium/releases
介绍中三行代码由于防火墙的缘故网速不给力。。直接去 github 上下载 deb 和校验文件，appimage 也可以


# python
Debian 11 Bullseye 中已经除去了 py2，只有一个 py3，无 pip。[Python - deb wiki](https://wiki.debian.org/Python?action=show&redirect=DebianPython) 中也有对此的说明，需要注意的是没有建立动态链接 /usr/bin/python 到 py3，因此直接敲 python 是无效的。

下面安装 pip3 并配置镜像源，以后使用 pip3 之前要先建立一个虚拟环境，不然最后就是列出来一大堆。。。

```bash
kearney@debian:~$ python -V
bash: python：未找到命令
kearney@debian:~$ python3 -V
Python 3.9.2
kearney@debian:~$ pip -V
bash: pip：未找到命令
kearney@debian:~$ pip3 -V
bash: pip3：未找到命令
kearney@debian:~$ sudo apt install python3-pip
建议安装：
  python-setuptools-doc
下列【新】软件包将被安装：
  libexpat1-dev libjs-sphinxdoc libpython3-dev libpython3.9-dev python-pip-whl
  python3-dev python3-distutils python3-lib2to3 python3-pip python3-setuptools
  python3-wheel python3.9-dev zlib1g-dev
kearney@debian:~$ pip -V
pip 20.3.4 from /usr/lib/python3/dist-packages/pip (python 3.9)
kearney@debian:~$ pip list
Package             Version
------------------- --------------
Brlapi              0.8.2
certifi             2020.6.20
chardet             4.0.0
cupshelpers         1.0
dbus-python         1.2.16
distro-info         1.0
fuse-python         1.0.2
gpg                 1.14.0-unknown
httplib2            0.18.1
idna                2.10
louis               3.16.0
pip                 20.3.4
pycairo             1.16.2
pycups              2.0.1
pycurl              7.43.0.6
PyGObject           3.38.0
pylibacl            0.6.0
PyQt5               5.15.2
PyQt5-sip           12.8.1
PySimpleSOAP        1.16.2
pysmbc              1.0.23
python-apt          2.2.1
python-debian       0.1.39
python-debianbts    3.1.0
pyxattr             0.7.2
pyxdg               0.27
reportbug           7.10.3
requests            2.25.1
setuptools          52.0.0
six                 1.16.0
tornado             6.1
unattended-upgrades 0.1
urllib3             1.26.5
wheel               0.34.2
xdg                 5

$ pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
$ pip config set install.trusted-host https://pypi.tuna.tsinghua.edu.cn
# 在 Debian 上不装它用不了 venv 模块。。。
$ sudo apt install python3-venv
# 不需要安装 pip 也能在 venv 中使用 pip
# why deb、arch 这些发行版的 python 默认都不带 pip 呢？
$ sudo apt remove python3-pip
```

# git

git version 2.30.2
配置一下，但是配置文件 .gitconfig 直接放在用户跟目录下，以前觉得没啥，现在看你不能像 pip、fcitx5 一样把配置放在 .config 里吗
# rust

- []()
- []()
