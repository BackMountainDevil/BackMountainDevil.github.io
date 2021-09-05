---
title: "ESP32 疯狂重启的原因探究"
date: 2021-07-18T23:08:18+08:00
lastmod: 2021-07-18T23:08:18+08:00
keywords: ['uc2', 'esp32', 'error']
description: "ESP32 疯狂重启的原因探究，最后发现是程序的问题"
tags: [esp]
categories: [dev-init]
author: "筱氚"
---
# 背景介绍
ESP 32 烧录点灯程序正常，烧录自定义代码也正常，烧入某个代码就疯狂重启。

## 错误信息

<details>
<summary> 点击查看串口输出</summary>

错误程序[源码地址](https://gitee.com/anidea/UC2-Software-GIT/blob/V2.0.0/HARDWARE_CONTROL/ESP32/GENERAL/ESP32_ledarr/src/ESP32_ledarr/ESP32_ledarr.ino)
```
This is:ESP32_40863 on /S007/LAR01.
VOID SETUP -> topicSTATUS=/S007/LAR01/STAT

Device-MAC: 7C:9E:BD:06:F6:C8
Connecting to Kearney.
WiFi connected with IP:172.20.10.6
abort() was called at PC 0x40085d61 on core 1

ELF file SHA256: 0000000000000000

Backtrace: 0x40088a8c:0x3ffb1e00 0x40088d09:0x3ffb1e20 0x40085d61:0x3ffb1e40 0x40085e8d:0x3ffb1e70 0x400d8a77:0x3ffb1e90 0x400d485e:0x3ffb1ec0 0x400d4a7c:0x3ffb1f20 0x400d116c:0x3ffb1f40 0x400d1b94:0x3ffb1f60 0x400d58ce:0x3ffb1fb0 0x4008a7be:0x3ffb1fd0

Rebooting...
ets Jun  8 2016 00:22:57

rst:0xc (SW_CPU_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:1
load:0x3fff0018,len:4
load:0x3fff001c,len:1044
load:0x40078000,len:10124
load:0x40080400,len:5856
entry 0x400806a8
This is:ESP32_94258 on /S007/LAR01.
VOID SETUP -> topicSTATUS=/S007/LAR01/STAT

Device-MAC: 7C:9E:BD:06:F6:C8
Connecting to Kearney..
WiFi connected with IP:172.20.10.6
abort() was called at PC 0x40085d61 on core 1

ELF file SHA256: 0000000000000000

Backtrace: 0x40088a8c:0x3ffb1e00 0x40088d09:0x3ffb1e20 0x40085d61:0x3ffb1e40 0x40085e8d:0x3ffb1e70 0x400d8a77:0x3ffb1e90 0x400d485e:0x3ffb1ec0 0x400d4a7c:0x3ffb1f20 0x400d116c:0x3ffb1f40 0x400d1b94:0x3ffb1f60 0x400d58ce:0x3ffb1fb0 0x4008a7be:0x3ffb1fd0

Rebooting...
ets Jun  8 2016 00:22:57

rst:0xc (SW_CPU_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:1
load:0x3fff0018,len:4
load:0x3fff001c,len:1044
load:0x40078000,len:10124
load:0x40080400,len:5856
entry 0x400806a8
This is:ESP32_40149 on /S007/LAR01.
VOID SETUP -> topicSTATUS=/S007/LAR01/STAT

Device-MAC: 7C:9E:BD:06:F6:C8
Connecting to Kearney..
WiFi connected with IP:172.20.10.6
abort() was called at PC 0x40085d61 on core 1

```
</details>

## 两个正常程序
上面的程序烧录运行依次，但是烧录下面的任何一个都是可以正常运行的，因此排除板子异常的问题。而换一个板子烧录程序也是一样的状况，基本可以断定程序出错了。
### Blink
Arduino - 文件 - 示例 - Basic - Blink，板子选择 Node32s，确认端口编译上传，正常闪烁。
### ws2812 点灯程序

<details>
<summary>点击展开代码</summary>

```cpp
// https://arduino.nxez.com/2019/06/10/arduino-driving-ws2812-led.html

#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif
 
// 控制 WS2812 灯条的引脚编号
#define PIN        26
 
//定义控制的 LED 数量
#define NUMPIXELS 5
 
Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);
 
//相邻 LED 之间的延迟，单位毫秒
#define DELAYVAL 500
 
void setup() {
#if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
  clock_prescale_set(clock_div_1);
#endif
  pixels.begin(); // INITIALIZE NeoPixel strip object (REQUIRED)
  Serial.begin(115200);
  Serial.println("start");
}
 
void loop() {
  pixels.clear(); // Set all pixel colors to 'off'
  for(int i=0; i<NUMPIXELS; i++) { // For each pixel...
    pixels.setPixelColor(i, pixels.Color(0, 150, 0));
    pixels.show();  
    delay(DELAYVAL); // Pause before next pass through loop
  }
}
```
</details>

#### 串口输出
五个灯依次亮起，往复循环，一切正常
```bash
rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
flash read err, 1000
ets_main.c 371 
ets Jun  8 2016 00:22:57

rst:0x10 (RTCWDT_RTC_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:1
load:0x3fff0018,len:4
load:0x3fff001c,len:1044
load:0x40078000,len:10124
load:0x40080400,len:5856
entry 0x400806a8
start

```

# 探究过程
arduino 程序里有两部分： setup 和 loop，把其中关于 LED 的 matrix 都注释掉，发现不会再疯狂重启，在 raspi 上用 `mosquitto_pub -t /S007/LAR01/RECM -m "PXL+20+127+255+50"` 测试 esp32 可以正常收到。

然后对 matrix 用二分法注释，最后定位到 matrix_show() 中的 portDISABLE_INTERRUPTS() 和 portENABLE_INTERRUPTS()，将这两注释掉便不再疯狂重启。


# 参考
- [ESP8266模块无限重启崩溃问题. 西红柿爆炒鸡蛋 2020-06-09](https://blog.csdn.net/qq_42150119/article/details/106637591)：Rst cause + Fatal Exception 分析原因
- [ESP32无限重启。少年歌行Faker 2020-11-29](https://blog.csdn.net/weixin_45445275/article/details/110312130):entry 之后的输出不同
  > I(434), I(436), I(4xx)
  > 最终原因：供电问题：只要是3.3V供电就会出现该问题，若是5V供电（板载稳压）则没该问题。
- [esp reboot crazy when ESP32_ledarr flashed. BackMountainDevil. 2021-07-23](https://github.com/openUC2/UC2-Software-GIT/issues/20)：向原作反馈提的 issue，后来自己发现是哪里出问题了
- []()