# Surface Pro 4 黑屏不开机维修记
- date: 2023-7-9
- lastmod: 2023-07-31

# 症状

机主说屏幕碎了换了一个据说是原装，但机主重装系统的时候，机器超级烫，卡死了机主强制关机，就再也打不开了。换的屏幕据机主描述疯狂抖动，在这种艰苦环境下，ta打完了刺客信条2。

机子充电时充电头灯量，机子键盘灯不亮，按键怎么按都没有反应。

怀疑是屏幕烧没了，也可能是主板烧坏了

# 屏幕拆解

热风枪的温度据 ifixit 中问答可知是 80～100 摄氏度，80最佳，太高了会把屏幕烧坏

我使用的薄片是JLC静电袋的塑封边和某防晒的塑料包装，手机用的真空吸盘我在这里没有使用到，感觉也可以用扑克牌。银行卡太厚了不行。

通过阅读[更换微软 Surface Pro 4 的屏幕 June 23, 2022](https://zh.ifixit.com/Guide/%E6%9B%B4%E6%8D%A2%E5%BE%AE%E8%BD%AF+Surface+Pro+4+%E7%9A%84%E5%B1%8F%E5%B9%95/60348)了解胶水布局、天线布局、排线位置来控制插入深度，在骆提供的热风枪的支持下，同时得到了辣辣的大力支持，慢慢吹慢慢拆，拆一会歇一会，拆了一天把屏幕几乎完美拆下来了。

不完美的地方在于屏幕内部左下角天线电缆软排线有一部分被我割开了，所幸的是没有伤到排线。其实吹完屏幕到分离屏幕更折磨，只能开一点点，还不知道哪里的胶没有清理掉，只能一点点地毯清楚，最后我处理的也就是左下角，我注意到其特殊的模样，经过反复试探和观察对比，加上其沾在主板这一边，因此认为其是胶水，实际上是软排线的一部分，这就是造就本次失误的地方。B站上也有很多拆屏幕耐心吹的视频，看了不少，实际上我花的时间老长了。

对于头一次拆 surface 的用户来说，拆成这样可以算的上优秀，100分算98分。给自己鼓掌

# 诊断

拆开散热和所有屏蔽罩后，目测无明显元器件炸裂。万用表测电池电压为零，把硬盘拆下来装到好机器上测试是可以正常启动的。

参照 `No Power, No Schematics` 后我的结果是苏菲Battery、Spare的二极管正常，缺少Charger二极管；保险丝很多都是可以让蜂鸣器导通的；开机3.3V是有的。因此问题就是不知道哪个地方短路了。试着把Spare换到Charger上看一下效果。用300W热风枪或黄花907S型刀头电烙铁都没能把Spare肖特基二极管取下来，电烙铁调到450摄氏度也不行，刀头相对苏菲来说太大了而且刀头最尖端位置导热不行，同时spare下边有元件不能用加热板。于是某宝购入SOD123封装耐压60V 2A的肖特基。

# 相关阅读

[Surface Pro 4自行维修记 藏安安 2021.3.9](https://zhuanlan.zhihu.com/p/354700246):这篇文章提到了ifixit都没有提到的FPC天线，我是拆完才读到这篇文章的，幸运的是我没有整坏这个摄像头附近的FPC天线。从ifixit的论坛里确定文章里说的确实是wifi天线
> 把FPC天线给划烂了 -> 某宝上的贴片天线

[Surface not turning on - burned diodes. Musti. 2018.4.21](https://zh.ifixit.com/Answers/View/478186/Surface+not+turning+on+-+burned+diodes):我拆开手上这台没发现二极管烧坏模样

[Surface Pro4详细拆机经验记录与分享 步子大了吧 2020-07-24](https://blog.csdn.net/wangpeng246300/article/details/107526132):换电池和固态

[Microsoft Surface Pro 4 – No Power, No Schematics, No Problem! March 18, 2020](https://www.aonemobiles.com.au/2020/03/microsoft-surface-pro-4-no-power-no-schematics-no-problem/):二极管、保险丝、3.3V。

[surface pro4不开机维修方法 2022-05-16 东风笔记本维修](https://www.bilibili.com/video/BV1CZ4y187Wc/):待机3.3V有，供电3.3V、5V、1.8V、8.7V有。视频证明可以不装电池开机，视频里的苏菲有Charger二极管，上电流烤机用手查找发烫元件


https://www.laptopschematic.com/microsoft-surface-pro-4-x911788-009-schematic-bios/：404

[Microsoft Surface Pro 4 BoardView, Schematics](https://www.repairlap.com/threads/microsoft-surface-pro-4-boardview-schematics.13588/):登录下载
