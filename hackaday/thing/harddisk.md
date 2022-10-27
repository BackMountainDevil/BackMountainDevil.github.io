# 机械硬盘和固态硬盘 检测
- date: 2022-10-22
- lastmod: 2022-10-27

# 前言

钱韦德 的视频有解释机械硬盘买2T以下不划算，有些推荐；二进制的老王的视频呢则告诉人民群众可以通过特定软件修改型号序列号，重置SMART信息达到翻新硬盘、造假数据。

CMR（垂直盘） 与 SMR（叠瓦盘）：不买SMR，聪明人CMR,傻帽人SMR。PMR 包括CMR和SMR，不写明是CMR还是SMR，默认SMR不买，高手发法院传票。

# 官方查序列号查保修

- [希捷 质保查询](https://www.seagate.com/cn/zh/support/warranty-and-replacements/)
- [SeaTools 希捷](https://www.seagate.com/cn/zh/support/downloads/seatools):配备指南和 linux 版本

- [西数 质保查询](https://support-zh.wd.com/app/warrantystatusweb)
- [西数的Data Lifeguard Diagnostics](ttps://support.wdc.com/downloads.aspx?lang=cn)

为什么不写日立、东芝，因为不推荐。序列号能查到质保也不一定是正品。查不到的就一定不是了。

## 查外观

到 其它平台 的优质店铺找用户晒图进行对比

# 第三方读 SMART 和 读写速度

- [CrystalDiskInfo](https://crystalmark.info/en/download/#CrystalDiskInfo)：读取 SMART 获取通电次数、通电时间等信息。

- [CrystalDiskMark](https://crystalmark.info/en/software/crystaldiskmark/)：随机读写速度测试，得先格盘分区才能测

- [HD Tune pro](http://www.hdtune.com/download.html)： 错误扫描（扫坏道） 基准(磁盘性能)测试 磁盘健康诊断 文件基准测试 随机存取测试

- [Victoria](http://hdd.by/victoria/)

- AS SSD Benchmark（固态硬盘基准测试）

## linux

linux 上可以通过 GSmartControl/smartctl 查看 smart 信息，badblocks 查找坏道，KDiskMart 进行测试。Hard Disk Sentinel 在包管理没找到，smartctl 的包名是 smartmontools

# 案例 ST4000VX015

[硬盘检测实际结果 Kearney 2022-10-24](https://blog.csdn.net/weixin_43031092/article/details/127503708)
<details>
<summary>检测图</summary>

![在这里插入图片描述](https://img-blog.csdnimg.cn/5acfc34476c443f49804d234e1cd4741.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3afe0c7ecce3403783a228b4d25d69d0.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/cff5247b387843bd9098897f2f2a2b7f.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c75d908c27b24dd58a9fbd3ed974ec94.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/276b6c6ccea442cfaa7863fabeb72314.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/796b68a80b4440879c77f0a58fa72157.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/52d71064f56541b1b6c1a739a9f0d82b.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c5f438fd116242e98b51143c7e8a65ed.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/1fc50e8fd5614ace8c5891857445346c.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c927418eda17429c934d27ba62a992bc.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c3ef1b5b5214b82b0fc17f1b5647682.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9f6c60b2032c47fea55e5e98d97d258d.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2f8b4dd8e4334ea69565cc3b2c8bde45.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/49f5edb79b3443c998b1eaa6fd3eda19.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/86b1f1ccfa934da9b527620611b8cc9d.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e740c08d765c47c283ea3da8a6b262c2.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/717d73f81bbb49438703cecfb39c7943.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/839be3d0f53e49e5a55a5311d83bf317.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0bb7949922d249d0b8df523d5694f9f3.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b2a1cf35733748b386ebc89661b1c9cb.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8e777d90ab8c4ba4bf4a4142597fb9ba.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/67228ea8354b4f818fa53b8612fe6a89.jpeg#pic_center)
</details>

测速读信息都好说，出结果时间短，扫坏道扫了七个小时，全绿，海康客服凭借SN查询质保三年。刚开始我决定速度正常、无坏道，但是 victoria  的又红又黄的看的令人慌张， HDTunePro 里读取的也没有啥，就一个磁头飞行小时数数值爆表，结果问了客服才发现我看的是原始值，实际应该看当前值。和其它几个帖子对比了一下数据，安全下车

同时测了一块一年左右的 东芝 P300 HDWD120 2TB 7200rpm cmr 的盘，通电 181 次、通电时间 425 h、在保。而我笔记本的 512G SSD 写入 13T 读取 24T，bt 先锋。

[ST4000VX015 看看是否翻车](https://tieba.baidu.com/p/8101734998)
> 清0盘。得mhdd慢扫看。
> 而且需要提前随机数填充。

# 硬盘盒

- [硬盘盒、硬盘阵列柜选购指南 钱韦德 2022-02-11](https://www.bilibili.com/video/BV1MS4y1C781)：电源电流要够、波纹要稳、散热要足、盘要稳妥固定，参考wd的平放式硬盘盒
> 一只漏网的小鱼: 1.硬盘盒尽量买转头那种的，横放式，别买竖插式。  
> 2.如果只是消费级，12v2a的电源线也够用了，企业级需要3a的电源线，可以去明纬买。 明纬需要一并购买转接头！ 
> 3.硬盘盒没特殊需求，买单硬盘的就行，多硬盘的多少容易出点问题。

我是之前在网购平台买的 sata转usb3.0 带了个电源，用了一阵子感觉不太靠谱，速度不行、电源不说了，后面收了个绿联双盘位的竖放硬盘扩展坞。有人说是硬盘升天盒，因为电源供电不稳、硬盘大头朝下没有固定。这个立式的电源12V3A对单盘是足够的，双盘感觉就不太行，波纹我不懂，固定确实不稳固，钱韦德说塞入泡沫加固。

ST4000VX015 手册显示电源 Startup current (typical) 12V 1.8A，toshiba PC P300 手册写得简短稀巴烂，压根没有说启动电流。群友推荐的有 qpnp 的 tr-004、星际蜗牛4盘D款，考虑了价格我不推荐。

综上，长时间挂载硬盘还是需要稳固的设备，短期使用稍微固定下即可。

## 绿联 USB3.0双盘位硬盘盒 CM198

盒子上其实印的是 “USB3.0 硬盘扩展坞“。说明书可以找客服要电子版，双盘位的盒子外没有印型号，关闭休眠功能的官方页面挂了

[CCC工厂信息查询](http://webdata.cqccms.com.cn/webdata/query/CCCFacCode.do)找到电源上的 深圳市成威源科技有限公司 的CCC工厂编号A088637，没什么用，不存在 深圳市绿联科技有限公司 的 CCC工厂。电源型号 CW1203000 表示输出电压12V最大输出电流3A，主要是点源头上没有 3C 编号查不到对应证书的情况，直接搜工厂发现有6个证还有效，其它3个撤销2个注销。直接凭制造商名称查[CCC证书查询](http://webdata.cqccms.com.cn/webdata/query/CCCCerti.do)，输入 深圳市绿联科技有限公司 查到全都是已撤销，可能是绿联只是 监制商的缘故，那么问题又来了，深圳市成威源科技有限公司 查到的证都是关于电源的，那盒子本体的制造商是谁？3C编号是多少？根据名称模糊查询没查出个结果

# 相关阅读

- [4T机械硬盘买哪个？拼多多的硬盘靠谱吗？——4T硬盘购买攻略 2021-12-16 钱韦德](https://www.bilibili.com/video/BV19g411w7KX):到手测试查质保

- [新酷鹰4T怎么样？对于紫盘有什么优势？——希捷 ST4000VX016评测 2022-07-01 钱韦德 LemonTeaTT](https://www.bilibili.com/video/BV1RG411s7uV)

- [硬盘检测大起底——手把手教你用硬盘检测工具（真人兽电脑真实测试）2020-03-13 来杯黄桃酸奶](https://post.smzdm.com/p/az5eqq9r/)：表格图文并茂，重点是有官方链接

- [硬盘吧](https://tieba.baidu.com/f?kw=%E7%A1%AC%E7%9B%98&ie=utf-8)

- [机械硬盘吧](https://jump2.bdimg.com/f?kw=%E6%9C%BA%E6%A2%B0%E7%A1%AC%E7%9B%98&ie=utf-8&tp=0)

- [Victoria简明图文教程（机械硬盘检测工具）匿名用户 2022-07-07](https://post.smzdm.com/p/a5o5pdpl/)

- [图拉丁吧工具箱](http://www.tbtool.cn/)

- [你知道翻新的硬盘都是怎么来的吗？二进制的老王 2022-06-19 ](https://www.bilibili.com/video/BV1pa411s7Zn)

- [小白向的机械硬盘检测 2020-05-28中原洪先生](https://www.bilibili.com/read/cv6231874))
    > 硬盘使用时需要系统开启AHCI模式（https://baike.baidu.com/item/AHCI），以及硬盘分区4K对齐（https://baike.baidu.com/item/4K%E5%AF%B9%E9%BD%90）。使用AS SSDBenchmark可以检验AHCI和4K对齐情况  
    SMART清零   
    目前VIctoria软件可以对IBM、Hitachi、HGST硬盘初始化SMART，这就是为什么本次的HGST硬盘通电数据不太合理的原因。数据恢复和硬盘修复软件PC3000可以对大部分机械硬盘初始化SMART。还有其他一些操作可以对硬盘SMART进行初始化。所以购买机械硬盘请认准正规商家。

- [[多多]海康&希捷ST4000VX015（4T机械硬盘）dung一下  2022-09-05](https://www.bilibili.com/video/BV1Td4y1V757)

- [监控盘 购买指南 分享一些自己买盘时学习到的经验（VX000/VX015/40PURX/42HKVS）zmxyz 2022-09-18 ](https://post.smzdm.com/p/ammk580k/)

- [单碟2TB时代：希捷新款酷鹰测试 2022-02-15 海门牌梭鱼罐头](https://www.bilibili.com/read/cv15276451):ST4000VX015

- [CVE-2022-38392 Detail ](https://nvd.nist.gov/vuln/detail/CVE-2022-38392):歌曲 Rhythm Nation 导致某些 5400转的 PMR 硬盘发生共振导致损坏，比如 Seagate STDT4000100 763649053447

- [绿联USB3.0双盘位硬盘盒操作视频教程丨CM198 2020/06/04 绿联](https://www.lulian.cn/news/439-cn.html):没什么阅读价值，就知道个型号

- [绿联双盘位硬盘盒 绿联扩展坞升级固件 支持M1芯片 同心巷影老四 2022-05-12](https://www.bilibili.com/video/av769081254)：来路不明的固件
