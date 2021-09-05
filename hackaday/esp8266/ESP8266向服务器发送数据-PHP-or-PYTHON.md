---
title: "ESP8266向服务器发送数据 TCP - PHP or PYTHON"
date: 2020-06-22T04:53:52+08:00
lastmod: 2020-06-22T04:53:52+08:00
keywords: []
description: ""
tags: []
categories: []
author: "筱氚"
---
# 简介

ESP8266向服务器发送数据，服务器采用PHP或者PYTHON来接收处理数据

WiFiClient库用于ESP8266的TCP协议通讯

[CSDN](https://blog.csdn.net/weixin_43031092/article/details/106894646)

# python

假设8266直接向服务器**8888**端口发送数据，服务器使用py程序监听8888端口

[Centos安装Python教程](https://blog.csdn.net/weixin_43031092/article/details/106892228)：	https://blog.csdn.net/weixin_43031092/article/details/106892228

## 检查端口是否被占用

```bash
[root@ecs ~]# netstat -lnpt		//查看所有被监听的端口
[root@ecs ~]# netstat -tunlp|grep 8888		//查看8888端口的占用情况
```

如果被占用，就换一个没有被占用的端口，记得防火墙和安全组要放行该端口

## 服务端启动py监听程序

```python
'''
/**********************************************************************
项目名称/Project          : 零基础入门学用物联网
程序名称/Program name     : cs-s-py.py
团队/Team                : 
作者/Author              : Kearney
日期/Date（YYYYMMDD）     : 20200623
程序目的/Purpose          : 
演示如何实现NodeMCU间通过WiFi进行与服务器通讯。服务端采用python接收数据，
ESP8266以客户端模式运行
 
此代码为服务端代码。此代码主要功能：
    - 通过HTTP协议接收来自客户端的数据
-----------------------------------------------------------------------
修订历史/Revision History  
日期/Date    作者/Author      参考号/Ref    修订说明/Revision Description
-----------------------------------------------------------------------
***********************************************************************/
'''
import threading
import socket
encoding = 'utf-8'
BUFSIZE = 1024

##读取端口消息
class Reader(threading.Thread):
    def __init__(self, client):##获取客户端
        threading.Thread.__init__(self)
        self.client = client

    def run(self):##持续接收消息并处理
        while True:
            data = self.client.recv(BUFSIZE)##接收字节消息
            if (data):
                string = bytes.decode(data, encoding)##转化为字符串
                print(string)##打印接收到的消息
                //to do,可以插入函数，对数据进行处理存储
                self.client.send("Received data successfully")##回复消息
            else:
                break

##建立端口监听
class Listener(threading.Thread):
    def __init__(self, port):
        threading.Thread.__init__(self)
        self.port = port
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.sock.bind(("0.0.0.0", port))
        self.sock.listen(0)

    def run(self):
        print("listener started")
        while True:
            client, cltadd = self.sock.accept()
            Reader(client).start()
            cltadd = cltadd

lst = Listener(8888)  # 建立监听线程，端口号根据需要修改
lst.start()  # 启动监听
```

将程序上传到服务器或者直接在服务器上写-.-

```bash
[root@ecs tmp]# ls
cs-s-py.py       systemd-prirvice-XpBmVu  mongodb-27017.sock 
[root@ecs tmp]# python cs-s-py.py 
listener started
```

## 8266写入程序

```c++
/**********************************************************************
项目名称/Project          : 零基础入门学用物联网
程序名称/Program name     : cs-c-py
团队/Team                : 
作者/Author              : Kearney
日期/Date（YYYYMMDD）     : 20200623
程序目的/Purpose          : 
演示如何实现NodeMCU间通过WiFi进行与服务器通讯。服务端采用python接收数据，
ESP8266以客户端模式运行
 
此代码为客户端代码。此代码主要功能：
    - 通过HTTP协议向服务器发送数据
-----------------------------------------------------------------------
修订历史/Revision History  
日期/Date    作者/Author      参考号/Ref    修订说明/Revision Description
-----------------------------------------------------------------------
***********************************************************************/
#include <ESP8266WiFi.h>

const char* ssid = "？？？";        //wifi名称
const char* password = "？？？？";  //wifi密码
const char* host = "101.200.101.101";    //服务器公网ip
const int httpPort = 8888;              //服务器端口号

void setup() {
  Serial.begin(9600);//开启串口监视器
  WiFi.begin(ssid, password);//使用名称和密码链接wifi
  while (WiFi.status() != WL_CONNECTED) {//如果连接成功跳出循环,没成功则一直尝试连接
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
}

void loop() {
  httpRequest(); // 发送HTTP请求
  delay(5000);    //再次请求间隔时间，单位毫秒
}

void   httpRequest(){
  Serial.println();
  Serial.println("Start Connecting ...  ");
  WiFiClient client;    // 建立WiFi客户端对象，对象名称client

  if(client.connect(host,httpPort)){    //向服务器发送连接请求
    Serial.print("Connecting Successfully to ");
    Serial.print(host);  
    Serial.print("  :  ");   
    Serial.println(httpPort);     

    Serial.print("Current Client Status: ");  //获取设备与服务器的连接状态
    Serial.println(client.status());          //4代表成功建立连接，10代表连接超时
    
    String data = "{'year': '2020', 'month': '6', 'day': '23'}"; 
    client.print(data);     // 向服务器发送数据
    Serial.print("Sending Data:  ");
    Serial.println(data);
    
    while (client.connected() || client.available()){ //读取返回值
      if (client.available()){
          String line = client.readStringUntil('\n');
          Serial.print("Receive:  ");
          Serial.println(line);
      }
    }
  }
  else{//连接失败
    Serial.println("Connect Failed");
    Serial.print(WiFi.localIP());
    return;
  }
  client.stop();    //关闭连接
  Serial.println("Connection Closed ");
  Serial.println();
}
```

## 下一步
当如是写入数据库，然后前端展示分析数据了啦，下期再说吧

### 结束py

如果要关闭服务端的py程序，	`Ctrl+C` 即可退出

然后查看`netstat -lnpt`刚才被监听端口对应的进程号 PID，此时应该刚才已经结束了对8888端口的监听，如果出现了下面的情况，表示没有结束py程序，使用`kill -9 PID`即可结束py进程

例如

```bash
[root@ecs tmp]# netstat -lnpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name             
tcp        0      0 127.0.0.1:8888          0.0.0.0:*               LISTEN      11034/python   

[root@ecs tmp]# kill -9 11034
```

### 长连接
[如何让py一直运行在服务器上](https://blog.csdn.net/weixin_43031092/article/details/105564949)

# php

在更

# 参考

- https://www.cnblogs.com/heqiuyong/p/10460150.html
- https://blog.csdn.net/guoxiaozhuang4/article/details/79833574
- https://www.cnblogs.com/alan-babyblog/p/5260156.html
- https://blog.csdn.net/r13929847477/article/details/52336925
- https://blog.csdn.net/qwe24111/article/details/87880917
- https://blog.csdn.net/qq_40604099/article/details/104723009