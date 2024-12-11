# Windows Sandbox 初探
- date: 2024-12-10

Windows Sandbox 是 Windows 10/11 专业版/企业版内置的隔离环境。很适合测试新软件、测试URL、运行存疑的软件。

前置条件、安装方式、使用方法可参阅[Windows 沙盒](https://learn.microsoft.com/zh-cn/windows/security/application-security/application-isolation/windows-sandbox/windows-sandbox-overview)。其中 Windows 沙盒默认启用网络连接

win 11 pro 符合前置条件，开启沙盒功能后需要重启操作系统，重启后在“开始”菜单上找到并选择“Windows Sandbox”点击次运行，首次运行会更新，可能更新失败转而使用经典沙盒。启动之后就像个虚拟机，有单独桌面环境。

直接复制粘贴 LibCyber-V2.7.31-x64.exe 到沙盒里，安装后启动，在沙盒里启动科学上网服务，结果沙盒、宿主机的网络均无法连接谷歌搜索。关闭沙盒再启动后会发现刚才安装的软件已经被清空了。

# LibCyber

文件名：LibCyber-V2.7.31-x64.exe  
SHA256：cf35c6f429236f2f4eace6a1457552474c85326d41d841463215c37e939a1cfc  
SHA1：641450c28fbca5664f5eda57a90550c864187302  
MD5：af09a1f1793005e04ebc923c2b6e4095  
文件大小：77.34 MB (81100629)  
文件类型：pe  

www.virscan.org 有 44 个引擎未检出，有 3 引擎检出，如下所示：
1. Xvirus: Malware
2. F-Prot: W32/Felix:EX:007!Eldorado
3. GDATA: Win32:Evo-gen

www.virustotal.com 69个厂家中有一家 Bkav Pro 检测为 W32.AIDetectMalware；这个网站的详情栏目还能看到不同字段的地址、用了哪一些 dll；行为栏目用了沙盒分析，可以看到该软件写入、打开了哪些文件、注册表、进程、服务、IP、恶意行为。

由于个人信息安全水平不足，无法判断上面的报毒信息具体是个什么情况。可以确定的是在沙盒里运行该软件时，该软件无法发挥其科学上网的功能。

# 参考

[如何将文件传输到 Windows Sandbox](https://cn.windows-office.net/?p=13846): 复制并粘贴、共享文件夹
