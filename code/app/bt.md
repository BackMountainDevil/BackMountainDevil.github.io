# P2P 技术/下载 与 qBitTorrent Transmission
- date: 2022-09-18
- lastmod: 2024-09-27

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

# 客户端
## qBitTorrent

bt 软件该有的基本功能都有。下载种子或磁力链接、做种、黑名单、web 端管理。支持精细化管理，可以选择单个种子内要下载的内容，可以部分下载，支持 tracker 添加，支持查看单个种子的用户并针对某雷用户关小黑屋。

[what is the meaning of lt20 when download windows version?](https://github.com/qbittorrent/qBittorrent/discussions/19653):lt20 是libtorrent version 2.0.x，它存在一些磁盘高速读写问题，这可能导致其它问题，因此默认采用稳定版的 libtorrent 1.2.x.

## Transmission

bt 软件该有的基本功能都有。下载种子或磁力链接、做种、黑名单、web 端管理。常用在 openwrt 中的轻量化 bt 客户端，功能相对简洁。支持一次添加多个种子。不支持一次添加多个磁链、不支持部分下载。

使用该软件的时候需要注意将配置保存到硬盘上，因为默认是保存在内容中，重启之后都会丢失，比如说做种列表重启就没了，需要重新添加种子然后校验。

# PeerBanHelper

反吸血的必要性：避免因 PCDN 吸血被运营商封号

qBittorrent-Enhanced-Edition 针对 qB 做了防迅雷吸血功能，分叉出来一个新的版本，不是所有 pt 都支持。而 [PeerBanHelper](https://github.com/PBH-BTN/PeerBanHelper) 的防止吸血功能更加强大，且是独立程序，兼任好几个 bt 客户端。

# 参考

[PeerBanHelper(PBH)+qbittorrent小白初启动教程 2024年07月14](https://www.bilibili.com/read/cv36091420/)

[关于 BitTorrent 和 PT 你需要知道的一切。 02/14/2020 by Ein Verne ](https://einverne.github.io/post/2020/02/everything-related-about-bittorrent-and-pt.html)

[PeerBanHelper：自动封禁吸血BT客户端 2024-04-12 vce1](https://blogs.vicsdf.com/article/109250)

[关于最近BT吸血事件的防范方法图文教程 2024年05月14日](https://www.bilibili.com/read/cv34508727/)

[简单记录一个近期恶意吸血BT客户端 2024年03月17日](https://www.bilibili.com/read/cv33250605/)
