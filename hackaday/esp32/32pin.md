# Esp32 开发板的管脚图与 GPIO 对应关系
- date: 2020-08-21
- lastmod: 2021-09-21

# 管脚图

![管脚图](https://img-blog.csdnimg.cn/20200821214227416.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70#pic_center)

35.34.36.39 不能配置为PWM

# Arduino

```bash
Adafruit_ILI9341 tft = Adafruit_ILI9341(5, 17, 23, 18, 4, 19);    //esp32-nodemcu32s
```
在上面的代码中，5、17分别指的是GPIO5、GPIO17，以此类推

# 参考

- [技术文档 乐鑫科技](https://www.espressif.com/zh-hans/support/documents/technical-documents?keys=8266&field_type_tid%5B%5D=14&field_type_tid%5B%5D=14)
- [ESP8266 板子引脚与GPIO引脚对应关系-管脚定义.Kearney form An idea. 2020-06-15](https://blog.csdn.net/weixin_43031092/article/details/106771413)
