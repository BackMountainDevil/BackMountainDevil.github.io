# 多系统下的蓝牙设备共用配对问题之 LTK、EDIV、ERAND.以 Manjaro、Debian、Windows10 为例
- date: 2020-09-6
- lastmod: 2022-10-05

# 简介

多系统下的蓝牙设备配对问题的解决办法，以 Windows 10 + Manjaro + Debian 为例。尤其适合双（多）系统下的蓝牙设备不支持多配对的时候（有的蓝牙设备是可以进行多主机配对的），同理适合 win + linux 双系统的用户，mac 同理（本人暂无法验证，见参考中大佬的解决办法）。

本文不涉及 Mac 以及需要 CSRK 的设备（原因是我没有这样的设备，有需要的可以去参考中看相关资料）。

## 大纲

1. linux 下进行蓝牙配对
2. windwnows 下进行蓝牙配对
3. 用 PStool/chntpw 导出 windows 的蓝牙配置信息
4. 用 sudo 修改 linux 的蓝牙配置信息

# 实操
## linux 下进行蓝牙配对

我这里使用的是 Manjaro,配对了两个蓝牙设备，鼠标（MiMouse）和键盘（KB），并且使用上是正常的。在系统托盘中的蓝牙管理可点击对应的设备展开更多信息可以看到设备对于的物理地址分别是 EB、D1 打头（方便起见，这个需要记一下，太长且不可复制，记一下开头就行，后面介绍咋复制）。

暂时我们记录的信息如下所示：

```
设备名称：鼠标 MiMouse
Linux 蓝牙 ID：EB

设备名称：键盘 KB
Linux 蓝牙 ID：D1
```

![蓝牙设备的地址信息](https://img-blog.csdnimg.cn/e4234976bb7b4410a0808da114fe16ef.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)

## windows 下进行蓝牙配对

配对蓝牙设备这个没啥好说的哈，配对完之后我们需要记录一下配对蓝牙设备的设备 ID。方法是在蓝牙设置界面中的 “相关设置” 选择 “更多蓝牙选项”；在弹出的 “蓝牙设置“ 窗口中的菜单栏选择 “硬件” ，在设备中选中刚才配对的某个设备，如键盘 KB；然后点击 “属性“；咋对应的 “属性窗口的菜单栏“ 选择 “事件“，在底部的 “信息” 展示中先是着 “已配置设备 BTHLE\DEV_id\“，这个 DEV_id 中的 id 就是该蓝牙设备的设备 ID。记录下来，之后对另一个蓝牙设备也这样操作获取其 ID.

![蓝牙配置-等多蓝牙选项](https://img-blog.csdnimg.cn/2bff68356c6d4d338c3f3ccae2e5c437.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_17,color_FFFFFF,t_70,g_se,x_16#pic_center)

![蓝牙属性获取-硬件-属性-事件-信息](https://img-blog.csdnimg.cn/9643f7f0fbf842ceaa38de75cb00690a.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


这里需要记住哪一个地址对应哪一个设备，因为在提取信息的时候没法从信息里知道其对应设备的名称（这一点 Linux 做的比较好，配置信息里有设备名称）。暂时我们记录的信息如下所示：

```
设备名称：鼠标 MiMouse
Linux 蓝牙 ID：EB
win 设备 ID：e35d79da5272

设备名称：键盘 KB
Linux 蓝牙 ID：D1
win 设备 ID：d118ff200048
```

## 方法一：用 PStool 导出 windows 的蓝牙配置信息

从 [PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) 官网下载其压缩包，提取其中的 `PsExec64.exe` 到 `c://windows/system32`，然后鼠标右键菜单标志（默认菜单栏最左边），选择以管理员身份打开 powershell，输入`psexec64.exe -si regedit` 后回车会打开注册表编辑器。（32位PC就提取PsExec.exe，powershell的命令改为`psexec.exe -si regedit`）。

在注册表上方的地址栏粘贴下面一行后回车转到蓝牙配置： `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\BTHPORT\Parameters\Keys\` 

可以在左侧看到 Keys 下面有一个文件夹，这个文件夹代表你的电脑，点击展开它，里面的文件夹的名称刚好对应我们在上一步获取到的蓝牙设备的设备 ID。

![d11的全部参数](https://img-blog.csdnimg.cn/a177121fa1d449548418a86c3a141707.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)

![e35的全部参数](https://img-blog.csdnimg.cn/82b6d448b82841e7a108ecf8b2762326.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_19,color_FFFFFF,t_70,g_se,x_16#pic_center)

![复制 ERand 的十进制数据示意图，右键选中修改](https://img-blog.csdnimg.cn/202103291315078.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

![复制 ERand 的十进制数据示意图，勾选十进制基数后复制](https://img-blog.csdnimg.cn/20210329131517432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

选中一个蓝牙设备的文件夹，右键导出，保存为 txt 文件，名称为这个设备的 ID，我们需要复制并记录其中的 LTK。之后获取 EDIV 和 ERAND 的十进制数值，数值在右侧可以看到，但不可以复制。复制的解决办法是右键这个参数的名称，点击修改，然后在弹出的窗口显示的是十六进制，在其右侧勾选十进制就会自动转换为十进制，复制保存下来。

复制 LTK 的时候需要注意两侧的不需要复制，只需要复制中间的就行，复制得到的结果和注册表中右侧显示的应该是一样的。

![导出示意图，注意保存类型为 txt](https://img-blog.csdnimg.cn/2021032913121865.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

![LIK 复制例子](https://img-blog.csdnimg.cn/4c7e2a9433c1462196163d749081f9d8.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)


暂时我们记录的信息如下所示：

```
设备名称：鼠标 MiMouse
Linux 蓝牙 ID：EB
win 设备 ID：e35d79da5272
LTK：04 29 6f 8e 03 bb 5c 13 - f1 4c 6d ba fe 45 d8 a4
EDIV：33544
ERAND：8341617009900789008

设备名称：键盘 KB
Linux 蓝牙 ID：D1
win 设备 ID：d118ff200048
LTK：0f d1 d2 cf 06 33 24 d1 - 47 4e a5 71 e3 3a 09 2d
EDIV：12016
ERAND：12806037061999603757
```

### 格式转换

Linux 下的配置格式和刚才获取的有一丢丢差距，大小写和空格等，对 RK、LTK 需要转大写并且去掉其中的空格和短线;对设备 ID 则需要转大写并手动插入英文冒号。

这是两个简单在线工具网站，[字母大写](https://www.iamwawa.cn/daxiaoxie.html) 和 [去空格](http://www.esjson.com/delSpace.html)，去短线和加入冒号需要自己操作，还没发现好工具。

暂时我们记录的信息如下所示：

```
设备名称：鼠标 MiMouse
Linux 蓝牙 ID：EB
win 设备 ID：EB:FE:DC:C0:E6:CE
LTK：04296F8E03BB5C13F14C6DBAFE45D8A4
EDIV：33544
ERAND：8341617009900789008

设备名称：键盘 KB
Linux 蓝牙 ID：D1
win 设备 ID：D1:18:FF:20:00:48
LTK：0FD1D2CF063324D1474EA571E33A092D
EDIV：12016
ERAND：12806037061999603757
```

## 方法二：在 linux 上用 chntpw 导出 windows 的蓝牙配置信息

如果已使用方法一，可以跳过本小节。首先挂载 C 盘，找到 C 盘下的路径 `Windows/System32/config/`。需要安装软件包 chntpw。此次无法通过 mac 查看设备名称，因此前面在 win 配对的时候就需要记录下设备对应的大概 mac 地址了。

只需要使用 hex 读取 LTK、ERand，EDIV 在 ls 的时候就可以读取到十进制数据，用 hex 读 EDIV 的话，还得先反转再转换成十进制（不反转直接进制转换出来是不对的，尝试了疯狂断连）

```bash
# 切换路径，此步骤每个人路径大多不同
$ cd /run/media/kearney/C/Windows/System32/config/
# 使用 chntpyw 读取注册表
$ chntpw -e SYSTEM
> ls
  <ControlSet001>
> cd ControlSet001\Services\BTHPORT\Parameters\Keys
> ls
  <28cdc4babb2c>

> cd 28cdc4babb2c
> ls
  <d132ff200048>
  <fae0e5592901>
> cd d132ff200048
> ls
    16  3 REG_BINARY         <LTK>
     8  b REG_QWORD          <ERand>
     4  4 REG_DWORD          <EDIV>                 55245 [0xd7cd]
     
> hex LTK
:00000  32 28 41 A7 3C D9 66 06 20 E2 50 D5 AE 6B 3E FF 2(A.<.f. .P..k>.
> hex IRK
:00000  A4 D4 B0 77 37 46 A7 DF 0C CF C0 91 67 51 36 85 ...w7F......gQ6.
> hex ERand
:00000  BE 58 C3 26 99 F9 DF 1D                         .X.&....

> cd ..
> cd fae0e5592901
  size     type              value name             [value if type DWORD]
    16  3 REG_BINARY         <LTK>
     8  b REG_QWORD          <ERand>
     4  4 REG_DWORD          <EDIV>                 41045 [0xa055]
> hex LTK
:00000  77 F7 F7 D1 38 6E 07 17 95 89 A2 00 2D D8 44 97 w...8n......-.D.
> hex ERand
:00000  86 C2 7B D2 B7 F7 AE 7B                         ..{....{
```

LTK、ERand 转换的 py3 代码，发现 wiki 上显示的代码有点问题，再深入发现是文档渲染的问题，然而找了下格式帮助文档也没有找到正确的格式，就先在 discussion 里提出了。

```python
# LTK 去除空格
>>> '77 F7 F7 D1 38 6E 07 17 95 89 A2 00 2D D8 44 97'.replace(' ', '')
# ERand 反转 去空格 换进制
>>> ERand='86 C2 7B D2 B7 F7 AE 7B'
>>> ERand=list(reversed(ERand.strip().split()))
>>> int("".join(ERand), 16)
```

后面的步骤一致，改 LongTermKey 下的 LTK ERand Eiv，然后修改路径为新 mac 地址即可。键盘、鼠标测试通过。

## 用 root 权限修改 linux 的蓝牙配置信息

重新进入到 Manjaro。到这里我们还需要本机的蓝牙 ID 和 Linux 系统下对应的蓝牙设备 ID，分别在下面第 1、2 条命令获取。

```bash
# 获取本机的蓝牙 ID
$ sudo ls /var/lib/bluetooth
28:CD:C4:BA:BB:2C

# 获取已经配对的蓝牙设备 ID，后面那串 28:...:2C 是上面获取到的“本机的蓝牙 ID” 
$ sudo ls /var/lib/bluetooth/28:CD:C4:BA:BB:2C
cache  D1:17:FF:20:00:48  EB:FE:DC:C0:E6:CE  settings
# 从上面的输出就知道鼠标和键盘的具体ID,而且可以复制
```

到这里我们可以更新小本本中 Linux 蓝牙 ID 并增加本机 ID了。现在我们记录的信息如下所示：

```
本机的蓝牙 ID: 28:CD:C4:BA:BB:2C
设备名称：鼠标 MiMouse
Linux 蓝牙 ID：EB:FE:DC:C0:E6:CE
win 设备 ID：E3:5D:79:DA:52:72
LTK：04296F8E03BB5C13F14C6DBAFE45D8A4
EDIV：33544
ERAND：8341617009900789008

设备名称：键盘 KB
Linux 蓝牙 ID：D1:17:FF:20:00:48
win 设备 ID：D1:18:FF:20:00:48
LTK：0FD1D2CF063324D1474EA571E33A092D
EDIV：12016
ERAND：12806037061999603757
```

现在我们该更改 Linux 的蓝牙配置信息了。其中要修改的配置有

| win| linux | 备注 |
|--|--|--|
| LTK | LongTermKey |转大写，去掉空格、短线 |
| EDIV  | LTK.EDiv  |十进制 |
| ERand  | LTK.Rand |十进制 |

有的设备配置文件有 [SlaveLongTermKey]，其也有 Ediv、Rand 的属性，不要动它，只需要改 [LongTermKey] 下的 Ediv、Rand。不然结果就是疯狂的连接与掉线。。。

```bash
# 以下指令需要修改对应的路径才能使用

# 修改鼠标的配置信息
$ sudo nano /var/lib/bluetooth/28:CD:C4:BA:BB:2C/EB:FE:DC:C0:E6:CE/info
# 更新配置的蓝牙 ID，这一步很重要。相当于重命名
# 将旧的 “Linux 蓝牙 ID“ 修改为 “win 设备 ID”
$ sudo mv /var/lib/bluetooth/28:CD:C4:BA:BB:2C/EB:FE:DC:C0:E6:CE/ /var/lib/bluetooth/28:CD:C4:BA:BB:2C/E3:5D:79:DA:52:72

# 修改键盘的配置信息
$ sudo nano /var/lib/bluetooth/28:CD:C4:BA:BB:2C/D1:17:FF:20:00:48/info
$ sudo mv /var/lib/bluetooth/28:CD:C4:BA:BB:2C/D1:17:FF:20:00:48/ /var/lib/bluetooth/28:CD:C4:BA:BB:2C/D1:18:FF:20:00:48

# 重启蓝牙服务，使设置生效
$ sudo systemctl restart bluetooth 

# 将 Manjaro 的蓝牙配置复制到 Debian 中，多 Linux 系统下可以这样操作，亲测有效。（双系统可以跳过）
# Debian 的路径与你挂载的路径和名称有关 - /run/media/kearney/Debian/
$ sudo cp -r /var/lib/bluetooth/28:CD:C4:BA:BB:2C/ /run/media/kearney/Debian/var/lib/bluetooth/
```

![键盘原配置](https://img-blog.csdnimg.cn/df5dcb291e90410793260e6622ba5030.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

![键盘更新之后的配置](https://img-blog.csdnimg.cn/fbb2425081854c0caef18ab58cae6703.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

![鼠标原配置](https://img-blog.csdnimg.cn/ba58fd1d709b490196e699a7fd4fa773.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

![鼠标更新之后的配置](https://img-blog.csdnimg.cn/82dfb50329b543f9b4b042f9af0cefa6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

## 开机自启

linux 的蓝牙默认是登陆后在启动服务的，也就是在输入密码的时候你还是得用电脑的键盘（如笔记本键盘），蓝牙键盘是暂时不能使用的，为了让在登陆的时候也能使用蓝牙键盘，如 [Bluetooth - ArchWiKi](https://wiki.archlinux.org/title/Bluetooth) 中 Auto power-on after boot 小节说的一样

```bash
sudo nano /etc/bluetooth/main.conf
# ctrl + w 启动搜索 AutoEnable
# 去掉 AutoEnable 前面的井号，把它的值改为 true
```

# 总结

在最后的步骤 “用 root 权限修改 linux 的蓝牙配置信息” 中，其实可以用 root 账户登陆桌面，操作就会是图形化操作，当然也可以在管理员用户下用 `su root` 切换到 root 账户，这样不需要记录本机 ID 和 Linux 蓝牙 ID,因为可以用 tab 自动补全，但想了一下能用 sudo 解决的话，就先用着吧见仁见智。

平实主要是用 Manjaro, Debian 没法用 AUR,下载了也涉及依赖问题，社区推的 DUR 还不成熟，先留着，之后估计会砍掉 Debian 装 BSD 玩一下。

上面用 pstool 的办法看起来似乎没有 chntpw 那么方便，有空再翻译一下 Arch wiki。

# 参考

- [win10 ubuntu16 双系统共用蓝牙鼠标 10km 2017-03-10](https://blog.csdn.net/10km/article/details/61201268):linux + win PS tool
- [解决方案：Win10和Linux双系统配对蓝牙设备 jiaolu☞ 2020-04-30](https://blog.csdn.net/jiaolu295/article/details/105821487): win + linux
- [ MacOS、Windows、Linux蓝牙4.0鼠标共用配对。2018-4-13 。jamyu](http://bbs.pcbeta.com/viewthread-1781802-1-1.html):从这里得知上面的两个办法（linkkey）不适合我的蓝牙设备，我的有好多参数
  > 这个帖子的方法只能用在蓝牙3.0的鼠标上，也就是使用 Link-key ID。蓝牙LE的不同的。  
  基本规则是：  
  IRK（Windows）-<转大写>-IdentityResolvingKey（Linux）—<HEX反转>—IRK（MacOS）  
  LTK（Windows）—<转大写>—LongTermKey(Linux)—<HEX直接带入>—LTK(MacOS)  
  ERAND（Windows）—<转DEC>—Rand（linux）—<HEX直接带入>—RAND(MacOS)  
  EDIV(Windows)—<转DEC>—EDIV(Linux)—<HEX反转>—EDIV（MacOS）  
  有CSRK的可以参考一下http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1839501
- [Bluetooth - ArchWiKi](https://wiki.archlinux.org/title/Bluetooth):Dual boot pairing 中提供了另外一种办法（chntpw），以及开机自启动蓝牙
- [Bluetooth mouse (简体中文) - ArchWiKi 2020-08-04 双系统鼠标配对问题](https://wiki.archlinux.org/index.php/Bluetooth_mouse_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
- [Bluetooth Pairing on Dual Boot of Windows & Linux Mint/Ubuntu - Stop having to Pair Devices。 Asked 5 years](https://unix.stackexchange.com/questions/255509/bluetooth-pairing-on-dual-boot-of-windows-linux-mint-ubuntu-stop-having-to-p):还是linkkey.。。不过里面有一个可以不使用pstool的办法
- [Dual Boot Bluetooth LE (low energy) device pairing。Asked 3 years](https://unix.stackexchange.com/questions/402488/dual-boot-bluetooth-le-low-energy-device-pairing/413831#413831)
- [ How to pair a Low Energy (LE) Bluetooth device in dual boot with Windows & Linux  Thursday, September 18, 2014](https://console.systems/2014/09/how-to-pair-low-energy-le-bluetooth.html)
- [双系统蓝牙键盘的共享配对解决办法的简要步骤：win + arch～IRK、LTK、ERand、EDIV.Kearney form An idea 2021-03-29](https://blog.csdn.net/weixin_43031092/article/details/115298442)：以前写的旧文
- [多双系统下蓝牙键盘鼠标的共享配对问题解决办法：win + debian + arch～IRK、LTK、ERand、EDIV、CSRK.Kearney form An idea 2021-03-29](https://blog.csdn.net/weixin_43031092/article/details/115281263)：：以前写的旧文
