# 显示器选购参数解析-笔记本核显外接显示器的带宽支持4K60Hz探究
- date: 2021-02-27
- lastmod: 2023-1-11

# 介绍
>大部分厂商并不会在产品宣传页上标出详细的屏幕素质，一般只给出了分辨率和屏幕面板技术（IPS, PLS 等）
>[【进阶】当评价一款产品时，我们关注的是什么？（一）我们需要什么样的显示器（屏幕）  Navis Li](https://zhuanlan.zhihu.com/p/22411193)

R5 4600U核显，只有全功能TypeC接口的笔记本，买个外接显示器

# 主要参数
## 面板

>- IPS面板是市面上使用最多的面板类型，其优点是可视角度大，色彩真实，动态画质优秀，满足了大众对显示器的基本需求。但是漏光也是IPS面板一直被人诟病的缺陷。
>- TN面板的虽然生产成本便宜，但是色彩表现力差，可视角度小，原本是接近淘汰的类型了。但随着这几年的电竞屏的崛起，TN屏的响应速度快，辐射水平低的特点尤其适合游戏玩家。所以近几年TN屏也慢慢回归视野，出现在各种游戏显示屏上。
>- VA面板最明显的特点就是对比度高，色彩还原准确，但可视角度比IPS略小一点，不过没有漏光情况。
>[2021年高性价比电脑显示器推荐（入门款和高端款皆有）  小道黑科技](https://zhuanlan.zhihu.com/p/174838223)

## 色域

>色域，一般指的是显示器所能呈现的色彩范围，色彩范围越广，我们能看到的色彩也就越丰富。目前我们购买显示器看到最多的色域主要有sRGB色域，NTSC色域。由于NTSC色域是最广的色域。
>100%sRGB色域≈72%NTSC色域
>[2021年高性价比电脑显示器推荐（入门款和高端款皆有）  小道黑科技](https://zhuanlan.zhihu.com/p/174838223)

>对于 PC, Mac 或是 iOS Android 来说，最为适合描述屏幕色域的就是 sRGB 和 Adobe RGB：
>- sRGB 是由 Microsoft 在 1997 年主导的标准，由于 Windows 强大的用户基础，所以从 PC, Mac 再到相机、扫描仪、打印机、投影仪都支持 sRGB, 几乎互联网上的所有的内容也都是以 sRGB 为准的，大约能覆盖 35％ 的 CIE（人眼可见颜色）。
>- Adobe RGB 全称应为 Adobe RGB 1998, 由 Adobe 定制，在 sRGB 的基础上增加了 CMYK 色域，相对于 sRGB 主要改善了青绿色的覆盖，被更广泛的应用于印刷行业。大约能覆盖 50％ 的 CIE（人眼可见颜色）。
>-  NTSC 由美国国家电视标准委员会在 1953 年订制，目的是为了给当时刚出现不久的 CRT 彩色电视定制一套标准，由于实在是太过于古老（Apple DOS 3.1 诞生于 1978 年， MS-DOS 诞生于 1980 年）早已不适用于现代显示器，更最重要的是对于 PC（广义的）和移动设备来说，几乎没有内容创作者是以 NTSC 为工作空间的，它保留下来最多的用途还是用于比较其他的色彩空间。
>[【进阶】当评价一款产品时，我们关注的是什么？（一）我们需要什么样的显示器（屏幕）  Navis Li](https://zhuanlan.zhihu.com/p/22411193)
>综上：NTSC 实际是一个根本没有人遵守的色域标准，然而还有很多不专业的评测机构和厂商喜欢采用 NTSC 来标明自己的色域覆盖，即便消费者通过这个数值并不能直观的了解这块屏幕到底表现怎么样。
>为什么对于大部分人来说色域覆盖高过 sRGB 并不是什么好事
>[【进阶】当评价一款产品时，我们关注的是什么？（一）我们需要什么样的显示器（屏幕）  Navis Li](https://zhuanlan.zhihu.com/p/22411193)
>对于普通用户来说，他们看到的图片，视频等几乎不会包含不常见的颜色，所以 80% sRGB 或者是 100% sRGB 其实是没有太大区别的，依旧可以体验到和 100% sRGB 屏幕非常近似的效果。

## 尺寸
通常说多少寸都是指显示器面板的对角线宽度。根据数量关系`1 英寸 = 2.54 厘米`以及勾股定理就能解出其变长。

|  inch| cm |
|--|--|
| 21 | 46 x 26 |
| 24 | 53 x 30 |
| 27 |  62 x 37|

带鱼屏21:9看电影更舒服，分屏看代码也舒服，竖过来之后就能一屏几十行（27寸竖过来太长了要抬头低头有点累。。），但是浏览网页两边有白边;16：9看电影就会有两个大大的黑边。。。

### 视距
## 分辨率

>1k 1920X1080
>2k 2560X1440
>4k 3840x2160
>25寸以下的显示器推荐使用1K的分辨率，27寸推荐2K，32寸推荐4K
>[如何选择显示器？](https://www.zhihu.com/question/35668312/answer/516910001)
> PPI = sqrt（H^2 + V^2） / Inch
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225191117233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

>像素密度，在 iPhone 上的 Retina 屏幕推出之后广为人知，与屏幕不同距离时对像素密度的需求也不同，手机一般为 300 PPI, 笔记本电脑上至少需要 150 PPI 才能避免直接看到像素点，而 200 PPI 以上才能算得上优秀。
>[【进阶】当评价一款产品时，我们关注的是什么？（一）我们需要什么样的显示器（屏幕）  Navis Li](https://zhuanlan.zhihu.com/p/22411193)

在同样的屏幕尺寸下，分辨率越高，PPI（    Pixels Per Inch ） 也就越高.也就是说谈分辨率不谈尺寸就是扯淡。。同时分辨率越高对电脑的性能要求也是越高的。
PPI 要是不会算的话我写了个代码，输入三个参数就行[用javascript计算PPI](https://blog.csdn.net/weixin_43031092/article/details/114119395)

## 刷新率  - Hz

FPS（频率） == 每秒钟更新画面的次数，频率越高，画面越流畅。  
众所周知，上世纪的电影基本都是25帧，在这个帧率下制片成本（胶卷）不会太高，人眼看起来也没有明显的视觉停顿。现在大多数视频下载都是主推60帧，至于再高的帧率那就是游戏信仰了。。。  
非游戏人群60Hz足矣。

## 频闪-闪屏

屏幕调光方式（DC/PWM/PWM + DC 混合调光）

## Sync
>G-Sync, G-Sync是显卡厂商nVIDIA提出的同步技术,通过改变VBLANK以及Burst Mode来让显示器刷新率可以动态变化,从原理上解决画面撕裂和卡顿问题提升画面品质,在后续的更新中nVIDIA还会导入HDR(HDR G-Sync).相关原理部分因为其资料保密程度很高这里就不详细介绍了,使用G-Sync必须为nVIDIA自家显卡搭配专门定制的显示芯片,因为相关授权费用一般G-Sync显示器会比一般显示器价格跟高.FreeSync以及Adaptive Sync,FreeSync是另外一家显卡大厂AMD(以前的ATI)提出的同步技术,实现原理与G-Sync类似,但因不需要特定芯片相比G-Sync成本略低,但是也仅支持AMD自家显卡,目前最新的版本为FreeSync 2,相比原先版本加入了HDR的支持.Adaptive Sync据说基本与FreeSync一致,AMD为推广其技术将其修改为Adaptive Sync并由VESA接受成为DP标准的一部分.
作者：什么值得买
链接：https://www.zhihu.com/question/35668312/answer/392123996

# 显卡- 核显

[AMD Ryzen™ 5 4600U](https://www.amd.com/en/products/apu/amd-ryzen-5-4600u)自带的核显
>显卡规格
显卡频率   1500 MHz
显卡型号   AMD Radeon™ Graphics
显卡核心数量   6
>主要特性:  
>Display Port 是
HDMI™   是

上面说明了支持DP接口，4K60p有的一拼？？ 看了B站大都是4800整4k60p.。可恶  

最后查的帖子逐渐躲起来，见参考最底部，最后表示这是可以玩耍4k60Hz的东西

# 一些显示器

>1k 1920X1080
>2k 2560X1440
>4k 3840x2160


| Name | 分辨率+帧率（Hz） | 尺寸（inch） | PPI | 接口 | 面板 | 调光方式 | 亮度（cd/m2） | 价格/元 |
|--|--|--|--|--|--|--|--|--|
| Redmi 1A（RMMNT238NF） | 1k/60 | 23.8 | 92.55 | HDMI、VGA | IPS | 无可视频闪 | 250 | 0.7k |
| Skyworth | 1k/75 | 23.8 | 92.55 | HDMI、VGA | IPS | DC | 200 | 0.7k |
| Philips 245E1S| 2K/75 | 23.8 | 123.41 | DP、HDMI、VGA | IPS | DC | 250 | 1.4k |
| AOC I2790PC| 2K/75 | 27 | 108.78 | HDMI,TYPE-C | AH-IPS |  | 250 | 1.1k |
| AOC | 4K/60 | 27 | 163.17 | HDMI,TYPE-C | IPS |  | 250 | 1k |

要求：至少有DP接口，27寸竖起来太大（桌面不够高也shu不起来）。不过之前用的27寸看电影是真的舒服。。
typeC接口普遍较少且价格较贵。。。于是找23.8inch 2K的基本都在1.2k左右，也有价格比较低的sane（还带typec.。）、koi、熊猫，但是不在JD自营里，且看店铺的营业执照，大都是近两年。。还是选择了JD自营的 Skyworth的23.8inch 2K/75,历史价格查了一波，购入。

# 参考

[2021年高性价比电脑显示器推荐（入门款和高端款皆有）小道黑科技](https://zhuanlan.zhihu.com/p/174838223)  

[【进阶】当评价一款产品时，我们关注的是什么？（一）我们需要什么样的显示器（屏幕）  Navis Li](https://zhuanlan.zhihu.com/p/22411193)
>可视角度，左右高于120° 上下高于 90 °
亮度，在周围光线水平低于 150 lux下屏幕亮度不能低于 80 cd/m2, 在周围光线水平 2000~ 2500 lux下屏幕亮度需高于 300 cd/m2。
偏色，平均 ΔE 低于 3
像素密度，至少需要 150 PPI
色域覆盖， 85% sRGB 以上没有意义

[关灯以后经常还是忍不住玩手机，是不是真的有很大危害啊？  丁当](https://zhuanlan.zhihu.com/p/20100643)  

[如何选择显示器？](https://www.zhihu.com/question/35668312/answer/516910001)

[UFO Test 屏幕调光方式测试](https://www.testufo.com/blurtrail)：VSYNC is not available on the Linux platform.

[2020 显示器选购指南 千元篇 | 先看评测](https://www.bilibili.com/video/BV1Wf4y1Q7d9?from=search&seid=11876184217601533571)
>1. 舒适的可视距离，1080p下24及其以上都不太舒适，2k的话24～27都ok;推荐是24inch买2k,27inch看钱;
>2. 亮度与对比度，标称300nit的实测200.。。除非使用信号发生器（非正常人），原因是HDMI接口电视也在用，只有少部分显示器（还不知道是哪些）重写了驱动弥补这个bug,所以结论是买个带**DP接口**的显示器。哈哈哈我的是typec接口，没有HDMI.。VGA上古时代产物就别提了。。
>3. 原厂背光和组装背光难以区分，厂商一般都会在背光和支架上做偷工减料（提高性价比）了。
>4. 低蓝光模式是**智商税**，大部分是降低RGB通道的Blue,并不是真正移动了蓝光最伤眼的波段，只是降低蓝色通道的亮度。。。然后发黄，所有显示屏（包括上古电视）都能做到。当然也有极少数确实有移动蓝光通道，但是劣币驱逐量币，在国内是混不下去的。
>5. 可视角度都是178,水分比马里纳亚海沟还深，亮度对比度下降的太多了。

[我的集成显卡支持2K显示器吗?  ZOL问答](https://ask.zol.com.cn/x/9362925.html)：垃圾回答

[有用核显接4k显示器的朋友吗   2018-12-6 12:29](https://bbs.a9vg.com/thread-5403046-1-1.html):i7-8650u+8g，核显是uhd620
>hdmi没有2.0的话 4k只有30HZ 很卡顿
>intel集显的CPU占用率非常高，HTPC最好还是上amd apu

[什么样的显卡能支持 4K 分辨率输出？ zhihu](https://www.zhihu.com/question/21834948)
>nfs king
>显卡支持最大分辨率的瓶颈不在于驱动，也不在于GPU性能，而是显存容量、RAMDAC以及输出接口
>
>Belleve
>HDMI 1.4 standard only has bandwidth for 4K at 30hz, HDMI 2.0 will add support for 4K at 60Hz
>DisplayPort can support 4K @ 60Hz using Multi-Stream Transport(MST).

[听说集成显卡带不动4K电脑显示器，有这么一回事吗？  2019-8-14](https://forum.xitek.com/thread-1846757-2-1-1.html)
>关键看是否支持4K 60HZ刷新率。30Hz的话移动鼠标都不连贯
>主要看主板的显示接口，cpu从四代酷睿开始都支持4K 60Hz了。AMD APU 2200G 2400G或以后的cpu都支持。
AMD的主板HDMI接口版本由cpu决定，2200G 2400G都支持4K 60Hz。
>
>luckcat
>Intel核心显卡，全系都不支持HDMI 2.0，所以一定要买带DP接口的主板，才能正常支持4K@60HZ。
说集成显卡显示慢，主要是因为，集成显卡会占用系统内存和总线带宽，所以会造成系统整体响应变慢，换一块独立显卡，会好很多。但是，如果只是上网、看电影，影响不大。
再一个就是，上了4K显示器以后，要换一个好点的鼠标，高DPI的，比如罗技8000DPI的就够用了。

intel的主板有DP口的都支持，HDMI口不一定，是否支持可以去主板厂家网站看详细说明

[谈谈集成显卡双2K/4K输出的那点事情  ryu2003   2016-10-19](https://blog.csdn.net/ryu2003/article/details/52859036)

[AMD Ryzen™ 5 4600U](https://www.amd.com/en/products/apu/amd-ryzen-5-4600u)

[不止37W的锐龙5 联想小新Pro13 2020 锐龙版评测 中关村在线  20-06-18](https://baijiahao.baidu.com/s?id=1669831142468510129&wfr=spider&for=pc)
>16:10/2.5K/100% sRGB/DC调光
>《英雄联盟》的测试，测试画质为1080P、最高画质，平均100帧
>集成显卡Vega 6
>Memory Size 512MB	Bandwith 51.2GB/s
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226161452448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

[小新pro13的4k输出实测和入手体验  2020-01-08 1白白_  ](https://www.bilibili.com/video/av82559056/)：用扩展wu转出的HDMI 4k下卡顿，无法辨别是核显因素还是HDMI因素
>摇摆无极限
>连接4K显示器，要买一根TYPE-C转DP的直连线才能显示4K60hz，不能用转接器。 因为TYPE-c传输标准要求，线路在全部用作视频传输能满速到32Gbps，有数据传输时候要占用掉一半，只能达到10Gbps。而4K60HZ要求的传输速率是11Gbp。

[AMD再战！——联想小新Pro13 2020锐龙版评测  墨鱼](https://zhuanlan.zhihu.com/p/149652214):R7 4800U
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226162218882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

[小新Pro13 2020 4800U锐龙版 TypeC转DP 外接2K显示器闪屏的问题(2)](https://www.bilibili.com/video/BV1Qg4y1v75k?from=search&seid=6280844254531178293)
>我是海倍思的type-c转HDMI2.0，显示器为4k60hz，完美支持，那根线49京东买的。[OK]，笔记本就是4800u版本，顺滑美滋滋
>typec转DP，接小米34寸2K 144曲面屏完美使用，不过我的是4800U

[YUV420 破解“真假”HDMI2.0之谜 2015/7/27](http://www.icpcw.com/Information/Tech/News/3257/325786_all.htm)
>一般来说，视频信号的规范能否实现，主要取决于带宽。以最普通的3840×2160分辨率为例，色深我们采用常见的8bit，那么RGB三原色就是24bit，要达到60Hz的刷新，我们需要的带宽就是3840×2160×24×60=11.94Gbps。考虑到HDMI信号传输的机制以及8bit/10bit的编码方式，通常实际效率是理论效率的80%，也就是说正常情况下，我们要输出4K@60Hz的带宽为11.94Gbps/0.8=14.925Gbps。HDMI1.4的最大带宽为10.2Gbps，HDMI2.0的最大带宽为18Gbps，所以理论上HDMI1.4是无法输出4K@60Hz的，只有HDMI2.0才可以。

[超能课堂(257)：4K144Hz到底需要多少带宽？   2020-12-31](https://www.expreview.com/77346.html)
>总带宽 = 每秒显示的总像素点 × 单个像素点的数据量 + 其它开销
>那么以华硕的PG27UQ为例，这款显示器在144Hz刷新率8bit色深模式下，需要的带宽为例：3840×2160分辨率、144Hz刷新率、3个子像素，8bit色深模式下需要的带宽（每秒数据量）为3840×2160×144×3×8≈28.66Gbit/s，在10bit模式下需要的带宽为3840×2160×144×3×10≈35.83Gbit/s。

[4K144Hz需要传输多少数据，教你如何正确的计算显示器的带宽需求？  2020-09-09  硬件茶谈](https://www.bilibili.com/video/BV1xy4y1y7FJ)：逐行扫描需要一些像素来同步和消隐，取决与显示器的cvt技术标准

[4k 60需要多大带宽？](https://www.chiphell.com/thread-2235805-1-1.html)
>c口转hdmi只有1.4，直接走dp，就算是1.2也够4k60
>直连DP吧，DP1.2也是够4K60HZ的，转HDMI，考验的是扩展坞，C口本来就是直通DP信号的，我的雷电口用一个绿联的扩展库也是HDMI 4K30HZ,我的雷电口的DP是1.4的

[用javascript计算PPI](https://blog.csdn.net/weixin_43031092/article/details/114119395)

[小新pro13如何实现外接2K 144hz的屏幕？](https://www.zhihu.com/question/365623508)
>小新13Pro支持两个全功能Type-C（DP 1.2协议）
>有些产品虚标，只有4k30却声称4k60.。

[小新pro13的type-c接口支持4k输出吗？](https://club.lenovo.com.cn/thread-5664306-1-1.html)
>看了联想官方手册，– USB-C 转 DP：3840 x 2160 像素，60 Hz，但是带宽只能以最高 5 Gbps 的 USB 3.1 Gen 1 速度传输数据，不知是否够用。是否屏幕有延迟

[我买的联想小新有两个typeC接口，有什么用？](https://www.zhihu.com/question/374882746)

[小新pro13 锐龙版外接4k显示器的问题](https://club.lenovo.com.cn/thread-6002222-1-1.html?highlight=%E5%B0%8F%E6%96%B0Pro13%2B4k)：可以没问题

[小新pro13 锐龙标压R5-3550H 支持外接4K显示器能达到60HZ吗？](https://club.lenovo.com.cn/thread-5688486-1-1.html?highlight=%E5%B0%8F%E6%96%B0Pro13%2B4k)

[Lenovo 小新Pro-13 2019、Lenovo 小新Pro-13S 2019、 Lenovo 小新Pro-13 2020、Lenovo 小新Pro-13S 2020用户指南(81XD 81XB 82DN 82DM 82EK)   2020/9/23 11:04:29](https://support.lenovo.com.cn/lenovo/wsi/lenovo2014/htmls/detail_20190926151344959.html?from=handbook)
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226223143736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226222951701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70#pic_center)
[usb3.2 gen1和gen2的区别](https://product.pconline.com.cn/itbk/top/1345/13451804.html)
>USB3.2 Gen1是USB3.1 Gen1的改称、USB3.2 Gen2是USB3.1 Gen2的改称，两者的区别和USB3.1 Gen1与USB3.1 Gen2相同。

其实看到这里我就有一个疑问了，4k60Hz的传输速率完全超过usb3.2gen1的5Gb/s.。可是大家都能耍上4k60Hz.。一定是哪里有问题，我找出了原装盒子里的说明书。。就是gen1啊。。可实际就能跑4k60啊。。莫非gen2没有诞生？？我继续查

[西数发布USB 3.2 gen 2×2通道外置硬盘 游戏党福音    2019-08-26 11:44:19   [  中关村在线 原创  ]   作者：姜凯译   |  责编：李诺 ](https://nb.zol.com.cn/725/7252949_all.html)

[【全新联想ThinkPad USB Type-C扩展坞Gen2】可65w供电 三屏可4K@60Hz   2021-1-16 16:28:15 Bulldozer](http://city.ibeike.ustb.edu.cn/thread-556319-1-1.html)
>用了个师弟的联想小新 amd版本（R7-3750H 小新Pro13），完美使用，供电正常，电源键呼吸灯都正常，usb口子测了速度了，可以达到gen2 10Gbps。
>https://support.lenovo.com/us/en/solutions/acc500106-thinkpad-usb-c-dock-gen-2-overview-and-service-parts
>https://download.lenovo.com/consumer/options/thinkpad_usb-c_dock_gen2_ug_zh-cn_201903.pdf

这下子我才明白不是全功能typeC的gen1 or gen2,而是下行usb设备的gen1/2牵制了传输速度。。

[全网最详细易懂的G-sync Freesync 垂直同步工作原理科普 2020-07-16 16:25:18 硬件茶谈](https://www.bilibili.com/video/BV1FK4y1x7bk):G-sync硬件加持，free sync emm阿猫阿狗都会，两者实际上目前来说没啥用处。。

