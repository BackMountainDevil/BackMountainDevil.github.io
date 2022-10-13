# 局域网传输文件与文件同步 ftp rsync
- date: 2022-10-13
- lastmod: 2022-10-13

手机、电脑之间，几个小文件完全可以用微信传输助手，再大一点（4GB）的电影文件我就会采用局域网传输文件了，比如 Landrop,但是他不能传输文件夹。当我想传几十个文件的时候，这个时候微信、Landrod、就会显的力不从心。这个时候就想起了 FTP、Samba、HTTP。

## HTTP

HTTP 是超文本传输协议，也可以用来传输文件，os 实验课程给板机传文件用的就是 tomcat 搭 web 服务器，然后 wget 下载文件。这个也是不能传输文件夹，得打包后传输。python3 提供了一个简单快速的方式搭建 web 服务端 `python -m http.server`

## FTP

有一个 60G 的游戏文件夹需要在两台台式机上传输，本科的时候我们采取的方法是移动硬盘，FTP 解决了移动硬盘同时只能传给一个人的烦恼。FTP 也支持拷贝文件夹。但是搭建比较麻烦一些。

## Samba

[这](https://www.samba.org) 经常用在 windows 和 linux 之间共享文件夹（网络共享）。但是我日常并不使用 windows.

## bt

这个协议开发的原因就是文件分享，还支持多对多加速下载(p2p)。

## rsync

这个玩意比较高级的地方在于它可以增量备份，也就是相当于 onedrive

- [rsync manpage](https://download.samba.org/pub/rsync/rsync.1): sync 参数 源文件地址 要保存的地方
> sync [OPTION...] [USER@]HOST::SRC... [DEST]

`-a` 表示递归传输并保持文档属性，`-v` 表示在传输过程中输出相关信息（传输的文件清单，总传输大小）, `-z` 表示在传输过程中进行压缩以减少传输时间。

同一个文件夹，参数 -z 对比。总体积、接收体积一样、发送的大小不一样，带 z 之后发送的大小会因为压缩变小，那么问题来了，大文件多文件夹的压缩解压时间会不会抵消了压缩后体积小的优势呢？下面的只有 308 个小文件共 4.7MB。关于文件体积在 manpage 有解释。

```bash
$ rsync -av /home/userName/Documents/BackMountainDevil.github.io/ /tmp/bmd
sent 4,970,697 bytes  received 4,917 bytes  3,317,076.00 bytes/sec
total size is 4,950,824  speedup is 1.00

$ rsync -avz /home/userName/Documents/BackMountainDevil.github.io/ /tmp/bmd
sent 3,936,673 bytes  received 4,917 bytes  2,627,726.67 bytes/sec
total size is 4,950,824  speedup is 1.26

$ rsync -avz useName@ip:/run/media/useName/D/SteamLibrary /run/media/userName/DATADRIVE1/SteamLibrary   # 拷贝 steam 测试
sent 59,111 bytes  received 17,325,904,667 bytes  2,931,387.15 bytes/sec
total size is 33,179,015,022  speedup is 1.91
```

steam 拷贝测试中，该进程的 cpu 占有不到 1%。2,931,387.15 bytes/sec = 2862 Kbps = 2Mbps。这个速度不对劲吧

# 相关阅读

- [Q: best method for copying large # of files](https://forum.endeavouros.com/t/q-best-method-for-copying-large-of-files/32727):rsync

- [rsync 用法教程 阮一峰 2020年8月26日](https://www.ruanyifeng.com/blog/2020/08/rsync.html)

- [Linux rsync命令用法详解 站长严长生](http://c.biancheng.net/view/6121.html)

- [Resilio Sync | 全平台多设备文件同步/传输终极产品 2022-01-19 LAN DU](https://zhuanlan.zhihu.com/p/459403503)
