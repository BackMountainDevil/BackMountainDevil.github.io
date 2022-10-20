# diy 逻辑分析仪
- date: 2022-10-20
- lastmod: 2022-10-20

作用：通过分析不同信号输入的情况判断通信方式。

看起来很像示波器加上分析功能，我的目的比较简单，用来读取 usb 输入的情况。快递封了不少，于是快速寻找可以自己动手的，在 [Arduino 逻辑分析仪 姜戈12 2022-09-29](https://blog.csdn.net/jiangge12/article/details/127001601)中发现了  [aster94/logic-analyzer 2019](https://github.com/aster94/logic-analyzer):Logic Analyzer, for Arduino, AVR, ESP8266 and STM32 with a very nice working processing interface, you could run it also on any Android device

# aster94/logic-analyzer

1. 安装 arduino ，配置 esp8266 开发环境（可选，用 uno 可跳过）
2. 安装 processing
3. 烧录测试脚本来生成方波（可选波形发生器替代）
4. 用 arduino 烧录主程序到开发板中，如 logic-analyzer/Microcontroller_Code/ESP8266/ESP8266.ino
5. 用 processing 打开示波器仿真程序 logic-analyzer/Computer_Interface/processing.pde
6. 在仿真程序中点击 start，给开发板接上方波之类的输入

## 实践一下

我使用一个 esp8266 作为波形发生器，一个作为接收器，测试结果没有看到任何波形，对调板子烧录程序，也是如此，发生器串口可以看到程序正常运行，接收器不太明朗，串口发送 G 也不太有效。有一回看到一个通道有一个方波，想加入多几个方波的时候，没法复现了。手头上有个 nano3，针对 uno 的程序烧录不进去。

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