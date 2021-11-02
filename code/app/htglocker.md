# HTG Locker && locker.bat 分析 与 80 G的数据恢复记录
- date: 2021-05-01
- lastmod: 2021-05-01

# 简介

好些年前不晓得在哪里看到一个简单粗暴的‘加密’批处理程序，然后如法炮制使用了三年多。。然后某天数据都不见了（不一定是该程序的问题，有可能是硬盘被我折腾出毛病了），恢复过程后面会说

# 批处理代码

创建一个文本文件，粘贴进去下面的代码，修改其中的‘密码’为你想设置的密码，保存之后修改文件名为 lock.bat （后缀是 bat 就行），需要注意的是其中密码那一行 `if NOT %pass%== 密码 goto FAIL` 的密码最好别整中文，假设为 adszcx.123，就修改为  `if NOT %pass%== adszcx.123 goto FAIL` ，不要在 adszcx.123 两端加单引号，否则密码就变成了  'adszcx.123' 。

```bash
cls
@ECHO OFF
title Folder Private
if EXIST "HTG Locker" goto UNLOCK
if NOT EXIST Private goto MDLOCKER
:CONFIRM
echo 你确定要加密隐藏Private文件夹吗？(Y/N)
set/p "cho=>"
if %cho%==Y goto LOCK
if %cho%==y goto LOCK
if %cho%==n goto END
if %cho%==N goto END
echo Invalid choice.
goto CONFIRM

:LOCK
ren Private "HTG Locker"
attrib +h +s "HTG Locker"
echo Folder locked
goto End

:UNLOCK
echo 输入密码来解锁文件夹
set/p "pass=>"
if NOT %pass%== 密码 goto FAIL
attrib -h -s "HTG Locker"
ren "HTG Locker" Private
echo Folder Unlocked successfully
goto End

:FAIL
echo Invalid password
goto end

:MDLOCKER
md Private
echo Private created successfully
goto End
:End
```

## 解释

双击运行 ta，就会生成一个 Private 文件夹，然后把需要‘加密’的文件剪切进去，然此双击运行这个程序就会把 Private 文件夹更名为 HTG Locker，然后被‘隐藏’起来了，想要‘解密’就再次运行该程序，输入密码即可。

该批处理程序通过文件名分别是加密还是解密，如果名称是 Private ，就加密（将文件属性变为系统文件并添加隐藏属性），如果是 HTG Locker 就进入解密（取消系统文件属性、取消隐藏属性）。也就是通俗的说，实际上这些文件并没有被加密，只是被归类为系统文件而已，而且在 windows 中系统文件是受保护的，默认不予显示。

其中 md 指建立文件夹，echo 为输出，ren 是更改名称，attrib 则是修改文件属性。

## 优点

- 简单方便

## 缺点

- 不可更改文件名（不是 Private 就是 HTG Locker）

在资源管理器设置的菜单中 -> 查看 -> 选项 -> 查看, 取消勾选“隐藏受保护的操作系统文件”，就可以看到了，还能直接查看访问。或者直接在地址栏里加上 `\HTG Locker` 再回车也可以访问。
- 不安全‘加密’

密码打开 bat 文件就可以查看和修改。

## 常见问题

1. 黑色窗口（控制台）为什么显示乱码？我该怎么办？

乱码是常见的编码问题，例如本文采用 UTF-8 编码，国区的 windwos 采用 gbk 编码。用记事本打开 bat 文件，将编码格式修改为 gbk 后保存即可。

2. 彻底删除了 bat 文件怎么办？

再建一个就行。误删的也是如此。是在不行就数据恢复这个 bat 文件。

# 数据恢复

这是一块跟了我十多年的机械硬盘(NTSC)了，和新的硬盘（西数蓝盘 2T）比起来轻一点，噪音也大一点，但是 ta 一直顽强拼搏到现在，说不准那天就崩盘了，等硬盘降价我就换东芝 P300 PMR。结果上个月拍了些照片、视频想存进去做个记录，发现之前拍的照片、视频都不见了。。。 bat 文件也没有了，新建 bat 也于是无补。尝试了一些 attrib 组合命令来取消全盘的文件属性也不行。。。

使用 windws 自带的磁盘扫描结果是这样的,但是也不敢随意修复哇，数据都没拿出来呢。。。
>检查基本文件系统结构...  
由于自愈命令失败，正在中止 "chkdsk /scan": 0xc0000102  
需要使用 "chkdsk /f" 才能修复卷

还有一点很奇怪的是，虽然文件不在了，取消勾选“隐藏受保护的操作系统文件”，然后显示出的全部隐藏文件里也没有。。。剩下的全部文件占了 30 G，但是磁盘属性显示已使用空间为 110 G,于是我大胆推测这些文件还在，但是因为某些问题导致无法识别和读取。用 Filelight 也只看到了 30 G的文件。这个时候第六感告诉我不要往里面写入数据了。

先后使用了 Recuva、DiskGenius 也没有找到这些珍贵的资料。遂封存这个硬盘开始学习数据恢复、数据拯救、分区原理....

## 原理

打一个比方，存数据就是给房间里放家具，然后给挂上门牌号，这样电脑就凭着这个门牌号找数据。删除数据并不是说真的把数据删除了，只是把门牌号删除了，然后多出一间可以出租的房子。因为把家具倒腾出去会消耗更多的时间，操作系统本着简洁原则不会这样做的，毕竟下次重新存数据会重新布置整个房子。虽然门牌号没有了，但是可以通过观察家具的摆放方式、装潢风格来判断这是啥文件，也就可以恢复门牌号了。不过如果这间房子刚被你取下门牌，随后恰好又被重新装修的话，那估计凉凉。

所以重点是数据不一定真的被删除了，而且当你意识到要数据恢复时就不要写入文件了，很有可能就把要恢复的文件真的删除（覆盖）了。

## 软件

由于我这个数据还算不上核弹、超算研发等高级涉密资料，因此采取的都是非收费版本的软件， Recuva、DiskGenius 确实找到了一些文件，但不是我想找回的那些，深度扫描也没有收获，无意之中得知了 R-Studio，一下子就给我找到了，恢复了大概一小时（ USB 2.0 的速度问题），注意恢复的数据先暂存到别的硬盘，后面再转移；如果直接恢复到原硬盘，很有可能恢复着恢复着文件就没了。

### 相关资源

- [Recuva 官网](https://www.ccleaner.com/recuva)
- DiskGenius [免安装 windwos 版 提取码: 8s2e](https://pan.baidu.com/s/1m781Dd7OCRklMVO2ENlK5g)  [官网](https://www.diskgenius.cn/)
- R-Studio [免安装 windwos 版 提取码: g23y](https://pan.baidu.com/s/1ffnPTxOdZyDOknQ8s-efvg)（需使用管理员身份运行）    [官网](https://www.rstudio.com)

# 参考

[如何给文件夹加密 腿疼姐姐](https://zhuanlan.zhihu.com/p/163026529)

[attrib 百度百科](https://baike.baidu.com/item/attrib)
