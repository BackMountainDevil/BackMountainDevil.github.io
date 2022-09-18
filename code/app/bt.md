# P2P 技术/下载 与 qBitTorrent
- date: 2022-09-18
- lastmod: 2022-09-18

P2P 技术可以用来高速分享文件。优点是可以提高下载速度、节省服务器带宽、避免单点故障。BitTorrent 则是建立在该技术上的应用软件，称之为 BitTorrent 客户端。客户端有很多，具有图形界面的有 Transmission、qBittorrent、没有图形化界面只有控制台界面的有 aria2、Transmission CLI（更多请查阅[BitTorrent_clients](https://wiki.archlinux.org/title/List_of_applications/Internet#BitTorrent_clients)）

最开始接触 bt 是utorrent，后面 bt 被某雷搅浑水之后就接触 PT（Private Tracker），之后就一直在使用 qBitTorrent。

# Tracker

Tracker 是服务器上一个程序，假设张三想知道谁有 deepLearn.mkv 这个文件，那我就得询问 Tracker,然后 ta 告诉我谁有，有多少，然后张三再去找名单上的人要。

一个经常更新的 [ngosang/trackerslist ](https://github.com/ngosang/trackerslist)

活跃的 Tracker 越多，能获取到同一资源的可能性越高。

# 种子 磁力

种子文件 或者 磁力链接 就是资源文件的地址簿，靠这个去下载。资源的下载速度取决于 P2P 之间的资源可用性和网络状况。

# BT/磁力搜索

一些比较知名的 bt 搜索网站，如海盗湾、RuTracker、[rarbg](http://rarbggo.org)