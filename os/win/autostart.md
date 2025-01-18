# 开机自动启动服务 sc nssm
- date: 2024-12-23
- lastmod: 2025-1-18

目的：有一个 python web 程序，将其设置为开机自启动。工作目录、虚拟环境、启动指令均明确。

NSSM 可以将程序包装为服务，最初我没有尝试它是因为其最新版好多年没更新了，最后试验了一阵子批处理，太影响日常使用了，最后用的 nssm。

## 批处理文件

start.bat 文件内容如下：

```bat
@echo off
cd /d C:\Users\mi6\Do\co\Mr
call env\Scripts\activate
python run.py
```

将批处理文件移动到启动文件夹（按 Win + R，输入 shell:startup，然后按回车），重启电脑测试，结果失败，推测是bat文件执行完毕就退出了，并没有一直在后台运行，我理解是它确实启动了 web 程序，但是很快就有关闭了。。

然后修改方案，原来的 bat 脚本不变，但不移动到启动文件夹中，而是保留在其它文件夹中（源代码目录中），然后将下面的脚本移动到启动文件夹中


```bat
@echo off
start "" cmd /k C:\Users\mi6\Do\co\Mr\start.bat
```

start 命令会启动一个新的命令提示符窗口，然后在新窗口中运行启动脚本，双引号表示标题为空。重启测试表明此方法可正常启动 web 程序，缺点就是会弹出一个命令提示符窗口，黑窗口留在桌面上，感觉不是很方便日常使用，如果关闭这个窗口，程序也就停止了。

[dos命令中的 start cmd命令及参数/k /s 详解](https://blog.csdn.net/modaner/article/details/142885478)
> /k：这个参数告诉start命令在新的窗口中运行指定的命令，并且当命令执行完毕后，保持窗口打开状态，以便用户可以继续输入其他命令

查了下 start 可以加上 /b 来表示后台运行，改成 `start "" /b cmd /k C:\Users\mi6\Do\co\Mr\start.bat` 重启实测还是有黑窗口弹出，改成 `start /b cmd /k C:\Users\mi6\Do\co\Mr\start.bat` 或 `start /b C:\Users\mi6\Do\co\Mr\start.bat` 之后测试运行指令发现不弹窗，但是关闭当前窗口发现服务就挂了。

将 start.bat 最后一行的 python 替换为 pythonw，查询可知这种方式会后台运行 py 程序。在 cmd 里自己测试是行的通的，关闭 cmd 确认程序还在后台运行，配合 `start /b C:\Users\mi6\Do\co\Mr\start.bat` 进行重启测试发现还是有黑框，但是关闭黑框不影响程序运行，然后我想刚才加 start 是启动新窗口后台运行，那 pythonw 已经可以后台了，把 start 去掉，就六指向启动脚本的绝对路径，重启测试表明还是会弹窗黑框，关闭不影响程序在后台运行

使用体验不是很好，时不时黑框闪现导致焦点变化，影响了日常使用。

## 服务
### sc
尝试了一下用 sc 指令来创建服务，有点难搞，尤其指令中涉及空格

1. 打开的终端要有管理员权限，不然 net 没权限启动
2. powershell 里打 sc.exe，不打 exe 的话不识别

下面的测试将 pythonw 换成 python 是一样的结果，且日志文件没见到输出

```powershell
PS C:\Users\mi6> sc.exe create pythonIpMailService binPath= "pythonw.exe C:\Users\mi6\code\main.py" start= auto
[SC] CreateService SUCCESS
PS C:\Users\mi6> sc query pythonIpMailService
PS C:\Users\mi6> sc.exe query pythonIpMailService

SERVICE_NAME: pythonIpMailService
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 1  STOPPED
        WIN32_EXIT_CODE    : 1077  (0x435)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
PS C:\Users\mi6> net start pythonIpMailService
The service is not responding to the control function.

More help is available by typing NET HELPMSG 2186.

PS C:\Users\mi6> sc.exe delete pythonIpMailService
[SC] DeleteService SUCCESS
```

### nssm

因为是 win 11，因此下载的是 nssm-2.24-101-g897c7ad

刚开始的时候，pythonw、python 的结果一样，日志文件都没有内容，很诡异，最后在 nssm 的 I/O 栏目指定输出到文件才看了服务的运行情况，因为日志模块初始化失败，因此之前日志文件没有内容，修改好错误后就能正常运行了。在任务管理器的服务里，也能看到正在运行，而不是出错的暂停。

在服务里删除不掉的服务（之前用 sc 装的，但删了还在），nssm 直接就能删。服务里能看到不同服务对应的进程号，进程号对应的是 nssm 的进程号，而不是 python 的。

```powershell
PS C:\Users\mi6> nssm install pythonIpMailService
Service "pythonIpMailService" installed successfully!
PS C:\Users\mi6> net start pythonIpMailService
The pythonIpMailService service is starting.
The pythonIpMailService service was started successfully.
PS C:\Users\mi6> sc.exe query pythonIpMailService

SERVICE_NAME: pythonIpMailService
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 7  PAUSED
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
PS C:\Users\mi6> nssm status pythonIpMailService
SERVICE_PAUSED
PS C:\Users\mi6> nssm start pythonIpMailService
pythonIpMailService: START: 服务的实例已在运行中。
PS C:\Users\mi6> nssm status pythonIpMailService
SERVICE_PAUSED
PS C:\Users\mi6> nssm restart pythonIpMailService
pythonIpMailService: STOP: 操作成功完成。
pythonIpMailService: Unexpected status SERVICE_PAUSED in response to START control.
PS C:\Users\mi6> nssm status pythonIpMailService
SERVICE_PAUSED
PS C:\Users\mi6> nssm.exe edit pythonIpMailService   # 编辑服务，将 I/O 指定文件，查看运行情况
PS C:\Users\mi6> nssm.exe restart pythonIpMailService
pythonIpMailService: STOP: 操作成功完成。
pythonIpMailService: START: 操作成功完成。
PS C:\Users\mi6> nssm status pythonIpMailService
SERVICE_RUNNING
PS C:\Users\mi6> sc.exe query pythonIpMailService

SERVICE_NAME: pythonIpMailService
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

PS C:\Users\mi> nssm.exe install pythonWebService
Service "pythonWebService" installed successfully!
PS C:\Users\mi> nssm.exe status pythonWebService
SERVICE_STOPPED
PS C:\Users\mi> nssm.exe start pythonWebService
pythonWebService: START: 操作成功完成。
PS C:\Users\mi> nssm.exe status pythonWebService
SERVICE_RUNNING
```

# 参考

- [nssm](https://nssm.cc/download)
