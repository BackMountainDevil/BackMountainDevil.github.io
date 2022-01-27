# 用 debtap 将 deb 转换成 arch 的软件包格式，以 Raspberry Pi Imager 为例
- date: 2021-04-27
- lastmod: 2022-01-27

# 简介

用 debtap 将 deb 软件包转换成 arch 下的软件包格式

[Raspberry Pi Imager ](https://www.raspberrypi.org/software/) 只提供了 Ubuntu、Windows、MacOS 版本，使用 yay 下载 rpi-imager 下载多次也失败了。。。(但是直接去 github 却可以下载deb或者tar？？)

# 安装

[debtap](https://github.com/helixarch/debtap) 是 AUR 中的包，需要借助 yay 等工具(Manjaro 的 pamac 提供了对 AUR 的支持)

`yay -S debtap`

之后需要更新 pkgfile 和 debtap 数据库，此步至少需要运行一次。

`sudo debtap -u`

如过更新过慢，可看 [archlinux debtap -u 加速 _点墨 2021年3月30](https://blog.zzy-ac.top/2021/03/30/archlinux-debtap-u-jia-su/) 更换debap的镜像，然后再进行更新；我自己用的时候没修改镜像，用的速度还是可以的。

## 换源

打开 /bin/debtap, 将 http://ftp.debian.org/ 和 http://archive.ubuntu.com/ 都全部替换成 https://mirrors.ustc.edu.cn/ 。 这个时候再更新数据库的速度就飞起了。

# 转换过程

以后想把  deb 软件包转换成 arch 下的格式的话，按照下面的两行操作即可

```bash
# 转换
debtap -q <deb-package-name>
# 安装
sudo pacman -U <package-name>
```

## debtap 参数解释

-q 略过除了编辑元数据之外的所有问题，会提示选择文本编辑器编辑，我用 nano 啥也没编辑，直接保存退出

-Q 忽略所有问题，直接转换（不推荐）

-h 查看帮助

## 案例

以 [Raspberry Pi Imager ](https://www.raspberrypi.org/software/) 为例，转换之后的包名用 `ls` 命令查看一下

```bash
debtap -q imager_1.6.1_amd64.deb  # 将 deb 包转换成 pkg 包
sudo pacman -U rpi-imager-1.6.1-1-x86_64.pkg.tar.zst  # 从 pkg 包中安装，Manjaro 可以使用 pamac GUI 方式安装本地软件包
```

之后就能在菜单栏里找到 Imger 应用了，桌面快捷方式的话右键它 - Add to Desktop 即可

# 参考
- [将 DEB 软件包转换成 Arch Linux 软件包  作者： Sk 译者： LCTT amwps290| 2018-06-21 19:03](https://linux.cn/article-9769-1.html)：-q、-Q参数解释，缺乏实际案例
- [archlinux debtap -u 加速 _点墨 2021年3月30](https://blog.zzy-ac.top/2021/03/30/archlinux-debtap-u-jia-su/)
- [arch大法好之debtap 2020-07-03](https://tomtomyoung.gitee.io/post/arch%E5%A4%A7%E6%B3%95%E5%A5%BD%E4%B9%8Bdebtap/)：有例子，有镜像(失效了)