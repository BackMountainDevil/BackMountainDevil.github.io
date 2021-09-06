---
title: "Mqtt mosquitto fontgoogle"
date: 2021-07-18T22:59:09+08:00
lastmod: 2021-07-18T22:59:09+08:00
keywords: []
description: ""
tags: []
categories: [Blog]
author: "筱氚"
---
# mqtt

```bash
sudo apt-get install mosquitto mosquitto-clients

客户端，模拟订阅
mosquitto_sub -v -t topic01
// motor
mosquitto_sub -v -t /S013/MOT01/STAT
mosquitto_sub -v -t /S013/MOT01/RECM
// led
mosquitto_sub -v -t /S007/LAR01/STAT
mosquitto_sub -v -t /S007/LAR01/RECM
mosquitto_sub -v -t /S007/LAR01/ANNO

一个客户端，模拟发布
mosquitto_pub -t topic01 -m hello
mosquitto_pub -t /S013/MOT01/RECM -m "DRVZ+1000"

mosquitto_pub -t /S007/LAR01/RECM -m "PXL+2+127+255+50"
```


# fontgoogle

https://cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css 中引入了 fonts.googleapis.com 的东西，导致加载缓慢

- [网站卡在“fonts.googleapis.com”谷歌字体，解决方案。xiangjai 2018-06-01](https://blog.csdn.net/xiangjai/article/details/80541465):放到本地加载
  > `@import url("https://fonts.googleapis.com/css?family=Open+Sans:300,400,600,700"); `   
  > `@import url("googleapis-fonts/Open+Sans.css");`
- [fonts.googleapis.com 加载慢解决方案。MrtBian 2019-11-09](https://blog.csdn.net/MrtBian/article/details/102991383)
  > fonts.googleapis.com 换成 fonts.font.im
- []()
- []()

# 参考
- []()
