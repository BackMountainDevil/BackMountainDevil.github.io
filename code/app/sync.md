# 局域网传输文件与文件同步 ftp rsync rslsync
- date: 2022-10-13
- lastmod: 2022-10-26

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

## rslsync - Resilio Sync

1. 下载安装（手机端略）: yay rslsync
2. 生成配置: 

    ```bash
    mkdir ~/.config/rslsync/ -p
    touch ~/.config/rslsync/rslsync.conf
    rslsync --dump-sample-config > ~/.config/rslsync/rslsync.conf
    ```

3. 启动服务: systemctl start rslsync --user
4. 设置开机自动启动（非必需）: systemctl enable rslsync --user
5. 管理界面、设置用户名、密码: http://localhost:8888。
6. 设置同步文件夹，手机上扫码同步，另一个 pc 端输入密钥同步

同一局域网内，电脑（linux rslsync 2.7.3-1）和手机（ios Sync 2.6.12(451)）上同步速度平均 15M，内部网络p2p传输，速度瓶颈在于硬盘速度而不是网速。就是密钥有点长，手机扫码好说，另一台 pc 就难搞了。

实际测试在 xjtu 的校园网内同步良好，手机、笔记本、台式机同步一致，但是发现在使用 wps linux 版的时候发现它会同步打开文档时的临时文件，文件名通常是 ~.fileName.docx （后缀与源文件一致）。想起 git 可以忽略某些文件，搜索发现它也可以，编辑同步目录下的隐藏文件(.sync/IgnoreList)即可

我发现用 systemctl status rslsync 查看服务状态是 inactive (dead)，可是明明它正在运行，于是看了下不同之处在于参数 `--user`，加上就好了。关闭服务操作：`systemctl stop rslsync --user`，关闭开机自动启动：`systemctl disable rslsync --user`。有的时候不需要同步的时候我会关掉，因为有一次我的 endeavouros 就因为同步问题卡住了

- [Resilio_Sync - Arch wiki](https://wiki.archlinux.org/title/Resilio_Sync):AUR 是从官方下载，有墙。archlinuxCN有。前边千万不要直接 sudo start/enable，不然有权限问题
- [Resilio Sync | 全平台多设备文件同步/传输终极产品 2022-01-19 LAN DU](https://zhuanlan.zhihu.com/p/459403503):linux 源码和 aur 中校验一致，其它不晓得
    > 搬运了常见系统（win/mac/linux/Android）的官方安装包 更新时间2022-10-15
- [Resilio Sync 新手常见问题汇总 Tony 2019-04-21](https://www.tonyhead.com/book/export/html/4776)
    > 我不想下载某个目录，但我不是收费版用户，怎么办？  
    不用什么正版key，编辑.sync目录下的IgnoreList，加上你不想同步的文件夹名称就行了。
- [新奇士 – SyncKeys.com](https://www.synckeys.com/)：提供的安装包文件和 LAN DU 提供的校验一致。该网站还用 rslsync 来分享影片资源，属实没想到过

# 相关阅读

- [Q: best method for copying large # of files](https://forum.endeavouros.com/t/q-best-method-for-copying-large-of-files/32727):rsync

- [rsync 用法教程 阮一峰 2020年8月26日](https://www.ruanyifeng.com/blog/2020/08/rsync.html)

- [Linux rsync命令用法详解 站长严长生](http://c.biancheng.net/view/6121.html)
