# 从自行车尾灯的平行线到鲍威尔透镜 - 一字直线发生器
- date: 2022-01-21
- lastmod: 2022-01-21

某天想去奥森夜骑，第一次去发现那边灯光不太好，随准备了手电和车尾灯。LED 尾灯除了几种闪烁模式之外还有两道红色平行线，刚好可以把车收在两道线之间。今天收拾东西的时候发现了这个尾灯，遂决定探究下这个平行线是怎么整出来的。

从 bing 搜索中查到了一个帖子[请教下给位可否有方案让打在墙上的激光点加什么后变成一条线? 2016-6-21](http://www.crystalradio.cn/thread-898772-1-1.html)。里面提到了投线仪、柱面镜、鲍威尔棱镜、激光水平仪，当然还有更神奇的回复 - “点光源加个来回摆动的小镜子就行了，超市扫描枪就是这种结构滴”。

搜索了一下激光水平仪，想起来在工地见过这玩意，但是当时没有太多精力想原理。显然车尾灯的体积比激光水平仪小多了，因此把这个塞里面的想法排除了。来回摆动的小镜子也排除了，太容易受外界干扰了吧，车抖来抖去的。最后把注意力放在了鲍威尔棱镜上，简单搜索下便得到了相关了[光学原理](http://www.fulaser.com/html/cn/news/201512311443.html)
 > 鲍威尔透镜类似于曲面弧顶的圆形棱镜，它是一种激光线发生器，将狭窄的激光束拉伸成均匀照射的直线。  
 鲍威尔透镜与简单的柱面透镜相比，是一字直线发生器性能上的巨大飞跃。柱面透镜产生的直线效果很差，整条线受到不均匀的高斯激光束的限制。鲍威尔透镜的曲面弧顶实际上是一个复杂的二维非球面曲线，会产生大量的球差，从而沿直线重新分布光；减少中心区域的光线，同时增加线条末端的光线强度。从生物医学、汽车装配到3D扫描等机器视觉应用中都使用了非常均匀的照明线。

# 实物验证

本着动手的欲望，拿起螺丝刀拆开看看，拆开之后是下图

![整体结构图](https://img-blog.csdnimg.cn/f96a3905d4da4817ac0b854c91d8346d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

可以看到 5 个红色 LED 和旁边的激光头（五毛激光笔里面的玩意），按钮标识称其为镭射（LASER），确实有点像镭射眼。看一下实际效果吧

![整体工作效果图](https://img-blog.csdnimg.cn/3322f7d3546d4dba849d00661a7f564d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

![激光对比图](https://img-blog.csdnimg.cn/6e30edd9c3524562886a052c66ee4538.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

效果比较简单，均为常量状态，当然也有闪烁模式，将激光头取下来可以看到发出来的光是点光源，直接照到地面就是一个点，想要从侧面看到光路成为一条线的话需要借助丁达尔效应。但这里并不是丁达尔效应，而是鲍威尔棱镜的缘故。

可以从侧面看到透镜底部是平的，上面是一道道凹槽，类似三角波，和上面查到的原理图不同的是这里的透镜有好几个波峰，发散出来的线光和凹槽垂直。所以为了方便确定线的方向，把这个棱镜做成矩形的会更好组装。

![鲍威尔棱镜工作发散图](https://img-blog.csdnimg.cn/6c0e66d7fd574130a3623d9d3f1103bf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
