# Sandboxie-Plus 沙盒初体验
- date: 2024-12-11

该软件仅适用于 Windows 系统。

# 使用方式

[下载页面](https://sandboxie-plus.com/downloads/) 提供32/64位版本的安装软件。下载速度较慢。

一般用户只能新建标准沙盒，其他类型的沙盒需要赞助。然后在宿主机中右键想要安装的软件，单击“在沙盒中运行”，然后就会开始安装了。

创建快捷方式：右键点击沙盒，选择 沙盒内容 - 创建快捷方式，然后点击确定，然后在左上角弹出来的窗口选择要创建的对象。

# 应用
## 微信

3.9.12.17 x64 能够安装和登录，发消息测试没问题，就是启动和退出时会报错。

避免登录状态、聊天记录被删除，需要设置沙盒选项-文件选项，两次勾选“保护此沙盒免受删除或清空”。

启动时报错，标题为 “WeChatAppEx.exe - DLL初始化实败” 的弹窗，提示 “动态链接库 C:\ProgramFiles\Sandboxie-Plus\SbieDll.dll 初始化失败。过程非正常终止。”。与此同时 Sandboxie-Plus 通知内显示了一条信息 “WeChatAppEx.exe: SBIE2181 LowLeveI.dII 绕行无法将 SbieDll.dll 加载到目标进程中。”

点击确定关闭上面的弹窗，Sandboxie-Plus 通知内就会多出 17 条信息，内容开头大都是 “WeChatAppEx.exe: SBIE2112 对象无法访问:\Sessions”。

然后点击登录，手机上确认后，登录成功，就是 “动态链接库 SbieDll.dll 初始化失败” 的错误弹窗又出现了好多次。虽然报错有一些吧，不过测试收发消息倒是没啥大问题。


## LibCyber

由于在 windows sandbox 测试 LibCyber-V2.7.31-x64 发现运行后网络没有联动（可登录可连接无谷歌），于是尝试使用 Sandboxie-Plus 沙盒环境。测试表明在 Sandboxie-Plus Version 1.15.3 的标准沙盒里，软件运行后无法开启连接（可登录不可连接），报错内容为“连接失败”。sbp 通知有两类，一类是 PID ID号: SBIE2205 未实现该服务 WMI IWbemServices，第二类是 powershell.exe : SBIE2205 未实现该服务: ConsoleInit(C00000D4)。

# refer

[App+1 | 用 Sandboxie-Plus，管住「不听话」的 Windows 软件 爱拼安小匠 05/20](https://sspai.com/post/88759)

[利用Sandboxie 限制某程序访问指定目录的方法](https://nanzt.info/8982.html)

[将 QQ、TIM 运行在 Sandboxie 之中](https://www.xeath.cc/2021/02/07/archives-379/)： 不涉及微信

[使用sandboxie-plus隔离QQ和微信 2022-07-25](https://blog.ztmzzz.win/posts/%E4%BD%BF%E7%94%A8sandboxie-plus%E9%9A%94%E7%A6%BBqq%E5%92%8C%E5%BE%AE%E4%BF%A1/)：不好读懂
