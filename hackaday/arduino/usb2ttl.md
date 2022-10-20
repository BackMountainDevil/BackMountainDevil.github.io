# usb转ttl UART 串口转换通信
- date: 2022-10-20
- lastmod: 2022-10-20

# 硬件
## 转换板

某宝上有卖，通常是 ch340 芯片，好一点是 cp2102 芯片，然后把外围电路搭建起来即可，一头 USB 口，一头好几个针脚(VCC, GND, RX, TX)，需要注意的是 tx 接 rx ，rx 接 tx。

## 开发板

手头上的转换板换了咋办。用常见的开发板充临时当转换板也可，常见开发板通常配备了串口转换芯片，将开发板设置为串口转发即可。

- [将arduino用作ttl串口转USB通信模块  2015/11/28 糖衣](https://www.rcsugus.com/2015/11/28/arduino_ttl_to_usb/)
    > ，TX接RX（pin8），RX接TX（pin9），arduino插电脑，开ide，串口监视器，成功。可以理解为，开启一个软串口跟模块相连，硬串口跟电脑相连，arduino起到一个信息中转的作用。

    ```c++
    #include <SoftwareSerial.h>
    SoftwareSerial WIFISerial(8, 9); // RX, TX
    void setup() {
        Serial.begin(115200); //跟电脑通信的速率115200
        WIFISerial.begin(115200); //跟esp8266通信速率，根据自己情况修改
    }
    void loop() {
        if (WIFISerial.available()) {
        Serial.write(WIFISerial.read());
        }
        if (Serial.available()) {
        WIFISerial.write(Serial.read());
        }
    }
    ```

# 相关阅读

- [ UART、TTL、RS232等概念的区别与联系 2017-11-20 ](https://fanzheng.org/archives/39)
    >   UART其实是一种软件层面的协议，定义了我们的信息要怎么组织封装成一串01序列。
    TTL本身是指一种集成电路的设计工艺，而我们常说的TTL指的是TTL电路的电平标准，可以认为是一种物理层面的协议，但它只定义了表示01所使用的电压，没有定义怎么传输。通常有5V和3.3V两种。
    RS232的内容多一些，物理层面的内容基本都定义了，具体可以查看wiki。RS232通常使用DE-9的D-sub接口，内部的数据通常采用UART协议。
    类似地，再分析USB。USB这个概念比较庞大，不仅定义了接口形状和物理层面的协议，软件层面的协议也一并定义了。也可以说是USB把这些内容都纳入到了一个概念内。
