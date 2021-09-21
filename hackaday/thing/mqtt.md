# Mqtt 协议与 mosquitto paho-mqtt 代码案例
- date: 2021-07-18
- lastmod: 2021-07-18

# mqtt

[MQTT](https://mqtt.org/) 全称 Message Queuing Telemetry Transport，中文译作 “消息队列遥测传输协议“。2021 入局物联网必然会听到的一个词汇，在[太极创客 的 零基础入门学用物联网 – MQTT篇](http://www.taichi-maker.com/homepage/esp8266-nodemcu-iot/iot-tuttorial/mqtt-tutorial/)中叙述了其 1999 年诞生在 Andy Stanford-Clark (IBM) 和 Arlen Nipper (Arcom, now Cirrus Link) 手中。

个人理解就是这玩意是一个用在低功耗设备上的”匿名“群聊系统，进入了同一个群聊，只要有个人发消息，其它人都会收到消息。怎么进入同一个群聊呢？订阅（subscribe）相同主题（topic）即可，只要有用户（client）发消息（publish），订阅了该主题的用户都会收到这个消息（payload），包括自身也会收到。为什么说是匿名呢？因为更关注发送了啥，不太关注是谁发的，除非你在主题或者消息中加上了你是谁，否则收到消息的用户不知道消息是谁发的，只知道发了啥。当然这不是严格意义上的匿名，因为在服务器上都是有记录的。

关于版本问题，根据太极的描述，现在 MQTT3.1.1（2014年发布） 还是主流，MQTT5（2019发布）虽然兼容 3,但是设备更新需要硬件成本，所以勇敢的迈出这一步吧。

之前也整过远程数据交换（wifi、socket），都是 C/S 架构， MQTT 也不例外，需要一个服务器作为消息代理（broker）来转发消息，实际代码效果就是比 socket 方便，订阅和发布就完事了。

主题区分大小写、支持空格（不建议使用），不建议使用汉字主题，因为不友好且占据更多字节，避免使用/作为主题的开头。主题也是支持类似正则表达式那样的匹配，更多内容参见[太极创客的MQTT主题进阶](http://www.taichi-maker.com/homepage/esp8266-nodemcu-iot/iot-tuttorial/mqtt-tutorial/mqtt-topics/)

MQTT 还有一个不得不说的服务质量（Qos， quality of services），虽然这个参数会默认设置，但是多学习一下也无妨。

| QoS |  含义     |
|:---:|:-----:|
|   0  | 最多发一次 |
|   1  | 最少发一次 |
|   2  | 保证收一次 |

更多的比如保留消息（retainFlag）、心跳（Keep Alive）、遗嘱（lastWill），[太极创客](http://www.taichi-maker.com/homepage/esp8266-nodemcu-iot/iot-tuttorial/mqtt-tutorial/) 写得可好了。

# mosquitto 案例

mosquitto 是一个比较好上手的 broker，没有太多华丽的东西，朴实无华的模样。

```bash
# 安装 mosquitto，我用的是 manjaro,用第二种方式安装
# Debain x
sudo apt install mosquitto mosquitto-clients
# Arch x
sudo pacman -S mosquitto


# 测试启动，使用默认配置 1883 端口
mosquitto

# 新会话，客户端模拟订阅，-t 之后的内容是主题
mosquitto_sub -v -t topic01
mosquitto_sub -v -t S013/MOT01/RECM


# 新会话，客户端模拟发布 -m 之后的引号里面就是消息了
mosquitto_pub -t topic01 -m hello
mosquitto_pub -t S013/MOT01/RECM -m "DRVZ+1000"
```

## python

需要安装依赖包(pip install paho-mqtt)

```python
# 参考代码地址：
import time
import paho.mqtt.client as mqtt


class Mqtt(object):
    broker = "localhost"  # borker IP
    port = 1883  # borker port
    # name = "raspi1"  # username
    # pwd = "1ipsar"  # password
    ID = "Test"  # mqttclient_ID
    keepalive = 60
    client = None  # mqtt client
    connected_flag = False
    bad_connection_flag = False
    disconnect_flag = False

    def __init__(self):
        self.client = mqtt.Client(self.ID)
        # self.client.username_pw_set(self.name, self.pwd)
        self.client.on_connect = self.on_connect
        self.client.on_message = self.on_message
        self.client.on_disconnect = self.on_disconnect
        self.client.on_subscribe = self.on_subscribe

    def connect(self):
        """connect to the mqtt server."""
        self.client.loop_start()
        try:
            print("MQTTClient: connecting to broker ", self.broker)
            self.client.connect(self.broker, self.port, self.keepalive)
            while not self.connected_flag and not self.bad_connection_flag:
                print("MQTTClient: Waiting for established connection.")
                time.sleep(1)
            if self.bad_connection_flag:
                self.client.loop_stop()
                print(
                    "MQTTClient: had bad-connection. Not trying to connect any further."
                )
        except Exception as err:
            print("MQTTClient: Connection failed")
            print(err)

    def on_disconnect(self, client, userdata, rc):
        print("MQTTClient: disconnect reason: {0}".format)
        self.connected_flag = False
        self.disconnect_flag = True
        self.client.loop_stop()

    def on_connect(self, client, userdata, flags, rc):
        """
        callback func when client connected to the mqtt server.
        rc  0：连接成功
            1：连接被拒绝-协议版本不正确
            2：连接被拒绝-无效的客户端标识符
            3：连接被拒绝-服务器不可用
            4：连接被拒绝-用户名或密码错误
            5：连接被拒绝-未经授权
            6-255：当前未使用。
        """
        if rc == 0:
            self.connected_flag = True
            print(
                "MQTTClient: Connection established successfuly with result code "
                + str(rc)
            )
        else:
            print("MQTTClient: Connection establish ERROR with result code " + str(rc))
            self.bad_connection_flag = True

    def on_subscribe(self, client, userdata, mid, granted_qos):
        """"""
        pass

    def on_message(self, client, userdata, message):
        """
        handle the message when a PUBLISH message is received from the server
        handlers should not set blocked
        """
        print("Time on receive={0}".format(time.asctime(time.localtime(time.time()))))
        print(
            "Received={0}\nTopic={1}\nQOS={2}\nRetain Flag={3}".format(
                message.payload.decode("utf-8"),
                message.topic,
                message.qos,
                message.retain,
            )
        )

    def subscribe(self, topic, **kwargs):
        """
        subscribe topic message
        default set qos to 0
        """
        return self.client.subscribe(topic, **kwargs)

    def unsubscribe(self, topic, properties=None):
        """unsubscribe topic"""
        return self.client.unsubscribe(topic, properties=properties)

    def publish(self, topic, message, **kwargs):
        """publish message through topic"""
        return self.client.publish(topic, message)


if __name__ == "__main__":
    # import 此类不会运行此测试代码
    # 测试代码，可选用 mosquitto_sub -v -t S001/MOT01/RECM 进行接收测试
    mqttClient = Mqtt()
    mqttClient.connect()
    mqttClient.subscribe("S001/MOT01/RECM", qos=1)
    mqttClient.publish("S001/MOT01/RECM", "1000", qos=1, retain=False)

```