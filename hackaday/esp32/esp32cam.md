# 为 Esp32 Cam 配置 Arduino 开发环境并人脸识别测试
- date: 2021-06-04
- lastmod: 2021-06-04

买回来一个OV7076.。。。找帖子发现到了。。但是太复杂。。。。于是找到一个简单的 ESP32 Cam，当时觉得这牛逼啊，还简单容易上手

# Arduino 开发环境配置

参照[为 Esp32 配置 Arduino 开发环境并测试](/hackaday/esp32/arduino.md)进行 esp32 环境配置即可。

开发板选择 ESP32 ESP32 Wrover Module，还需要设置 Partition Scheme 为 Huge APP（菜单栏–>工具–>Partition scheme）

![菜单栏–>工具–>Partition scheme](https://img-blog.csdnimg.cn/202006252108503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

# 经典示例

打开 Arduino,菜单栏–>文件–> 示例–> ESP32 Camera–>  CameraWebServer

wifi名称和密码改为自己知道密码的那个

![wifi帐号密码的代码位置L18～L19](https://img-blog.csdnimg.cn/2020062521105639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

代码中的摄像头模块默认选择是第一种摄像头模块，由于自己也不知道哪个对应哪一个，不过呢，这里我板子上印的是安可信，所以我在这里选择第五个，大部人也是第五个，注意把默认的第一个注释掉（前面加两繁写刚），然后取消自己模块前面的注释。。。。（我就是没取消第一个折腾了好久才发现的。。）

![默认的镜头选择代码](https://img-blog.csdnimg.cn/20200625211121861.png)
如下图所示

![注释L9,取消L14的注释](https://img-blog.csdnimg.cn/20200625211251541.png)

## 接线上传

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020062521143782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

这里我用3.3V之后会发现一个问题（见问题二），上传之前记得IO 0要接地，如下图我使用了短路帽，也可以用杜邦线替代

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625211615338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

连接好之后usb连接电脑，选好端口COM，点击上传

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625212003766.png)

有的时候会出现connect比较长时间，可尝试重新插usb或者按下一板子上的RTS按钮

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625212112367.png)

上传完之后会出现 `Hard reset via RTS pin...`
这个时候暗示你拔掉IO 0和GND的短路接线，然后按下RTS按钮就欧克了

## 收工

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625212322617.png)
打开串口监视器，选择115200的波特率之后就能看到非乱码输出
......表示正在连接wifi，连接成功之后会显示内网IP，直接在**连接了同一WiFi的设备**的浏览器地址栏上输入这个IP（`172.20.10.14`，默认访问80端口）就可以访问这个摄像头了
`172.20.10.14:81`则表示访问。。。我发现无法访问404

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200625213113154.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

点击Start Stream就能看到实时图像了，开启选项Face Dection和Face Recognition就可以玩耍人脸识别了（识别的分辨率不高，这个价格还要什么自行车）

# 问题记录
## Detected camera not supported

```bash
[E][camera.c:1049] camera_probe(): Detected camera not supported.
[E][camera.c:1249] esp_camera_init(): Camera probe failed with error 0x20004
```

**可能的原因：** 
- 修改摄像头模块 这一步跳过了或者操作有误
- usb接线错误

## Brownout detector was triggered

```bash
20:39:26.411 -> Brownout detector was triggered
20:39:26.411 -> ets Jun  8 2016 00:22:57
20:39:26.411 -> 
20:39:26.411 -> rst:0xc (SW_CPU_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
20:39:26.411 -> configsip: 0, SPIWP:0xee
20:39:26.411 -> clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
20:39:26.411 -> mode:DIO, clock div:1
20:39:26.411 -> load:0x3fff0018,len:4
20:39:26.411 -> load:0x3fff001c,len:1216
20:39:26.411 -> ho 0 tail 12 room 4
20:39:26.411 -> load:0x40078000,len:9720
20:39:26.411 -> ho 0 tail 12 room 4
20:39:26.411 -> load:0x40080400,len:6352
20:39:26.411 -> entry 0x400806b8
```

 **可能的原因：** 
 - 供电不足，接5V供电重新烧录

# 参考

- [esp32-cam摄像头+远程遥控小车](https://blog.csdn.net/qq_43141903/article/details/105240958)
- [视频教程](https://www.bilibili.com/video/av57997536?from=search&seid=3652933720567762882)
- [失败案例](https://zhuanlan.zhihu.com/p/104356606)
- [GitHub issue](https://github.com/espressif/esp32-camera/issues/118)
- [ESP32-CAM摄像头开发板. 安信可](https://docs.ai-thinker.com/esp32-cam)
- [ESP32-CAM AI-Thinker引脚指南：GPIO使用说明.2021-5-23. Luca](https://www.qutaojiao.com/24272.html)
