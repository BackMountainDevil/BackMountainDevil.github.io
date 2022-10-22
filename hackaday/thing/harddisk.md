# 机械硬盘和固态硬盘 检测
- date: 2022-10-22
- lastmod: 2022-10-22

# 前言

钱韦德 的视频有解释机械硬盘买2T以下不划算，有些推荐；二进制的老王的视频呢则告诉人民群众可以通过特定软件修改型号序列号，重置SMART信息达到翻新硬盘、造假数据。

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

- [CrystalDiskMark](https://crystalmark.info/en/software/crystaldiskmark/)：  随机读写速度测试

- [HD Tune pro](http://www.hdtune.com/download.html)： 错误扫描（扫坏道） 基准(磁盘性能)测试 磁盘健康诊断 文件基准测试 随机存取测试

http://hdd.by/victoria/
- AS SSD Benchmark（固态硬盘基准测试）

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