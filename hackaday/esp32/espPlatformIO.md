---
title: "PlatformIO 与 ESP 32 点灯体验"
date: 2021-07-17T18:31:55+08:00
lastmod: 2021-07-17T18:31:55+08:00
keywords: ['esp', 'esp32', 'platformIO']
description: "在 VS Code 中使用 PlatformIO 与 ESP32 进行开发"
tags: [esp]
categories: [dev-init]
author: "筱氚"
---
# 背景
去年学会了 Arduino 写 ESP 8266+32，后面有看到了 PlatforrmIO，于是那时候立马跃跃欲试，就有了[用PlatformIO开发Esp8266/32](https://blog.csdn.net/weixin_43031092/article/details/109233133)这篇文章，不过由于大部分学习资料（案例、文档、手册、博客）都是使用 Arduino，代码不兼容的我就很少使用了。然后今年在某个项目里神奇的发现，这伙人一个用 Arduino，另一个用 PlatformIO，各自不相同的终端使得这两相安无事，而我自己一个人就同时得整明白这两玩意了...

# 开始动手
- 下载安装 VS Code
- 安装 PlatformIO 插件
- 初始化板子的运行环境

第一步由于自己一直在用 Code，因此跳过。第二步不知道电信和教育网络是不是同时报毙了，Code 里连接不上插件市场，只好从网页端去市场下载相应的 vsix，然后在 Code 的扩展插件里选择 Install From VSIX 安装，安装过程很快，然后它就会自己下载需要的 python 包。之后会自动打开 PIO Home，还是网速问题加载好几次才显示出来。最后新建项目，选择板子的时候就很懵逼了，上面的文章用的是 ESP 8266（多为 ESP 12E 和 ESP 12F），所以选择 Board 的时候没有太纠结就选择了 NodeMCU 1.0(ESP-12E Module)，最后也是能成功点亮；而这次的 ESP 32 是某电商平台最常见的 30 块的开发板，屏蔽罩上印花写的是 ESP-WROOM-32，记得之前自己用的类似开发板上印的是 ESP 32S。

看了好几页的板子的名称，也不知道哪一个合适，在 Boards 里搜索 esp32 会发现更多，幸运的是这个板子后面有个购物车标志可以查看这个板子的详情页，看了水果姐的板子上印的也是 ESP-WROOM-32，看样子和某宝上的差不多，但是卖将近 20 美刀，感谢中国制造，也不是每一款都是详情页，有的页面已经是 404，有的是维基百科，为了看这个板子到底是啥，还得想办法看百科，最后[ESP32. wikipedia.](https://en.wikipedia.org/wiki/ESP32)中描述到了 ESP32-S2, ESP32-C3, ESP32-S3, ESP32-C6 等板子，我都怀疑这个 Board 是不是对应这个百科的了，最后我的 Board 选择的是 `Espressif ESP32 Dev Module`，乐鑫嘛，esp32 的芯片生产商，最后也确实成功的运行起了一个案例。框架（Framework）和上文一样选择的是 Arduino。

第一次用这个 Board 需要从 github 下载些东西，换上教育网络下载了十分钟左右吧。之后就会打开对应的项目。

![初次创建 esp32项目示意图](/images/espPlatformIO/esp32-project.jpg)

## Error：'Arduino.h' file not found
终端中显示了一个错误，然而啥也没动呢，只是刚建立了一个空项目而已，错误显示是 `'Arduino.h' file not found`，搜索了一番在 [‘Arduino.h’ file not found. 2019](https://community.platformio.org/t/arduino-h-file-not-found/11236)
  > okay I looking alone and now I disabled mitaki28.vscode-clang (Look pic) and the problems don’t come I hope its right thx [16] 

然后我就把名字有点类似的插件 `clangd` 给关闭（Disable）了，这一下子就没有这个错误提示了，耶！

后面在复盘的时候发现可以右键那个错误选择复制（不是复制消息哦），然后得到的就是下面的东西，在 source 那里可以知道这个错误的是哪个宝贝插件提示的。
```json
{
	"resource": "/home/kearney/Documents/code/PlatformIO/blink_esp32/src/main.cpp",
	"owner": "_generated_diagnostic_collection_name_#0",
	"code": "pp_file_not_found",
	"severity": 8,
	"message": "'Arduino.h' file not found",
	"source": "clang",
	"startLineNumber": 1,
	"startColumn": 10,
	"endLineNumber": 1,
	"endColumn": 21
}
```

## hello world
我本来想用去年那篇文里的经典点灯代码试验一下的，但是编译不通过，只好再找合适的代码测试到底安装成功了没有。在 [Get started with Arduino and ESP32-DevKitC: debugging and unit testing](https://docs.platformio.org/en/latest/tutorials/espressif32/arduino_debugging_unit_testing.html) 中找到了下面的串口演示代码。

```cpp
#include <Arduino.h>

void setup()
{
    Serial.begin(9600);
}

void loop()
{
    Serial.println("Hello world!");
    delay(1000);
}
```

点击左下角的勾勾编译通过，点击它旁边的箭头即可上传，上传成功后点它旁边的插头打开串口监视器，就能看到上面的经典 - 世界，我来了，你好呀！

![串口监视器显示 hello world](/images/espPlatformIO/serial-hello.jpg)

# Q&A
1. 选择 ESP32 对应的 Board 的时候，不同的 Board 之间到底通用不通用？就好像水果姐的 Adafruit ESP32 Feather 和 Espressif ESP32 Dev Module，毕竟水果姐的那个板子和手上的看起来差别不大。

  [Adafruit ESP32 Feather](https://www.adafruit.com/product/3405) 和某宝上的 ESP 32 开发板不同之处：引脚的数量和标注、串口芯片、凤凰端子。水果的多两个引脚，标注为 An（n 为自然数），手上标注的则是 Dn。到底通用不通用后面有流量再测试吧。

# 参考
- [2020年你还在用Arduino？？快开始用PlatformIO开发Esp8266/32、Arduino、STM32，十分钟亲测ESP8266. Kearney form An idea 2020-10-23](https://blog.csdn.net/weixin_43031092/article/details/109233133)
- [ESP32. wikipedia.](https://en.wikipedia.org/wiki/ESP32)
- [‘Arduino.h’ file not found. 2019](https://community.platformio.org/t/arduino-h-file-not-found/11236)
- [Get started with Arduino and ESP32-DevKitC: debugging and unit testing](https://docs.platformio.org/en/latest/tutorials/espressif32/arduino_debugging_unit_testing.html)
