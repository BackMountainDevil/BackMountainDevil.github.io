# lightdm .service: Start request repeated too quickly. grub正常，不显示登录和桌面 Endeavour
- date: 2022-11-12
- lastmod: 2022-11-12

# 情况描述

启动界面 grub 正常显示，进入系统后会有代码提示启动了啥，但是最后停在了这个界面，没有转到桌面进行登录，可以切换到 tty2 进行操作。

## 前情提要

前一天晚上写完代码提示有更新，就浅浅的更新了一下，提示要重启，我就把代码push了重启就出现了上面的情况，有了 sddm 卡死的经历猜测是类似的问题，于是查看 lightdm 的状态，得到关键原因 - lightmdm 没有正常启动

```bash
lightdm .service: Start request repeated too quickly
lightdm .service: Failed with result 'exit-code'
Failed to start Light Display Manager
```

# 解决过程

当时也十点多了，要坐公交回宿舍，感觉把 [日志](http://0x0.st/o6ee.txt) 先存一波，后面发现日志太长不看（fail、error也有不少），以 `lightdm .service: Start request repeated too quickly` 为关键字在 bing、论坛上进行搜索，得到一些可能的答案：安装 lightdm-gtk-greeter 、从 lightdm 切换到 sddm、重装 lightdm、重装 mesa、xf86-video-intel、升级 lightdm、设置amdgpu（主机是intel，这条不试） 一次次尝试重启都不行，reisub 也一样，然后我想是不是某些设置的问题。

更新问题：确实进行了好几次更新，论坛里有个人说降级，但是其它坛友和我对降级这种方案不是很有好感

磁盘空间问题：根分区和 home 分区空间都有足够空余

启动项问题：上一次正常启动到这一次不正常启动，我给了 docker 加了开机自启，关掉无效；[kmcounter](https://gitee.com/anidea/kmcounter) 计数自启脚本关掉也无效；sshd 关闭也一样

设置问题：想了一下还捣鼓了 [tigervnc](https://wiki.archlinux.org/title/TigerVNC)，x0vncserver 两台电脑靠近一点速度还可以，但是我在不同大楼远程操控的延迟就很高，3～5s 的响应时间。然后把关于 vnc 的设置全部 rm,重启也无效，最后把 tigervnc 卸载就好了，盲猜是其依赖导致的问题

```
$ sudo pacman -Rs tigervnc
[2022-11-12T09:47:42+0800] [ALPM] removed fltk (1.3.8-1)
[2022-11-12T09:47:42+0800] [ALPM] removed xorg-xsetroot (1.1.3-1)
[2022-11-12T09:47:42+0800] [ALPM] removed tigervnc (1.12.0-3)
```
