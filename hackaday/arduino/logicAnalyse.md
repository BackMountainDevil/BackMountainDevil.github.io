# diy arduino 逻辑分析仪
- date: 2022-10-20
- lastmod: 2022-10-22

作用：通过分析不同信号输入的情况判断通信方式。

看起来很像示波器加上分析功能，我的目的比较简单，用来读取 usb 输入的情况。快递封了不少，于是快速寻找可以自己动手的，在 [Arduino 逻辑分析仪 姜戈12 2022-09-29](https://blog.csdn.net/jiangge12/article/details/127001601)中发现了  [aster94/logic-analyzer 2019](https://github.com/aster94/logic-analyzer):Logic Analyzer, for Arduino, AVR, ESP8266 and STM32 with a very nice working processing interface, you could run it also on any Android device

# aster94/logic-analyzer

1. 安装 arduino ，配置 esp8266 开发环境（可选，用 uno 可跳过）
2. 安装 processing
3. 烧录测试脚本来生成方波（可选波形发生器替代）
4. 用 arduino 烧录主程序到开发板中
5. 用 processing 打开示波器仿真程序
6. 在仿真程序中点击 start，给开发板接上方波之类的输入

## 代码
### 测试代码 方波发生器

这里我选择 esp8266 作为方波发生器。烧录之后可以在串口监视器看到 LOW/HIGH 的输出

```c
/*
 * tester.ino
 *
 * Author : Vincenzo
 * Test your logic analyzer with another arduino
 * 修改了引脚
 */

#define led LED_BUILTIN

static const int OUT0 = 4; // D2
static const int OUT1 = 5;  // D1
static const int OUT2 = 12; // D6
static const int OUT3 = 14; // D5

static_assert(OUT0 >= 0 && OUT0 < 16, "");
static_assert(OUT1 >= 0 && OUT1 < 16, "");
static_assert(OUT2 >= 0 && OUT2 < 16, "");
static_assert(OUT3 >= 0 && OUT3 < 16, "");


void setup() {
  Serial.begin(115200);
  
  pinMode(led, OUTPUT);
  pinMode(OUT0, OUTPUT);
  pinMode(OUT1, OUTPUT);
  pinMode(OUT2, OUTPUT);
  pinMode(OUT3, OUTPUT);
}


void loop() {
  digitalWrite(led, HIGH);
  delayMicroseconds(random(200));
  digitalWrite(OUT0, HIGH);
  delayMicroseconds(random(200));
  digitalWrite(OUT1, HIGH);
  delayMicroseconds(random(200));
  digitalWrite(OUT2, HIGH);
  delayMicroseconds(random(200));
  digitalWrite(OUT3, HIGH);
  delayMicroseconds(random(200));
  Serial.println(F("HIGH"));
  
  digitalWrite(led, LOW);
  delayMicroseconds(random(200));
  digitalWrite(OUT0, LOW);
  delayMicroseconds(random(200));
  digitalWrite(OUT1, LOW);
  delayMicroseconds(random(200));
  digitalWrite(OUT2, LOW);
  delayMicroseconds(random(200));
  digitalWrite(OUT3, LOW);
  delayMicroseconds(random(200));
  Serial.println(F("LOW"));
}
```

### 示波器代码

`logic-analyzer/Microcontroller_Code/UNO/UNO.ino`，直接烧录到 uno 中即可

### 上位机界面代码

`logic-analyzer/Computer_Interface/processing.pde`，L9～L11 行代码需要注意适当修改串口。我这里 uno 的串口是 /dev/ttyUSB0，而 esp 的串口是 /dev/ttyUSB1,可以在 arduino 中很方便的查看。因此需要注释掉 pde 代码中的 L11,并且取消L10的注释，还需要注意是否要求该为 ttyUSB1

```c
//String LA_port = "/dev/ttyACM0";    //linux DFU
String LA_port = "/dev/ttyUSB0";  //linux Serial
//String LA_port = "COM10";          //windows
```

## 实践一下

我使用一个 esp8266 作为波形发生器，一个作为接收器，测试结果没有看到任何波形，对调板子烧录程序，也是如此，发生器串口可以看到程序正常运行，接收器不太明朗，串口发送 G 也不太有效。有一回看到一个通道有一个方波，想加入多几个方波的时候，没法复现了。手头上有个 nano3，针对 uno 的程序烧录不进去。

然后找了块钙版 uno 烧录了示波器程序，uno 和 esp 接地，uno 的 8～13 输入端接 esp 的方波输出端 D1 D2 D5 D6，打开上位机代码点击左上角三角形运行，然后在波形图左下角点击 start 开始，打开正常运行。

![上位机示波器运行截图](https://img-blog.csdnimg.cn/b6a1de178ca64e8e93737a9f0c181a7e.png#pic_center)

左下角的 True 点击可以切换是否显示灰色垂直虚线与坐标；点击 xxxseconds 可以切换时间单位；把鼠标放在有数字的按钮上，滚动鼠标滚轮可以进行缩放；底部最右边的按钮是保存图片的按钮，试验会报错如下

`Could not save /home/mifen/Documents/Arduino/logic-analyzer/Computer_Interface/la_capture-1, make sure it ends with a supported extension (JPG, jpg, tiff, bmp, BMP, gif, GIF, WBMP, png, PNG, JPEG, tif, TIF, TIFF, wbmp, jpeg)`

根据码农的特性，在 pde 代码里搜索 save,在 L302 行发现 

```c
String a = "la_capture-"+immage; //+".jpg";  //if you prefer this format, default .tif
```

原来注释说默认是 tif 后缀，但是你这代码是不是忘记修改了，然后把这一行代码修改为

```c
String a = "la_capture-"+immage+".jpg";  // 保存为 jpg 格式
```

重新运行即可，保存功能修复

## 代码剖析

ino 接收主程序代码中有一个串口字符 G，没有收到这个字符就会一直等待。在 pde 中搜索可以发现 仿真示波器程序 会向开发板发送这个字符，然后开发板才会把数据发给仿真程序。

- [Arduino 逻辑分析仪 姜戈12 2022-09-29](https://blog.csdn.net/jiangge12/article/details/127001601)
    > 原理分析：
    上面步骤2之后，步骤3 4 5如果有困难，可以先用串口调试工具发送一个字符“G”， 会收到一大串数据： （ 不接波形发生器没有数据返回 ）
    大概看了下，就是 单片机这边 读取 PORTB ，有变化就记录一次具体变化的端口和时间到数组，凑够200次变化发送给上位机，上位机用 processing 按数组记录的数据绘图。
    因为用到数组，UNO内存较小，还是ESP8266内存大些比较好。

什么是 PORTB，可以阅读 [Arduino - PortManipulation](https://docs.arduino.cc/hacking/software/PortManipulation)，简单的说就是端口寄存器，BCD 则是代表不同的 io 段
> 三种寄存器 (DDR, PORT, PIN)  
    DDR寄存器，决定了引脚是INPUT还是OUTPUT  
    PORT寄存器控制引脚是高电平还是低电平  
    PIN寄存器读取用pinMode()设置为输入的INPUT引脚的状态  
    DDR和PORT寄存器既可以被写入，也可以被读取。PIN寄存器对应于输入的状态，只能被读取  
    B (digital pin 8 to 13)
    C (analog input pins)
    D (digital pins 0 to 7)

# 相关阅读

- [Arduino UNO Logic Sniffer By marklinmax](https://www.instructables.com/Arduino-UNO-Logic-Sniffer/)
- [ESP8266 板子引脚与GPIO引脚对应关系-管脚定义 Kearney 2020-06-15](https://blog.csdn.net/weixin_43031092/article/details/106771413)