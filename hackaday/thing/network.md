# 小型内部网络 软路由
- date: 2022-10-09
- lastmod: 2022-10-29

# 路由器

k2p k3 newifi 3 都有些年头了，还得检查有没有漏油然后更换，价格现在不见得有优势。要个带 USB 口主要是我有个硬盘扩展坞 bt 做种。然后也想尝试下下非官方固件，当然无线功能也得有，暂时不需要 wifi6（没有 wifi6 的设备和带宽）。ac、ax 分别对应 wifi5、wifi6。

- 支持双频 wifi 独立控制
- 支持 ipv6
- 带 usb3.0 接口
- 可刷非官方固件（去广告、挂硬盘）

<details>
<summary>相关阅读</summary>

[求推荐个带usb3.0接口的路由器?](https://www.zhihu.com/question/480640197)
> 小米路由器3G一代，非R3Gv2版，有线千兆，带Usb3.0. 小米路由器pro版，带一个usb3.0接口

[小米路由器的USB，一种低成本的NAS方案。数不识趣 2021-08-06 ](https://www.bilibili.com/video/BV1d64y1W7Z8)：小米路由器Pro USB3.0 默认关闭，打开时提示“USB3.0使用时会对2.4G Wi-Fi有些影响”

	小米路由器1代硬盘版（R1D）100￥
	全新小米路由器硬盘版（R2D）100￥
	小米路由器mini（R1CM）40￥ usb2.0 百兆网口，MT7620, 16MB SPI Flash, 128MB DDR2 RAM, 300+867Mbps。v1 可刷
	小米路由器3（R3）
	小米路由器3G（R3G）80￥	 MT7621A, 128M Flash(ROM), 256MB 内存(RAM), 300+867Mbps, 买v1，v2缩水的
	小米路由器Pro（R3P）120￥	MT7621A, 256M Flash, 512MB 内存, 800+1733Mbps
	小米路由器HD（R3D）400￥

[哪些路由器可刷老毛子固件（Padavan）？ 品牌型号/固件下载汇总2021年2月2日 作者 admin](https://www.vpsgongyi.net/archives/436.html)

[有什么好用的可以刷梅林or老毛子固件的路由器?](https://www.zhihu.com/question/327772696)

[联想新路由3 newifi 3简评，100块的简易矿渣究竟值不值得买？有态度的土豆 2019-10-14](https://page.om.qq.com/page/OSkdE4bILoEbTuyAS59Qq9uQ0)
> 联发科MT7621双核880Mhz，32M闪存空间和512M的超大内存。 加上全千兆网口和1200M无线规格（300M 2.4G+867M 5G）  四个全千兆LAN口加上一个USB3。比市面上一百元左右的百兆路由器还是要香不少的。不过建议拿它做主力无线的话就不要开启USB3.0 ，否则对2.4G信号有较大干扰。

[简易矿渣newifi 3（newifid2）路由器散热改造与固件安装匿名用户 2019-07-22](https://post.smzdm.com/p/and20692/)
> 年初全新还不到80的价格现在已涨到了110。 涨价的理由就是K2P的替代品，便宜又可以刷很多固件（恩山很多大神支持）。百元左右路由器的最佳选择简易矿渣newifi 3（newifid2）路由器散热改造与固件安装 PS.超出120就没啥性价比了。奸商可恶啊。
	因为newifi3路由器带usb3.0， 会对2.4g信号干扰。（搜不到2.4信号）因此需在pcb USB位置进行屏蔽，因为本人没有用到此接口故没有进行屏蔽，屏蔽方法就是覆盖锡纸

[一次意外后的折腾---矿渣newifi 3路由器折腾记（改散热、屏蔽并增加电容）2020-03-04 ](https://post.smzdm.com/p/apz377ox/)
> 某鱼花100块大洋. 酒精洗板、加装屏蔽、滤波电容一气呵成

[新3路由  轻度硬改篇，打造一个百元级高性价比家用路由 匿名用户 2020-03-01](https://post.smzdm.com/p/amm5464d/)
> 硬改部分：1、给电源部分加电容。2、给功放、cpu、usb3.0部分做屏蔽罩 。3、防止漏油将原装导热硅脂保鲜膜包裹后贴回去

[买华硕路由 花冤枉钱了吗？带你避开华硕路由器的坑 我是-桥 2021-07-25](https://www.bilibili.com/video/BV1Cf4y157bS):ac86u 300￥

[华硕路由器有个通病，过保后厂家不管，教你一招三分钟就可以修好！ 2022-03-03 猫猫无线](https://www.bilibili.com/video/BV1644y1T7MQ)：更换某烧坏芯片

[这台AC86U的故障和之前修的都不同，为什么AC86U这么多坏的？顺便说一下华硕路由器质量到底好不好。2022-08-07 猫猫无线 ](https://www.bilibili.com/video/BV1GF411P7Te)：没修好

[闲置路由再利用，MT7621路由变身便携科学路由器。1.8万 15 2022-02-12](https://www.bilibili.com/video/BV1y3411j7Q3)
> 联想新路由2 D1 MT7621A 32M Flash, 256MB 内存 usb3.0+usb2.0 50￥ 新3D2就一个usb3.0 120￥

[小米路由器 Pro HD 官方参数](https://www.mi.com/miwifihd/specs)

[这可能是2020年性价比最高的一款WIFI5路由器了——瑞斯达康MSG1500 2020-09-06](https://post.smzdm.com/p/aoozem39/):版本号X.00 黑色可刷，白色不可
> USB2.0, MTK7621, 128ROM+256RAM,300+866Mbps

[中兴E8820S路由器拆机及OpenWrt固件(含源代码) yumeimm 2021-8-2 ](https://www.right.com.cn/forum/thread-5203754-1-1.html):千兆网口+USB2.0 MT7621A 128MB Flash+256MB DDR3 RAM 2.4G MT7603E 5G MT7612E

[50元的有线千兆无线AC路由器 中兴E8820s个人向测评 2021-09-14 值得买数码频道](https://www.163.com/dy/article/GJSIKCRF0550VSCB.html)
> 2.4G没有外置放大，且2.4G和5G共用天线，所以一共只有2根天线  
> 板载的固件存储器是NAND的。NAND经过多次读写已经坏了，需要换掉。

[ [网络] 中兴E8822拆机，应该是全网第一个拆机 不二不三不四 2019-7-14](https://www.mydigit.cn/thread-55922-1-1.html):8820缩水版。千兆网口+USB2.0, MT7621A, 300+867Mbps
> MT7621A+MT7603+MT7612E,双核，闪存16M，DRAM DDR2 64M,我自己换成32M+128M了
</details>

## 固件/系统

- 引导：breed, pb-boot
- 系统固件：除官方固件外，还有openwrt、padavan（老毛子）、Merlin（梅林）、PandoraBox（潘多拉）

[路由器刷固件有什么用？](https://www.zhihu.com/question/408115206)
> Mariko真理: 普遍使用的路由器固件包括梅林固件、OpenWrt、LEDE、ROS、爱快等。用 MTK 芯片的刷 OpenWrt；采用博通芯片的刷梅林固件；软路由采用 x86 架构，可以刷 LEDE、ROS、爱快等固件

WikipediA:刷openwrt 主要多播，梯子 ，广告过滤，应用过滤，音乐解锁，KMS激活！解锁160！很多…老毛子没用

[OpenWrt——适用于路由器的Linux系统| 2013-07-17](https://linux.cn/article-1653-1.html)

# 交换机

路由器的LAN网口可能不够一个大户型使用，就需要使用交换机对LAN网口进行扩展了。通过在购物网站的检索，得知存在网口速率区别（百兆、千兆）、家用/工业等区别。

- [路由器和交换机的不同之处有哪些？](https://www.zhihu.com/question/20465477)
> 交换机能做的，路由都能做。交换机不能分割广播域，路由可以。路由还可以提供防火墙的功能。路由配置比交换机复杂。
>
> 路由器是快递分拣中心，交换机是区域快递员

- [装修日记 篇三：全屋网络布局（弱电箱，光猫，无线路由器，POE交换机，入墙式无线AP）2018-11-02红烛小火炉](https://post.smzdm.com/p/755224/):用交换机来扩展路由器的网口

# 网线

材质：单股纯铜（全铜）即可，不要无氧铜、铜包铝，不要扁线、超细线、多股线（每根由7股线芯绞合，某绿某泽不做人详情页一点都不详情）

双绞线减少内部干扰，六类线比超五类多了绝缘十字骨架将四组双绞线隔开，再高级就是一组双绞线包一层屏蔽膜，最后四组一起再包一层

我的需求在宿舍连接网口和路由器，一米左右的超五类单股跳线即可，没找到纯铜的，挑了个无氧铜全包。要是几个房间布线肯定买根长网线回来自己裁剪做水晶头，但是自己做水晶头怎么确保无氧铜不暴露和检测问题。。。这个问题暂时不用考虑，没有房

<details>
<summary>相关阅读</summary>

[价值18万的网线测评！网线里的智商税，买啥网线不吃亏？五类线 超五类 六类线 超六类 福禄克 水晶头 网络面板 跳线 永久链路 信道 DSX-5000 2021-05-20 科技宅小明](https://www.bilibili.com/video/BV14f4y1Y7mL)
	> 1.租的是泰和信测的设备，他们家服务范围涵盖北成沈香，有兴趣的话可以联系这个人13439036507弱电工程百事通
	2.家用六类线足够了，90m内跑满万兆
	3.超五类55m内万兆，五类15m内万兆
	4.福禄克测试大唐电信六类线质量较好
	5.超六以上网线需要接地，效果会更好，家用环境不接地影响也不大
	6.六类网线打水晶头教程05:55
	12.福禄克测试报告9份（顺序：泛达一测、二测、康普、罗格朗、日线、山泽、秋叶原、爱普华顿、大唐电信）链接: https://pan.baidu.com/s/1xWEciySPT8_PHg5r1lz2FQ  密码: m6jk

[你根本不懂网线！双绞线 超五类 Cat5e 六类 Cat6 七类 Cat7 八类 Cat8.1 看这一期就够了哦～ 2021-02-10 科技宅小明](https://www.bilibili.com/video/BV1p5411E7Nj)

[1元1米的网线无敌？百元1米的网线垃圾？我们用价值20W的设备，实测告诉你差距！ 2022-07-11 韭菜实验室](https://www.bilibili.com/video/BV1mB4y1i7d9):电竞网线是智商税

[家里网速跑不满还经常断流？关于光猫和网线的秘密 2022-07-30 晨钟酱Official](https://www.bilibili.com/video/BV1D94y1D7gA)：两个水晶头的线序要一样，八线都要接。办理宽带提速注意换新的光猫。
> 不建议使用光猫的 wifi 上网。光猫改桥接，将拨号功能下放给路由器。一般家庭超五类、六类足矣

[多股网线和单股网线有什么区别? 2020年01月02日  编辑：深圳连讯 ](http://www.faxytech.com/archives/solid-stranded.html)
	顾名思义，绞合的四对电缆是指四对电缆的八根导线中的每根都由多根“多股”电线相互缠绕构成的电缆，而实心电缆仅由一根实心铜线构成 每个导体。
	绞合电缆和实心电缆之间的关键物理区别是灵活性。与刚性实心导体相比，绞合电缆具有更大的柔性，并且可以承受更多的弯曲，如果弯曲太多次，则可能会失效。导体的股数越多，柔韧性就越大。股数也会影响成本–构成导线的股数越多，成本越高。为了降低成本，双绞线电缆使用足够高的绞线数来保持适当的柔性，但绞合线电缆的绞合线数量却很少，以至于造成巨大的价格差异。换句话说，这是成本与灵活性之间的谨慎平衡。
	电缆的结构也会影响端接。插孔，配线架和连接块上的IDC用于固定电缆。实心电缆的各个导体将保持其形状并正确放置在IDC中，而绞合的导体通常会折断并随着时间的流逝而松动。由于实心线的表面积比多股绞线的表面积小，因此也被认为更坚固并且不易腐蚀。

	另一个主要区别是电气性能。实心电缆是更好的电导体，可在更宽的频率范围内提供出色的稳定电特性，与绞合电缆相比，对高频效应的敏感性更低，并且直流电阻更低。这就是TIA标准允许绞合结构的衰减增加20％的原因。
</details>

# 网络测速

- [中国科学技术大学测速网站](https://test.ustc.edu.cn/)

- [使用iPerf进行网络吞吐量测试 2019-10-28 胡齐](https://cloud.tencent.com/developer/article/1528627) 

	```bash
	# TCP客户端和服务器
	iperf -s # 服务端
	iperf -c ip # 客户端

	# UDP客户端和服务器
	iperf -s -u
	perf -c ip -u

	# 双向测试
	iperf -c ip -d
	```

<details>
<summary>学校校园网测试</summary>

一台笔记本(RTL8822CE)连接校园网 wifi,一台台式机(RTL8111/8168/8411)网线连接校园网，双方防火墙测试时关闭。ip 信息已经剔除。可以 ping 通，但是服务端开启 web 服务端，客户端无法访问。TCP 双向可以有 25Mbps，UDP 只有 1Mbps？台式机从学校网信中心下载正版软件的速度有 50Mbps

```bash
$ ping ip
PING ip (ip) 56(84) 字节的数据。
64 字节，来自 ip: icmp_seq=1 ttl=63 时间=6.64 毫秒
64 字节，来自 ip: icmp_seq=2 ttl=63 时间=1.69 毫秒
64 字节，来自 ip: icmp_seq=3 ttl=63 时间=10.3 毫秒
64 字节，来自 ip: icmp_seq=4 ttl=63 时间=8.63 毫秒
64 字节，来自 ip: icmp_seq=5 ttl=63 时间=8.42 毫秒
64 字节，来自 ip: icmp_seq=6 ttl=63 时间=4.37 毫秒
64 字节，来自 ip: icmp_seq=7 ttl=63 时间=2.95 毫秒
64 字节，来自 ip: icmp_seq=8 ttl=63 时间=3.74 毫秒
--- ip ping 统计 ---
已发送 8 个包， 已接收 8 个包, 0% packet loss, time 7012ms
rtt min/avg/max/mdev = 1.686/5.846/10.348/2.905 ms

$ curl ip:8000
curl: (7) Failed to connect to ip port 8000 after 2 ms: 没有到主机的路由

$ wget ip:8000
--2022-10-13 14:46:42--  http://ip:8000/
正在连接 ip:8000... 失败：没有到主机的路由。

$ iperf -c ip
------------------------------------------------------------
Client connecting to ip, TCP port 5001
TCP window size: 16.0 KByte (default)
------------------------------------------------------------
[  1] local ip2 port 35130 connected with ip port 5001 (icwnd/mss/irtt=14/1448/1808)
[ ID] Interval       Transfer     Bandwidth
[  1] 0.0000-11.2220 sec  35.3 MBytes  26.4 Mbits/sec

$ iperf -c ip -d
------------------------------------------------------------
Server listening on TCP port 5001
TCP window size:  128 KByte (default)
------------------------------------------------------------
------------------------------------------------------------
Client connecting to ip, TCP port 5001
TCP window size: 16.0 KByte (default)
------------------------------------------------------------
[  1] local ip2 port 44440 connected with ip port 5001 (icwnd/mss/irtt=14/1448/5753)
[  2] local ip2 port 5001 connected with ip port 49212
[ ID] Interval       Transfer     Bandwidth
[  2] 0.0000-10.8547 sec  30.9 MBytes  23.9 Mbits/sec
[  1] 0.0000-10.9577 sec  33.6 MBytes  25.7 Mbits/sec

$ iperf -c ip -u
------------------------------------------------------------
Client connecting to ip, UDP port 5001
Sending 1470 byte datagrams, IPG target: 11215.21 us (kalman adjust)
UDP buffer size:  208 KByte (default)
------------------------------------------------------------
[  1] local ip2 port 60674 connected with ip port 5001
[ ID] Interval       Transfer     Bandwidth
[  1] 0.0000-10.0156 sec  1.25 MBytes  1.05 Mbits/sec
[  1] Sent 896 datagrams
[  1] Server Report:
[ ID] Interval       Transfer     Bandwidth        Jitter   Lost/Total Datagrams
[  1] 0.0000-10.0169 sec  1.20 MBytes  1.01 Mbits/sec   3.381 ms 38/895 (4.2%)

$ iperf -c ip -u -d
------------------------------------------------------------
Server listening on UDP port 5001
UDP buffer size:  208 KByte (default)
------------------------------------------------------------
------------------------------------------------------------
Client connecting to ip, UDP port 5001
Sending 1470 byte datagrams, IPG target: 11215.21 us (kalman adjust)
UDP buffer size:  208 KByte (default)
------------------------------------------------------------
[  2] local ip2 port 5001 connected with ip port 57740
[  1] local ip2 port 37823 connected with ip port 5001
[ ID] Interval       Transfer     Bandwidth
[  1] 0.0000-10.0153 sec  1.25 MBytes  1.05 Mbits/sec
[  1] Sent 896 datagrams
[ ID] Interval       Transfer     Bandwidth        Jitter   Lost/Total Datagrams
[  2] 0.0000-10.0182 sec  1.24 MBytes  1.04 Mbits/sec   0.810 ms 6/894 (0.67%)
[  1] Server Report:
[ ID] Interval       Transfer     Bandwidth        Jitter   Lost/Total Datagrams
[  1] 0.0000-10.0200 sec  1.25 MBytes  1.05 Mbits/sec   3.613 ms 0/895 (0%)
```
</details>
