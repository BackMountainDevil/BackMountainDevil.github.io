# 大部分网页无法连接，能 ping 不能 curl. 换系统不行 换网络就可以
- date: 2022-11-17
- lastmod: 2022-11-18

# 故障描述

大部分网页无法访问，一直加载空白，比如说 bing、bilibili、csdn 无法访问，百度翻译、arch wiki却可以正常访问。

# 尝试解决

更换网络：手机开热点，故障消失，说明网络可能是故障源头，但是其它同样连接校园网的设备却没有这个故障

更换系统：双系统切换到 win11 ，故障依旧。工位设备上也是 endevaourOS，但是没有这个故障，使用当初安装 ende 的安装u盘进入 livecd 模式，故障居然还存在。

更换 dns 服务器(`sudo nano /etc/resolv.conf`)，默认的是校园网内置的 dns，工位设备说明默认 dns 没问题，尝试使用别的 dns，可以正常解析域名，故障依旧

关闭防火墙(`systemctl stop firewalld`)：故障依旧

更换系统+重置网络设置+清空dns缓存：win11 下的 dns 使用的是校园网提供的 dns，重置网络设置(`netsh winsock reset`)重启后无效，然后清空dns缓存(`ipconfig/flushdns`)也无效

更换 mac address：参照 [MAC address spoofing](https://wiki.archlinux.org/title/MAC_address_spoofing#systemd-networkd)systemd-udevd 进行修改，生效后校园网立马发现踢下线，然后重新认证，故障依旧

```bash
$ cat /etc/resolv.conf 
nameserver 2400:3200::1
nameserver 223.5.5.5
nameserver 166.111.68.130

$ dig www.bing.com +short
www-www.bing.com.trafficmanager.net.
cn-bing-com.cn.a-0001.a-msedge.net.
a-0001.a-msedge.net.
204.79.197.200
13.107.21.200

$ ping 204.79.197.200
PING 204.79.197.200 (204.79.197.200) 56(84) 字节的数据。64 字节，来自 204.79.197.200: icmp_seq=1 ttl=108 时间=121 毫秒64 字节，来自 204.79.197.200: icmp_seq=2 ttl=108 时间=157 毫秒64 字节，来自 204.79.197.200: icmp_seq=3 ttl=108 时间=77.6 毫秒64 字节，来自 204.79.197.200: icmp_seq=4 ttl=108 时间=311 毫秒64 字节，来自 204.79.197.200: icmp_seq=5 ttl=108 时间=125 毫秒64 字节，来自 204.79.197.200: icmp_seq=6 ttl=108 时间=149 毫秒^C
--- 204.79.197.200 ping 统计 ---
已发送 6 个包， 已接收 6 个包, 0% packet loss, time 5003ms
rtt min/avg/max/mdev = 77.626/156.653/311.185/73.562 ms

$ curl 204.79.197.200
curl: (28) Failed to connect to 204.79.197.200 port 80 after 129391 ms: Couldn't connect to server
```

wireshark 抓包显示，ping 期间的ICMP是通的，TCP则是红色重传错误，firefox通过bing搜索引擎得到的也是红色的 TCP Retransmission

网关可 ping

```bash
# 查找网关，第一个 ip 即为网关地址
$ ip route
default via 10.180.0.1 dev wlan0 proto dhcp src 10.180.80.151 metric 20600 
10.180.0.0/16 dev wlan0 proto kernel scope link src 10.180.80.151 metric 600
$ ip route show
default via 10.180.0.1 dev wlan0 proto dhcp src 10.180.80.151 metric 20600 
10.180.0.0/16 dev wlan0 proto kernel scope link src 10.180.80.151 metric 600

$ ping 10.180.0.1
已发送 26 个包， 已接收 26 个包, 0% packet loss, time 25039ms
rtt min/avg/max/mdev = 4.842/17.159/103.995/21.685 ms
```

主机地址解析正常，curl 超时

```
$ getent hosts cn.bing.com
202.89.233.101  china.bing123.com
202.89.233.100  china.bing123.com

$ curl cn.bing.com
curl: (28) Failed to connect to cn.bing.com port 80 after 215538 ms: Couldn't connect to server

$ host address
Host address not found: 3(NXDOMAIN)

$ host cn.bing.com
cn.bing.com is an alias for cn-bing-com.cn.a-0001.a-msedge.net.
cn-bing-com.cn.a-0001.a-msedge.net is an alias for china.bing123.com.
china.bing123.com has address 202.89.233.100
china.bing123.com has address 202.89.233.101
```

traceroute 分析，百度、bilibili 有获取到中继路由，cn.bing 则一直停留在内网

```bash
$ traceroute www.bilibili.com
traceroute to www.bilibili.com (124.239.244.18), 30 hops max, 60 byte packets
 1  _gateway (10.180.0.1)  11.295 ms  11.208 ms *
 2  10.6.33.12 (10.6.33.12)  11.158 ms  11.114 ms  11.093 ms
 3  * * *
 4  10.6.32.98 (10.6.32.98)  14.282 ms  14.263 ms  14.243 ms
 5  * * *
 6  10.6.32.62 (10.6.32.62)  14.155 ms  18.589 ms  18.484 ms
 7  61.185.212.193 (61.185.212.193)  18.737 ms  18.744 ms  14.998 ms
 8  10.224.169.21 (10.224.169.21)  59.156 ms 10.224.169.17 (10.224.169.17)  59.054 ms 10.224.169.13 (10.224.169.13)  59.027 ms
 9  * 1.85.253.33 (1.85.253.33)  58.948 ms 117.36.240.185 (117.36.240.185)  58.911 ms
10  10.255.51.13 (10.255.51.13)  58.873 ms 202.97.105.105 (202.97.105.105)  58.838 ms 10.255.51.13 (10.255.51.13)  58.800 ms
11  124.239.224.234 (124.239.224.234)  59.125 ms 202.97.68.85 (202.97.68.85)  59.091 ms 124.239.224.242 (124.239.224.242)  27.204 ms
12  * 124.239.224.246 (124.239.224.246)  27.097 ms *
13  * 124.239.224.86 (124.239.224.86)  225.178 ms *
14  * * *
15  * * *
16  124.239.244.18 (124.239.244.18)  224.582 ms  224.556 ms *

$ traceroute www.baidu.com
traceroute to www.baidu.com (14.215.177.39), 30 hops max, 60 byte packets
 1  _gateway (10.180.0.1)  334.051 ms * *
 2  10.6.33.12 (10.6.33.12)  333.190 ms  333.168 ms  333.133 ms
 3  * * *
 4  10.6.32.98 (10.6.32.98)  333.038 ms  333.357 ms  332.982 ms
 5  * * *
 6  10.6.32.62 (10.6.32.62)  332.899 ms  254.634 ms  254.533 ms
 7  61.185.212.193 (61.185.212.193)  254.488 ms  254.451 ms  254.332 ms
 8  10.224.169.1 (10.224.169.1)  254.187 ms 10.224.169.9 (10.224.169.9)  254.053 ms *
 9  * 117.36.240.177 (117.36.240.177)  193.225 ms *
10  10.255.51.13 (10.255.51.13)  193.090 ms 202.97.13.253 (202.97.13.253)  193.072 ms *
11  113.96.4.126 (113.96.4.126)  193.029 ms 202.97.98.218 (202.97.98.218)  193.004 ms *
12  113.96.5.58 (113.96.5.58)  101.177 ms 121.14.14.162 (121.14.14.162)  101.076 ms 113.96.4.98 (113.96.4.98)  101.050 ms
13  14.29.121.206 (14.29.121.206)  101.023 ms 106.96.135.219.broad.fs.gd.dynamic.163data.com.cn (219.135.96.106)  100.982 ms  306.784 ms
14  14.215.32.94 (14.215.32.94)  306.678 ms * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *

$ traceroute cn.bing.com
traceroute to cn.bing.com (202.89.233.101), 30 hops max, 60 byte packets
 1  * * *
 2  10.6.33.12 (10.6.33.12)  15.328 ms  15.278 ms  15.247 ms
 3  * * *
 4  10.6.32.98 (10.6.32.98)  18.623 ms  18.540 ms  29.961 ms
 5  * * *
 6  10.6.32.62 (10.6.32.62)  29.853 ms  22.052 ms  21.945 ms
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *
$ dig cn.bing.com +short
cn-bing-com.cn.a-0001.a-msedge.net.
china.bing123.com.
202.89.233.100
202.89.233.101

$ ping 202.89.233.100
PING 202.89.233.100 (202.89.233.100) 56(84) 字节的数据。64 字节，来自 202.89.233.100: icmp_seq=1 ttl=111 时间=710 毫秒64 字节，来自 202.89.233.100: icmp_seq=2 ttl=111 时间=43.8 毫秒64 字节，来自 202.89.233.100: icmp_seq=3 ttl=111 时间=127 毫秒^C
--- 202.89.233.100 ping 统计 ---
已发送 3 个包， 已接收 3 个包, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 43.797/293.508/709.930/296.397 ms

$ traceroute 202.89.233.100
traceroute to 202.89.233.100 (202.89.233.100), 30 hops max, 60 byte packets
 1  _gateway (10.180.0.1)  8.873 ms * *
 2  10.6.33.12 (10.6.33.12)  8.684 ms  8.640 ms  8.616 ms
 3  * * *
 4  10.6.32.98 (10.6.32.98)  12.454 ms  12.428 ms  12.404 ms
 5  * * *
 6  10.6.32.62 (10.6.32.62)  12.310 ms  9.003 ms  8.899 ms
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *
```

Defender 没有报毒，Windows 网络诊断也没有发现问题，360断网急救箱没有发现任何问题，360木马查杀没报毒，win 下的ping、curl和linux结果一致，都能ping，curl cn.bing.com 就超时。Edge 针对访问bing超时显示的原因是 ERR_CONNECTION_TIMED_OUT

没有尝试360断网急救箱的强力修复，因为它上面写着的是重置网络，我猜和我前面尝试的 netsh 是同一个东西。最后顺手清理垃圾、检查驱动，发现一个比我用的还新的公版网卡驱动，就安装了，然后重启，去打疫苗，打疫苗的人真多，留观半小时后回来，重启默认进入 endevaour，居然好了？我原先的旧版驱动就算是旧也是电脑官网驱动里最新的了，一直没啥问题，总不可能网卡驱动旧了只能访问一小部分网址然后换一个网络又没有坏？本着探究精神重启进入win11,win11里边网络还是故障依旧，断网急救箱依旧是啥问题没看出来，cmd 里的结果没有变，无所谓啦，反正 win11 用的机会非常少。linux NB

# 相关阅读

[ 如何在Linux上刷新DNS缓存 2020-09-01 A5互联](https://www.cnblogs.com/a5idc/p/13594833.html)

[3 useful commands to Check Gateway in Linux. 2022-04-16. David Cao](https://sslhow.com/3-ways-to-check-gateway-in-linux):ip route, route -n, netstat -rn

[I can ping a website, but I cannot access the website using the IP number or the actual web address, help?  2013](https://answers.microsoft.com/en-us/ie/forum/all/i-can-ping-a-website-but-i-cannot-access-the/1f3475cc-9815-453b-9714-2657a891292e)：无后续，工程师6种方法：关闭代理、关闭防火墙、重置路由器、重置IE设置、重置TCP/IP、检查计算机病毒

[Can Ping websites but wont load. Rossco88 on Oct 17th, 2016](https://community.spiceworks.com/topic/1877883-can-ping-websites-but-wont-load):找相同的正常允许的路由器，导出设置进行恢复