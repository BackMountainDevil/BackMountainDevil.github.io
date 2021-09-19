# 解决ESP32 驱动 28BYJ-48 步进电机反向不转之震动的问题
- date: 2021-09-19
- lastmod: 2021-09-19

# 问题描述

Esp 32 在 mqtt 消息下通过 StepMotor 库驱动 28BYJ-48 步进电机，程序运行显示电机逆时针转动正常，顺时针转动只有偶尔会转，大部分时候都是在那里震动不转的情况下用手触摸电机可以感觉到这玩意只在那震动，并没有转。预期结果应该是既能正着转、也能反着转，而且要转的准确。

## 原程序 - 出问题的那个

<details>
<summary>uc2 原步进电机 mqtt 控制程序</summary>

密码、IP 等已经删除
```cpp
// ----------- ----------- ----------- ----------- ----------- -----------
// ESP32 script to accept MQTT commands for UC2-control
// by: Rene Lachmann
// date: 11.09.2019
// based on Arduino-Interface by Rene Lachmann, Xavier Uwurukundu
//----------- ----------- ----------- ----------- ----------- -----------

// ----------------------------------------------------------------------------------------------------------------
//                          INCLUDES
#include <WiFi.h>
#include <PubSubClient.h>
#include <string.h>
#include <vector>
#include <StepMotor.h>
#include "driver/ledc.h"
#include <ctime>
#include <sstream>
// ----------------------------------------------------------------------------------------------------------------
//                          Global Defines
#define MAX_CMD 3
#define MAX_INST 10
#define NCOMMANDS 15
#define MAX_MSG_LEN 40
#define LED_BUILTIN 11
#define LED_FLUO_PIN 26

// ----------------------------------------------------------------------------------------------------------------
//                          Parameters
// ~~~~ Device ~~~~
// create Pseudo-random number with temporal dependent input

// saved in strings, so that later (if implemented) e.g. easily changeable via Bluetooth -> to avoid connection errors
std::string SETUP = "S001";    //"S013";      //S006->Aurelie; S004->Barbora
std::string COMPONENT = "MOT01"; // LAR01 //LED01 // MOT02=x,y // MOT01=z
std::string DEVICE = "ESP32";
std::string DEVICENAME;
std::string CLIENTNAME;
std::string SETUP_INFO;

// ~~~~  Wifi  ~~~~
const char *ssid = "";
const char *password = "";
WiFiClient espClient;
PubSubClient client(espClient);
// ~~~~  MQTT  ~~~~
const char *MQTT_SERVER = "";
const int MQTT_PORT = 1883;
const char *MQTT_CLIENTID;
const char *MQTT_USER;
const char *MQTT_PASS = "23SPE";
const int MQTT_SUBS_QOS = 0;
//const int MAX_CONN = 10; // maximum tries to connect
const unsigned long period = 80000; // 80s
unsigned long time_now = 0;
// topics to listen to
std::string stopicREC = "/" + SETUP + "/" + COMPONENT + "/RECM";
std::string stopicSTATUS = "/" + SETUP + "/" + COMPONENT + "/STAT";
std::string stopicANNOUNCE = "/" + SETUP + "/" + COMPONENT + "/ANNO";
// Deliminators for CMDs (published via payload-string)
const char *delim_inst = "+";
const int delim_len = 1;

// ~~~~ MOTOR ~~~~
StepMotor stepperZ = StepMotor(25, 26, 27, 14); //normally: 25, 26, 27, 14 // 12, 14, 27, 26
StepMotor stepperY = StepMotor(5, 17, 16, 4);
StepMotor stepperX = StepMotor(33, 32, 27, 14); // 27, 25, 32, 4 never connected to same ESP32 as stepperZ -> hence: universally possible

// ~~~~ FLUO ~~~~
int led_fluo_pwm_frequency = 12000;
int led_fluo_pwm_channel = 0;
int led_fluo_pwm_resolution = 8;

// ~~~~ Commands ~~~~
const char *CMD;     //Commands like: PXL -> limited to size of 3?
int *INST[MAX_INST]; //Maximum number of possible instructions =
std::vector<int> INSTS;
std::string CMDS;

const char *COMMANDSET[NCOMMANDS] = {"DRVX", "DRVY", "DRVZ", "FLUO"};
const char *INSTRUCTS[NCOMMANDS] = {"1", "1", "1", "1"};

// ~~~~ FLUO ~~~~
int FLUO_STATUS = 0;
// ----------------------------------------------------------------------------------------------------------------
//                          Additional Functions
// Most stable and efficient way to have the ESP32 be active for input, but still wait (best for Android as well)
void uc2wait(int period)
{
    unsigned long time_now = millis();
    while (millis() < time_now + period)
    {
        //wait approx. [period] ms
    };
}
// Random-string generation from: https://stackoverflow.com/a/12468109
/*std::string random_string(size_t length)
{
    auto randchar = []() -> char {
        const char charset[] =
            "0123456789"
            "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
            "abcdefghijklmnopqrstuvwxyz";
        const size_t max_index = (sizeof(charset) - 1);
        return charset[rand() % max_index];
    };
    std::string str(length, 0);
    std::generate_n(str.begin(), length, randchar);
    return str;
}*/

void setup_device_properties()
{
    //std::time_t result = std::time(nullptr);
    //srand(result); // init randomizer with pseudo-random seed on boot
    //int randnum = rand() % 10000;
    int rand_number = random(1, 100000);
    std::stringstream srn;
    srn << rand_number;
    DEVICENAME = DEVICE + "_" + srn.str(); // random number generated up to macro MAX_RAND
    CLIENTNAME = SETUP + "_" + COMPONENT + "_" + DEVICENAME;
    SETUP_INFO = "This is:" + DEVICENAME + " on /" + SETUP + "/" + COMPONENT + ".";
    MQTT_CLIENTID = DEVICENAME.c_str(); //"S1_MOT2_ESP32"
    //Serial.print("MQTT_CLIENTID=");Serial.println(MQTT_CLIENTID);
    MQTT_USER = DEVICE.c_str();
    Serial.println(SETUP_INFO.c_str());
}

void setup_wifi()
{
    uc2wait(10);
    // We start by connecting to a WiFi network
    Serial.println();
    Serial.print("Device-MAC: ");
    Serial.println(WiFi.macAddress());
    Serial.print("Connecting to ");
    Serial.print(ssid);
    WiFi.setHostname(MQTT_CLIENTID);
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED)
    {
        uc2wait(500);
        Serial.print(".");
    }

    Serial.println("");
    Serial.print("WiFi connected with IP:");
    Serial.println(WiFi.localIP());
}

int separateMessage(byte *message, unsigned int length)
{

    //Serial.println("Seperating Message.");
    //Serial.print("Message=");
    char messageSep[length];
    for (int myc = 0; myc < length; myc++)
    {
        messageSep[myc] = (char)message[myc];
        //Serial.print(messageSep[myc]);
    }
    messageSep[length] = NULL;
    //Serial.println("");
    Serial.print("Mess=");
    std::string mess(messageSep);
    Serial.println(mess.c_str());
    size_t pos = 0;
    int i = 0;
    bool found_cmd = false;
    while ((pos = mess.find(delim_inst)) != std::string::npos)
    {
        if (!found_cmd)
        {
            //Serial.print("CMD-del@");
            //Serial.println(pos);
            CMDS = mess.substr(0, pos);
            //Serial.print("CMDS=");
            CMD = CMDS.c_str();
            //Serial.println(CMD);
            found_cmd = true;
        }
        else
        {
            INSTS.push_back(atoi(mess.substr(0, pos).c_str()));
            //Serial.print("INST[");
            //Serial.print(i);
            //Serial.print("]=");
            //Serial.println(INSTS[i]);
            i++;
        }
        mess.erase(0, pos + delim_len);
    }
    if (!found_cmd)
    {
        //Serial.print("CMD-del@");
        //Serial.println(pos);
        CMDS = mess.substr(0, pos);
        //Serial.print("CMDS=");
        CMD = CMDS.c_str();
        //Serial.println(CMD);
        found_cmd = true;
    }
    else if (mess.length() > 0)
    {
        INSTS.push_back(atoi(mess.substr(0, pos).c_str()));
        //Serial.print("INST[");
        //Serial.print(i);
        //Serial.print("]=");
        //Serial.println(INSTS[i]);
        i++;
    }
    else
    {
        Serial.println("Nothing found...");
    }
    return i;
    mess.clear();
}

void callback(char *topic, byte *message, unsigned int length)
{
    Serial.println("Callback-func called.");
    // test topics
    if (std::string(topic) == stopicREC)
    {
        //Serial.println(topicREC.c_str());
        int nINST = separateMessage(message, length);
        if (strcmp(CMD, COMMANDSET[0]) == 0)
        {
            //Serial.print(INSTS[0] * 10);
            stepperX.Move((int)(INSTS[0] * 10));
        }
        else if (strcmp(CMD, COMMANDSET[1]) == 0)
        {
            stepperY.Move((int)(INSTS[0] * 10));
        }
        else if (strcmp(CMD, COMMANDSET[2]) == 0)
        {
            stepperZ.Move(INSTS[0] * 10);
        }
        else if (strcmp(CMD, COMMANDSET[3]) == 0)
        {
            //analogWrite(FLUO_PIN, INSTS[0]);
            ledcWrite(led_fluo_pwm_channel, INSTS[0]);
        }
        else
        {
            Serial.print("CMD not found.");
        }
    }
    else if (std::string(topic) == stopicSTATUS)
    {
        Serial.println(stopicSTATUS.c_str());
    }
    else if (std::string(topic) == stopicANNOUNCE)
    {
        Serial.println(stopicANNOUNCE.c_str());
    }
    else
    {
        Serial.print("Assortion Error: Nothing found for topic=");
        Serial.println(topic);
    }
    INSTS.clear();
}

void reconnect()
{
    // Loop until we're reconnected
    while (!client.connected())
    {
        Serial.print("MQTT_CLIENTID=");
        Serial.println(MQTT_CLIENTID);
        Serial.print("topicSTATUS=");
        Serial.println(stopicSTATUS.c_str());
        Serial.print("Attempting MQTT connection...");
        // Attempt to connect

        if (client.connect(MQTT_CLIENTID, stopicSTATUS.c_str(), 2, 1, "0"))
        {
            // client.connect(MQTT_CLIENTID,MQTT_USER,MQTT_PASS,"esp32/on",2,1,"off")
            Serial.println("connected");
            // Subscribe
            client.subscribe(stopicREC.c_str());
            client.publish(stopicSTATUS.c_str(), "1");
            client.publish(stopicANNOUNCE.c_str(), SETUP_INFO.c_str());
        }
        else
        {
            Serial.print("failed, rc=");
            Serial.print(client.state());
            Serial.println(" try again in 5 seconds");
            // Wait 5 seconds before retrying
            uc2wait(5000);
        }
    }
}

// ----------------------------------------------------------------------------------------------------------------
//                          SETUP
void setup()
{
    Serial.begin(115200);
    // check for connected motors
    //status = bme.begin();
    setup_device_properties();
    Serial.print("VOID SETUP -> topicSTATUS=");
    Serial.println(stopicSTATUS.c_str());
    setup_wifi();
    Serial.print("Starting to connect MQTT to: ");
    Serial.print(MQTT_SERVER);
    Serial.print(" at port:");
    Serial.println(MQTT_PORT);
    client.setServer(MQTT_SERVER, MQTT_PORT);
    client.setCallback(callback);
    pinMode(LED_BUILTIN, OUTPUT);
    time_now = millis();
    //testCPP();
    // ---> pinMode(LED_BUILTIN, OUTPUT);
    // ---> pinMode(LED_FLUO_PIN, OUTPUT);
    ledcSetup(led_fluo_pwm_channel, led_fluo_pwm_frequency, led_fluo_pwm_resolution);
    ledcAttachPin(LED_FLUO_PIN, led_fluo_pwm_channel);
    ledcWrite(led_fluo_pwm_channel, 20); //analogWrite(FLUO_PIN, 20);
    uc2wait(1000);
    ledcWrite(led_fluo_pwm_channel, 0); //analogWrite(FLUO_PIN, 0,0);
    uc2wait(100);
    stepperX.SetSpeed(10);
    stepperY.SetSpeed(10);
    stepperZ.SetSpeed(100); // 原来是 10
}
// ----------------------------------------------------------------------------------------------------------------
//                          LOOP
void loop()
{
    if (!client.connected())
    {
        reconnect();
    }
    client.loop();
    if (time_now + period < millis())
    {
        client.publish(stopicSTATUS.c_str(), "1");
        time_now = millis();
    }
}
// ----------------------------------------------------------------------------------------------------------------
```
</details>

# 解决过程
搜索后从[【求助】步进电机只震动不转.2017-4-3](https://www.arduino.cn/thread-44283-1-1.html)中知道大概三种可能的因素：电源、接线、速度。自己将速度从 10 提高到 100 问题没解决，排除速度因素。其它参考表明可能是使用的库有问题

> [Arduino 调用Stepper库驱动28BYJ-48步进电机，电机振动不转、无法反方向转的解决办法。半岛铁盒1069 2020-06-29](https://blog.csdn.net/weixin_42358937/article/details/107022433):时序不对，三种解决办法。库不相同，源码不一样；改初始化针脚顺序也不行

> [ESP32 ULN2003驱动步进电机 ，解决电机振动，但不转动问题。青烨慕容 2021-02-04](https://blog.csdn.net/weixin_45488643/article/details/113663882)：和上面一样的库，改库源码

## 电机测试程序
能正常正反运行说明接线没有问题、驱动版正常、电机正常，也就是问题出现在原程序里面了。

<details>
<summary>没有使用库的的步进电机测试程序</summary>

```cpp
/*
 * Stepper_Motor
 * 步进电机驱动，不借助电机库 实现正反转
 * 参考：https://blog.csdn.net/TonyIOT/article/details/88605767
 * 效果：步进电机正转完反转
 */
// Uno
//int input1 = 2; 
//int input2 = 3; 
//int input3 = 4; 
//int input4 = 5; 

// ESP 32
int input1 = 25; 
int input2 = 26; 
int input3 = 27; 
int input4 = 14; 

void setup() {
  //初始化各IO,模式为OUTPUT 输出模式
  pinMode(input1, OUTPUT);
  pinMode(input2, OUTPUT);
  pinMode(input3, OUTPUT);
  pinMode(input4, OUTPUT);
}

void clockwise(int num)
{
  for (int count = 0; count < num; count++)
  {
    digitalWrite(input1, HIGH);
    delay(3);
    digitalWrite(input1, LOW);
    
    digitalWrite(input2, HIGH);
    delay(3);
    digitalWrite(input2, LOW);
  
    digitalWrite(input3, HIGH);
    delay(3);
    digitalWrite(input3, LOW);
  
    digitalWrite(input4, HIGH);
    delay(3);
    digitalWrite(input4, LOW);
  }
}

void anticlockwise(int num)
{
  for (int count = 0; count < num; count++)
  {
    digitalWrite(input4, HIGH);
    delay(3);
    digitalWrite(input4, LOW);
  
    digitalWrite(input3, HIGH);
    delay(3);
    digitalWrite(input3, LOW);
  
    digitalWrite(input2, HIGH);
    delay(3);
    digitalWrite(input2, LOW);
  
    digitalWrite(input1, HIGH);
    delay(3);
    digitalWrite(input1, LOW);
  }
}

void loop() {
  clockwise(512);
  delay(10);
  anticlockwise(512);
}
```
</details>

## 库
StepMotor 这个第三方库（名称显示为 I2C_StepMotor）只有一个示例程序，把四个引脚改了一下发现没法运行。回到 uc2 原程序修改初始化针脚顺序没有正面影响，然后试图从 Arduino/libraries/StepMotor/src/StepMotor.cpp 中试图去修改原代码的时序，发现源码不太具备可读性。于是直接在库管理器搜步进电机库，找到一个 CheapStepper 的库，带的三个案例都完美运行。

于是回到 uc2 原程序中将电机z 全部修改为 CheapStepper 的类型和函数（引入库、初始化、速度设置、转动），xy电机依旧保留旧的代码

```cpp
#include <CheapStepper.h>
// StepMotor stepperZ = StepMotor(25, 26, 27, 14); 
CheapStepper stepperZ (25,26,27,14); 

void setup()
    // stepperZ.SetSpeed(10);
    stepperZ.setRpm(15);

void callback
        if (strcmp(CMD, COMMANDSET[2]) == 0)
        {
            // stepperZ.Move(INSTS[0] * 10);
            if( INSTS[0]>0){
                Serial.println("CW");
                stepperZ.moveDegreesCW(INSTS[0]);
                Serial.print("step position: ");
                Serial.print(stepperZ.getStep());
                Serial.print(" / 4096");
                Serial.println();
                delay(1000);
            }             
            else{
                Serial.println("CCW");
                stepperZ.moveDegreesCCW(0-INSTS[0]);
                Serial.print("step position: ");
                Serial.print(stepperZ.getStep());
                Serial.print(" / 4096");
                Serial.println();
                delay(1000);
            }
        }
```

实际测试表明，指令 moveDegreesCW、moveDegreesCCW 都有执行，但是效果没有变化，还是老样子，以为是 0-INSTS[0] 的问题，就无论是正负，都直接使用 CW,最后测试表明这样会是其陷入无法终止的阻塞转动状态。库自带的案例都没有问题，结合到 uc2 中就有问题，怀疑是自己忘记了啥，就把案例中一些变量也抄过来，但是结果没有变化。

控制变量法整了将近一个下午没有什么正面突破，反倒是推出了不可能结论 - CW 不行，然而事实上案例程序的 CW 是可以运行的。想起之前在 ESP32_ledarr.ino 中的调试中遇到过疯狂重启的状态，最后那个是因为 portDISABLE_INTERRUPTS() 设置了中断引起的，去掉这个设置就行了（和作者讨论后无法判断其为根本原因，但具有实际效果）。在这次的源代码中搜索了一下没有整啥中断啊。

之后怀疑是原电机库和新电机库的冲突，然后把旧电机库、电机xy的代码都移除了，测试结果排除了这种猜测。最后解决的办法是删除了所有我不能一眼看出来作用的代码就好了。

<details>
<summary>最终的代码</summary>

```cpp
```
</details>

# 总结

除了电源、接线、速度、时序等因素之外，发现了一种暂时未整明白的冲突，未知代码的相互冲突，不是逻辑错误、也不是代码语法错误，这种错误的深层原因我也没能整明白。关于 CheapStepper 库函数传参负数的 bug，还需要即使反馈作者，把解决方案合并到主线上。
