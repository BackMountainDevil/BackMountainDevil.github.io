# UMA Frame Buffer Size 核显显存与CSGO帧率
- date: 2022-11-01
- lastmod: 2022-11-03

因为经常捣鼓系统重装的引导修复，在 BIOS 设置里看到不认识的选项都会去搜索，UMA 这个单词见多了就知道啥意思了，就是给核显分配多少内存，这部分内存专供核显使用，分太少核显发挥不出最大能力，分太多把cpu和内存抢走了。

举个例子，[小新 Pro-13 2020(AMD平台：ARE版)](https://newsupport.lenovo.com.cn/driveList.html?fromsource=driveList&selname=%E5%B0%8F%E6%96%B0%20pro-13%202020(AMD%E5%B9%B3%E5%8F%B0%EF%BC%9AARE%E7%89%88))的16+512版本配合BIOS F0CN36WW+ 都可以给 r5 4600u 的核显分配512M～4G的显存大小。 [HP ProDesk 480 G6 小型立式商用电脑](https://support.hp.com/cn-zh/document/c06412581) 16+256+2T的i5-9500 核显 630 分配的默认显存是 64M，更新 BIOS 到 2022.10.31的最新版(02.14.01)之后的最大显存依旧是 512M，没有更大的选项了。两者更新BIOS后 hp 不会重置设置，也不会覆盖 grub；联想你自己反省反省为什么更新BIOS选项都重置了、还把引导也重置了，有对比才知道。

> 下面 g 和 G 混用，特此说明

# 性能提升

默认情况下 小新 Pro-13 2020、HP ProDesk 480 G6 分配核显分别是 512M、64M，csgo 平均帧率都在 28 左右。小新的显存调节到 2G 之后，帧率可以到 50 多，任务管理器可以看到 2 g 专用显存拉满了，调节到 4G，帧率上到 60,调节 windows 电源计划新增高性能计划(我的笔记本win11默认只有平衡模式)之后又提高了十多帧，4g显存占用到 2.9G，不过BIOS里面没有3G这个选项。 HP 的我也更新到最新BIOS驱动，UMA上限还是 512M，本想着更新之后会不会有 4G 选项，现在看来是没有，社区“专家”建议我加装显卡。

在办公、网络视频的情况下显存需不需要 4G，这个感觉上是不需要的，也就打大型游戏、剪辑渲染时需要。核显能不能打，目前我感觉是可以的，最好外加散热风扇。

![UMA 2G](https://img-blog.csdnimg.cn/4fc52a0902804e1eaad3a5a975943643.jpeg#pic_center)

![UMA 4G](https://img-blog.csdnimg.cn/2c4c8d402a3b4f14941503abb53fcccc.jpeg#pic_center)

# Q&A
1. 办公、网络视频需要多少显存呢？

    看网络视频 1080p x4（实际上我开 UMA 512M 也能看 1080p 唉

    ![bilibili 1080p x4](https://img-blog.csdnimg.cn/37a60a61ed3a44ecb55a19bc1f8452f8.jpeg#pic_center)
    ![bilibili 1080p x4 usage](https://img-blog.csdnimg.cn/9c37a4e23e904323b781d029eee46c17.jpeg#pic_center)

    关掉网络视频后空载

    ![empty](https://img-blog.csdnimg.cn/e0f2fac065b544e1bcea17de42cef2fb.jpeg#pic_center)
    ![empty performance](https://img-blog.csdnimg.cn/1f0269854999414d8060e252a8f39ab5.jpeg#pic_center)
    
    打开 word、ppt、excel 空白文档各x3.（仅供参考，实际上应该打开标书、论文等实际文档来衡量的）

    ![empty office x3](https://img-blog.csdnimg.cn/44c814e72a2247f5ad675b33f8a81894.jpeg#pic_center)
    ![empty office x3 performance](https://img-blog.csdnimg.cn/8420103f8c6845d39cbc97fc9afde110.jpeg#pic_center)

# 相关阅读

[Configuring UMA Frame Buffer Size on Desktop Systems with Integrated Graphics. UMA](https://www.amd.com/en/support/kb/faq/pa-280):BIOS - Advanced - UMA frame buffer size - save&reboot. 1/8-1/4

[11-21-2021 11:09 AM. andremachado. Adept Ryzen 7 5700g - How to adjust the UMA Frame Buffer from 512MB to 2048MB?](https://community.amd.com/t5/graphics/ryzen-7-5700g-how-to-adjust-the-uma-frame-buffer-from-512mb-to/td-p/498030):这个也是16g大小的内存，uma设置居然 64M～16g都可以选择，猜测是 BIOS 工程师忘记删除一些选项了

[教你0元解锁电脑隐藏性能：性能提升30%！2018-04-12](https://diy.zol.com.cn/684/6848892.html): 也是设置 UMA Frame Buffer Size 
> APU作为CPU和GPU的结合体，它的GPU部分与普通显卡没什么区别，但是APU的显卡有个缺点，就是它没有显存 ，它的末端直接连接上了北桥（见下图），所以APU想发挥性能必须占用内存。显存带宽高延迟也高，而内存的带宽相对要低一些。所以当APU和CPU进行共享内存时，势必要损失一些性能，所以将APU的显存设置大一些可以有效提升APU的显卡性能。

[ 请求BISO支持更大的UMA Frame Buffer Size 2022-10-31 ](https://h30471.www3.hp.com/t5/tai-shi-dian-nao/qing-qiuBISO-zhi-chi-geng-da-deUMA-Frame-Buffer-Size/m-p/1196431/highlight/false#M74334)：专家建议我加显卡

[Win10任务管理器中的"专用GPU内存"是怎么回事？“共享GPU内存”又是什么? 老狼 2018-05-08](https://zhuanlan.zhihu.com/p/36575387)