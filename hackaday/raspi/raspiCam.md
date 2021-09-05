---
title: "树莓派摄像头无法使用，报错 No data received from sensor，原因是带电插拔摄像头引起损坏"
date: 2021-07-23T17:51:55+08:00
lastmod: 2021-07-23T17:51:55+08:00
keywords: ['camera', 'raspberry']
description: "树莓派摄像头无法使用，报错 No data received from sensor，查询网络世界逐渐排查，最后发现原因是带电插拔摄像头引起损坏，错误信息：Camera control callback  cmd=0x4f525245mmal: No data received from sensor. Check all connections, including the Sunny one on the camera board"
tags: ['raspberry']
categories: [dev-init]
author: "筱氚"
---
# 摄像头错误情况
今天发现了一个程序的 bug 的大致原因，想用树莓派摄像头测试一下，结果显示我摄像头没有连接上。寻思着我也开启了哈，无论是 pi config 界面还是 sudo raspi-config 都显示我开启了，重启也无济于事。把排线重新连接也还是一样...

```bash
$ raspistill -k
Camera control callback  cmd=0x4f525245mmal: No data received from sensor. Check all connections, including the Sunny one on the camera board
^Cmmal: Aborting program
```

# 排查步骤
1. 使能摄像头，确认开启了摄像头的接口（第一次我就是没 enable 浪费了不少时间）
  > sudo raspi-config
2. 断电重启树莓派
3. 测试摄像头
  > raspistill -k

到第三步我的报错依然一样，问题完美的重现。。。

## 检测摄像头
[解决新版树莓派无法开启CSI接口摄像头问题. 白学家Lynn 2018-03-25](https://blog.csdn.net/lynn_coder/article/details/79687939)
 > vcgencmd get_camera  
 发现support=0  确实没识别到摄像头

```bash
$ vcgencmd get_camera
supported=1 detected=1
```
根据白的办法发现我这里检测到了摄像头！那说明连接还是可以的嘛。

## 加载系统模块
[树莓派无法打开摄像头（USB和piCam均无法打开） 解决过程记录。 肥杨同学 2020-04-04](https://blog.csdn.net/qq_39522167/article/details/105317063):错误信息不一样，最后重装了系统好了！！！！！！！
  > 利用 $ sudo raspi-config 使能摄像头。
  打开sudo nano /etc/modules在其中加入bcm2835-v4l2 注意是4L2不是1,L为小写。

```bash
# 加载模块bcm2835-v4l2
sudo modprobe bcm2835-v4l2
# 重启测试摄像头，错误依旧
raspistill -k
# 不死心，在其中加入bcm2835-v4l2，最后也失败了
sudo nano /etc/modules
```

## 换个摄像头
- [树莓派摄像头烧坏后感言。刘小白DOER。2021.03.20](https://www.jianshu.com/p/f8b2af92ff16)：指令不太相同但错误信息一致，
  > 运行拍照命令raspistill -o image.jpg又有错误信息：Camera control callback cmd=0x4f525245mmal: No data received from sensor. Check all connections, including the Sunny one on the camera board。我勒个去，原来是带电的时候插拔摄像头，导致管脚烧坏了，没有数据返回。 

- [求助！树莓派4B连接官方摄像头后报错。Charrrrr • 2020-03-04](https://talk.quwj.com/topic/756)：两种错误推测
  > Spoony 小组长 2020-03-04 
  帮你查了一下，这个问题可能是硬件本身有问题。官方的工程师基本上建议是联系经销商退货处理。  
  > RaspiSQH 79.05m 2020-03-04 
  我的摄像头挂掉了就是这样。你是不是热插拔了。。。。。。 

在李总的大力支持下，支持了一个摄像头，换上即可使用，看来原因很有可能是刚开始出现异常的时候带电插拔导致的烧摄像头了，最后某宝走起，中国制造真香。

犯错是正常的，要发现错误原因，从而下一次再遇到类似的情形的时候，解决它。
# 参考
- [解决新版树莓派无法开启CSI接口摄像头问题. 白学家Lynn 2018-03-25](https://blog.csdn.net/lynn_coder/article/details/79687939):开启摄像头，在/etc/modules-load.d中加入 bcm2835-v4l2
- [树莓派无法打开摄像头（USB和piCam均无法打开） 解决过程记录。 肥杨同学 2020-04-04](https://blog.csdn.net/qq_39522167/article/details/105317063):错误信息不一样，最后重装了系统好了！！！！！！！
  > 利用 $ sudo raspi-config 使能摄像头。
  打开sudo nano /etc/modules在其中加入bcm2835-v4l2 注意是4L2不是1,L为小写。

- [树莓派摄像头烧坏后感言。刘小白DOER。2021.03.20](https://www.jianshu.com/p/f8b2af92ff16)：指令不太相同但错误信息一致
- [求助！树莓派4B连接官方摄像头后报错。Charrrrr • 2020-03-04](https://talk.quwj.com/topic/756)
- [求助！树莓派4B使用摄像头报错](https://tieba.baidu.com/p/6528558098)：相同错误，我换摄像头就好了，这个楼主没那么好运了
  > 使用vcgencmd get_camera检查摄像头连接正常（supported=1 detected=1）
但是一用拍照指令，会在几秒的卡顿后报错（Camera control callback cmd=0×4f525245mmal: No data received from sensor. Check all connections, including the Sunny one on the camera board）
之前的旧相机就有这个问题，换了新相机后还是这样。CSI插口没差错，排线也没插反。
