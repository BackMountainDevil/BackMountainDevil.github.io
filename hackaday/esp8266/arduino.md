# 为 Esp8266 配置 Arduino 开发环境并测试 WiFi
- date: 2020-04-16
- lastmod: 2021-09-21

# 安装驱动

为了让电脑可以正确识别开发板，需要装设备驱动（免驱插上就会自动安装），大部分廉价的板子会采用 CH340 芯片，那么就装 CH340 的驱动，这个商家一般都会放在商品详情页面，附一个[某云下载地址](https://pan.baidu.com/s/1dGXBNxB), CP2102 其实是免驱芯片，价格比 CH340 高一丢丢，以防万一也附一个[某云下载地址](https://pan.baidu.com/s/1dF6iiS5)

装了驱动之后，如果板子连上电脑，在设备管理器中的端口名称就会显示芯片名字，不装驱动的话程序就无法正确烧录到开发板上（用过中国制造 Arduino UnoR3 的盆友估计都装了CH340的驱动吧）


# Arduino 附加开发板

1. 打开arduino，菜单栏-文件-首选项，在“附加开发板管理器网址”中粘贴这个网址
http://arduino.esp8266.com/stable/package_esp8266com_index.json ，点击好。
2. 菜单栏-工具-开发版-开发版管理器，搜索 ESP8266 ，只出现一个结果，选择最新版安装即可，亲测安装较慢。。。。
3. 菜单栏-工具-开发版，选择 NodeMCU 1.0
4. 选择端口，在 windows菜单 中搜素设备管理器，进入设备管理器之后，查看端口，一搬也就几个端口，看看那个名称对应你的芯片就在 arduino 的菜单栏-工具-端口，选择对应的端口

不加开发板管理的话是搜不到对应的库的

![未添加附加开发板管理的搜索结果 - 空](https://img-blog.csdnimg.cn/20210111180134449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

![搜索结果](https://img-blog.csdnimg.cn/20210111180254608.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

## 亮灯测试

Arduino-菜单栏-文件-示例-Basics-Blink，这是 Arduino、Esp 系列的经典点灯代码，选择好开发板 ESP8266 -Nodemcu 1.0 和端口，上传之后开发板会自己重启，一切正常的话就说明成功了

## 连接wifi

Arduino-菜单栏-文件-新建，在新窗口中删除原来的所有代码，
复制下面的代码粘贴进去，然后修改wifi.begin中的wifi名和密码，注意名称和密码要用英文双引号括起来

```c
#include <ESP8266WiFi.h>
void setup() {
  //Serial.begin(115200);//esp8266默认波特率乱码
  Serial.begin(9600);
  Serial.println();

  WiFi.begin("KearneyShuai", "shuaidaozhalie");//第一个参数是wifi名，第二个是wifi密码

  Serial.print("Connecting");
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();

  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());

}

void loop() {

}
/*
 * Code by DaLao
 * Spread by kearney
 * */
 */
```

保存，命名不限制，例如node8266-2-wificonnect，然后ctrl+u验证，验证通过后再ctrl+U上传代码到板子里。
此时打开菜单栏中的工具-串口监视器，运行成功之后就会看到

![串口监视器输出对应内容](https://img-blog.csdnimg.cn/20200416111627176.PNG#pic_center)

# 参考

 - [Arduino 配置 ESP8266环境.GetcharZp. 2019-09-25 ](https://blog.csdn.net/qq_39042062/article/details/101413999)
 - [物联网初探 - NodeMCU（ESP8266）WIFI芯片开发板试玩 - 完全新手向. 2019-11-02. 乐只兮兮鹿 ](https://www.bilibili.com/video/BV1rE411t78H?p=1)
 - [KaleoFeng/Esp8266WithArduino - Github](https://github.com/KaleoFeng/Esp8266WithArduino) 
 - [ESP8266 Arduino Core. Docs » Installing](https://arduino-esp8266.readthedocs.io/en/latest/installing.html)
