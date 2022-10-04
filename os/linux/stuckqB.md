# KDE 桌面不完全卡死的解决记录
- date: 2022-01-02
- lastmod: 2022-10-04

在手动查找资料的时候，打开 qbittorrent 准备下载的时候桌面卡住了，鼠标还能移动，但是鼠标点击均无反应。当时自己启动的程序只有 firefox、qb、dolphin。

之前我的处理办法都是 ctrl+alt+F2 切换到其它 tty 执行重启命令（`reboot`），在卡死的桌面没法唤起系统监视器。如果是 qb 单独卡死的话使用系统监视器终止 qb 进程的话大部分情况会有效。这一次的卡死流程比较简单，因此猜测是 qb 进程导致的。

在浏览了几下 bing 搜索 “Linux桌面卡死” 的解决方案大都是重启系统或者 sddm。我用 `ps aux` 查看进程得到了许多结果，没有办法上下滚动浏览全部信息，想起了管道过滤，于是通过 `ps aux | grep kearney`(其中 kearney 是我的用户名) 查看我启动的进程，虽然结果还是略微超出了一个屏幕所能显示的范围，好在 firefox、qb 都在目之所及。尝试关闭 firefox 进程，状况没有好转，关闭 qb 进程之后就恢复了（`kill -9 进程号`）。

## 仅 qb 卡住

这回是 qb 没法打开窗口进行操作，右键任务栏 qb 图标老半天才显示选项，但选择显示并不会打开窗口，此时我无法弹出我的硬盘扩展坞（通常只有 qb 才会使用到扩展坞的硬盘），pt站点也显示我没有在做种。日志（~/.local/share/qBittorrent/logs/qbittorrent.log）停留在15点时的监听ip,到现在19:30之间并没有记录。。。然后查看进程状态为 Sl，不是我想象中的僵尸进程，确认了同为 Sl 状态的 firefox 可以操作自如，这就很奇怪了

这期间我对电脑的操作有：发现自己v6无法通联就在系统里把自己踢下线（这一招奏效过两次，加上这一次是第三次）、反复在校园wifi和无网络wifi之切换。

最后的处理办法：在 systemmonitor 中对 qb 进程发生 TERM 终止信号，观察一会发现没反应，再发生 KILL 信号，qb顺利被终止，硬盘扩展坞被解除占有可以弹出。

```bash
$ ps aux | grep qbit
kearney   119723  1.4 13.8 109438040 2170716 ?   Sl   10月02  37:02 /usr/bin/qbittorrent

$ ps aux | grep firefox
kearney   266728 18.5  5.2 5072272 830572 ?      Sl   08:14 123:01 /usr/lib/firefox/firefox
```

[ Qbittorrent - Stalled (troubleshooting included) #1220 Closed war4peace 1 Jan 2014](https://github.com/qbittorrent/qBittorrent/issues/1220)：我关闭防火墙之后 qb 没有好转
>  WilliamArmstrong commented on 15 Apr 2017
	Had the same problem with torrents stalling. When I paused them they would Error.
	Found out that the entire process was being blocked by BitDefender's Ransomware protection. Disabled that and everything was working fine again.

[How to Fix the “qBittorrent Stalled” Error? – 10 Proven Ways [Resize Partition]By Ariel	| Follow | Last Updated June 14, 2022 ](https://www.partitionwizard.com/resizepartition/qbittorrent-stalled.html)

	1. Restart qBittorrent： In the Processes tab, right-click the qBittorrent client and select End task to completely close the process.
	2. Run qBittorrent as Administrator
	3. Change Some Settings in qBittorrent
	4. Remove and Re-add the Torrent
	5. Force Resume Downloading
	6. Check the Number of Available Seeders
	7. Check Your Hard Disk Space
	8. Check the Tracker URL
	9. Check the Firewall or Antivirus Interference
	10. Clean Reinstall the App