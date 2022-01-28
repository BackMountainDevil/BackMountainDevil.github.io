# 为 Esp32 配置 Arduino 开发环境并测试
- date: 2021-02-02
- lastmod: 2022-01-28

# 简介

最近Raspberry 推出了新款双核单片机Pico，大家一片欢呼。然而乐鑫早年的 Esp 32 不仅双核、还拥有板载蓝牙、WIFI技术，价格还低于 Pico，早已兼容 Arduino、Micropython。下面介绍如何在 Arduino 上配置 Esp 32开发板。开始之前首先安装 Arduino 最新版（[Arduino Download](https://www.arduino.cc/en/software)），这个我想大家都懂，不懂留言哈。想配置 PlatformIO 的可以看[PlatformIO 与 ESP 32 点灯体验.Kearney form An idea. 2021-07-17](https://blog.csdn.net/weixin_43031092/article/details/118861747)

不过需要注意的是，这种方式目前还没有支持全部芯片的全部功能，有些可能还没有支持，具体支持情况请查阅[ESP32 Arduino Core’s documentation -> Libraries -> Supported Peripherals](https://docs.espressif.com/projects/arduino-esp32/en/latest/libraries.html)

# 常规配置办法
## 附加开发板地址
打开 Arduino，从菜单栏中这样选择：`“文件”–>“首选项”–>附加开发板管理器网址`，加入这个地址 https://dl.espressif.com/dl/package_esp32_index.json ，点击好保存即可。从其它地方还看到这个地址https://git.oschina.net/dfrobot/FireBeetle-ESP32/raw/master/package_esp32_index.json，这其实是DFRobot的镜像，但是版本低了点（具体可以把网址粘贴到浏览器查看内容进行对比）。

## 添加开发板

打开 Arduino，从菜单栏中这样选择：`“工具”–>“开发板”–>“开发板管理器” `，搜索 esp32，点击下载安装，安装过程会取决于访问 github 的速度。如果下载超过七分钟，那此时你的网速暂不支持方法，要么采取科学冲浪的方式，要么采取下面的非常规办法。

## 测试

参见[为 Esp8266 配置 Arduino 开发环境并测试 WiFi](/hackaday/esp8266/arduino.md)中的测试办法。开发板可选择 ESP32 -Node32s。

在 Linux 系统上，有时候会出现上传错误，缺少 serial 库，此时需要我们手动安装该库（apt install python3-pyserial 或者 pip3 install pyserial --user(--user表示仅为当前用户安装该库)）

# 非常规办法
## 思路

既然从 github 下载很慢，那我从别人下好的文件里拷贝不就行了吗，又或者从 gitee 下载。
这里有两种办法：
1. 下载别人打包好的（但这样通常与最新版不匹配，容易过时）
2. **自己从gitee下载**：以前我在github下载慢的时候，通常会导入到gitee再下载，通过gitee的加速就很快，我看gitee上有好几个这么做的，但是这里特别的感谢乐鑫，他们采取异步操作的方式同步到gitee了。

## 动手-Gitee
### 准备py、git

此办法最少需要git，有Pyhon3就更好了。没有也不是不行，方法后面说
python3自行查找安装办法，git可以参考这个： [Git for windows安装到配置GUI版Git安装与最小化安装 - kearney](https://blog.csdn.net/weixin_43031092/article/details/112549639?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161225034616780261994876%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161225034616780261994876&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-112549639.pc_v2_rank_blog_default&utm_term=git&spm=1018.2226.3001.4450)

### 动手

确保你已经安装了较新版的Arduino。然后确保它没有被打开。
找到Arduino安装路径。进入hardware目录，打开终端，粘贴下面的命令回车
以下方法在Linux Deein测试成功，在win下有所不同，参考下面一点点的
```bash
mkdir -p espressif && \
cd espressif && \
git clone https://gitee.com/espressif-systems/arduino-esp32.git esp32 && \
cd esp32 && \
git submodule update --init --recursive && \
cd tools && \
python3 get.py
```

上面的网址原本是https://github.com/espressif/arduino-esp32.git，换成gitee的就行了，

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210202153411607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210202153550686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)亲测有效。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210202154144648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

#### windows

在最后一行有所不同
```bash
mkdir -p espressif
cd espressif 
git clone https://gitee.com/espressif-systems/arduino-esp32.git esp32 
cd esp32
git submodule update --init --recursive
cd tools
。/get.exe
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301103309380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

### 不装py3、git

我也很讨厌装一堆东西、简洁最好了。打开[https://gitee.com/espressif-systems/arduino-esp32](https://gitee.com/espressif-systems/arduino-esp32)，下载ZIP。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021020215395853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

很快啊。然后进入到arduino安装目录下的hardware，建立文件夹espressif，进去espressif，再建立一个esp32文件夹，把下载下来的zip解压到esp32里面，解压完类似酱紫。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210202154420636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)然后进入tools，wnidows用户双击允许get.exe允许即可。linux用户双击允许get.py即可（Linux自带python3）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210202154514997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)


# 参考

- [Arduino搭建ESP32开发环境路上遇到的天坑-Github-记录-教程 - Kearney](https://blog.csdn.net/weixin_43031092/article/details/106860485)
- [Installation instructions for Debian / Ubuntu OS - Espressif Systems / arduino-esp32 ](https://gitee.com/espressif-systems/arduino-esp32/blob/master/docs/arduino-ide/debian_ubuntu.md)
- [ESP32-CAM摄像头-Arduino IDE-网页展示-人脸识别-之七次失败后的成功记录 - Kearney](https://blog.csdn.net/weixin_43031092/article/details/106962217?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161224981316780265457517%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161224981316780265457517&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-2-106962217.pc_v2_rank_blog_default&utm_term=esp32&spm=1018.2226.3001.4450)
- [Git for windows安装到配置GUI版Git安装与最小化安装 - kearney](https://blog.csdn.net/weixin_43031092/article/details/112549639?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161225034616780261994876%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161225034616780261994876&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-112549639.pc_v2_rank_blog_default&utm_term=git&spm=1018.2226.3001.4450)
- [Arduino Download](https://www.arduino.cc/en/software)
- [https://gitee.com/espressif-systems/arduino-esp32](https://gitee.com/espressif-systems/arduino-esp32)
