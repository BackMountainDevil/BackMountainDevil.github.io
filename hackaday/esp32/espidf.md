# ESP IDF 开发框架构建 与 ADF 语音识别框架尝试 - esp 32s
- date: 2021-06-04
- lastmod: 2021-06-04

# ESP IDF
AUR 中有 esp-idf 的包可以直接一键安装，不过包里面是会从 github 中进行下载，所以没有科学超能力的用户使用还是会安装失败。

## 环境准备
根据[ vscode-esp-idf-extension/docs/tutorial/install.md ](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/install.md)所描述，我这里又是 Arch Linux,因此需要先安装 python 3.5+, pip, git, camke, Ninja-build。这些东东具体如何安装这里不做叙述，因系统而异。比如我的是 `sudo pacman -S --needed gcc git make flex bison gperf python-pip cmake ninja ccache dfu-util libusb`。

## 安装 ESP-IDF
根据 [ESP-IDF 文档](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/get-started/index.html#get-started-connect) 的描述，我可以选择安装 ESP-IDF 或者插件，我这里安装的是它的 VS Code 插件，安装过程在文档里也有（选择ESPRESSIF的服务器不见得比 Github 要快，将近一个小时的下载安装。。操作系统我都能安好几个了）

```bash
# 切换目录，之后会安装到 /opt/esp-idf，其实可以直接下载最新版的 zip 包，但是这样不方便使用 git 进行版本升级
$ cd /opt
# github 的方法比较方便，但是需要科学能力
# $ git clone --recursive https://github.com/espressif/esp-idf.git

# 这里使用 gitee 镜像的办法，由于子模块的问题会需要多几步
$ sudo git clone https://gitee.com/EspressifSystems/esp-gitee-tools.git
$ sudo git clone https://gitee.com/EspressifSystems/esp-idf.git #  --recursive 在 gitee 需要验证

$ cd esp-gitee-tools
$ export EGT_PATH=$(pwd)
$ cd ../esp-idf
$ sudo $EGT_PATH/submodule-update.sh
# 切换至稳定版本
$ sudo git checkout v4.3
# 开始安装
$ ./install.sh
Detecting the Python interpreter
Checking "python" ...
Python 3.9.7
"python" has been detected
Installing ESP-IDF tools
Installing tools: xtensa-esp32-elf, xtensa-esp32s2-elf, xtensa-esp32s3-elf, riscv32-esp-elf, esp32ulp-elf, esp32s2ulp-elf, openocd-esp32
```

## 环境变量

```bash
$ nano ~/.bashrc 
# 粘贴
alias get_idf='. /opt/esp-idf/export.sh'

$ source ~/.bashrc 
$ get_idf
$ printenv | grep IDF
IDF_PYTHON_ENV_PATH=/home/kearney/.espressif/python_env/idf4.3_py3.9_env
IDF_PATH=/opt/esp-idf
IDF_TOOLS_EXPORT_CMD=/opt/esp-idf/export.sh
IDF_TOOLS_INSTALL_CMD=/opt/esp-idf/install.sh
```

> 现在您可以在任何终端窗口中运行 get_idf 来设置或刷新 esp-idf 环境。  
这里不建议您直接将 export.sh 添加到 shell 的配置文件。因为这会导致在每个终端会话中都激活 IDF 虚拟环境（包括无需使用 IDF 的情况），从而破坏使用虚拟环境的目的，并可能影响其他软件的使用。

# ESP-ADF

[ESP32 语音框架开发文档](https://docs.espressif.com/projects/esp-adf/en/latest/get-started/index.html#step-1-set-up-esp-idf)

待完善（此部分真板测试未通过）

```bash
# 我更换了目录
cd /home/kearney/opt/esp
# github 换成 gitee 镜像
git clone --recursive git@gitee.com:EspressifSystems/esp-adf.git
```

# 使用

下面这些步骤除了敲命令之外，IDF for VS Code 插件会方便一些，点点按钮就完事。

```bash
# 导入 IDF 环境变量，启动虚拟环境
get_idf
# ADF 可以直接加入环境变量
export ADF_PATH=/home/kearney/opt/esp/esp-adf

# 连接开发板
# 设置“目标”芯片，清除并初始化项目之前的编译和配置（如有）
idf.py set-target esp32
# 配置
idf.py menuconfig
# 编译工程
idf.py build
# 烧录到设备，DevPort 替换为实际的端口标识
idf.py -p DevPort flash
idf.py monitor
```

## 设置默认开发板

为了避免每次打开工程都要敲 `idf.py set-target esp32`，[这里](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/api-guides/build-system.html#selecting-idf-target)提供了一些办法，设置环境变量 `export IDF_TARGET=esp32`。或者设置 CMake 变量。对于特定项目，可以把 `CONFIG_IDF_TARGE`T 的值加入项目里的配置文件 `sdkconfig.defaults`。

但是我手头上都是 esp32 的板子，那还是设置环境变量舒服些。

# Demo
## IDF
在 IDF 安装目录中有配套案例（example），拷贝一个 helloworld 出来整进 esp32s 开发板里，一气呵成。
## ADF
同样的 ADF 也有配套案例，我拷贝一个简单的 声音检测 vad 出来，无法进入配置界面（menuconfig）...提示找不到啥文件，缺啥模块，根据错误提示去 IDF/ADF 的 github 仓库找，整下来放到对应位置，这下子终于可以进入蓝白的配置界面了。

然后 Build 的时候遇到了新的错误，这个错误暂时不知道咋解决-.- (得亏我还想整一下离线语音识别呢)
```bash
/home/kearney/opt/esp/esp-adf/components/audio_stream/algorithm_stream.c:169:42: error: implicit declaration of function 'aec_pro_create'; did you mean 'aec_create'? [-Werror=implicit-function-declaration]
         _success &= ((algo->aec_handle = aec_pro_create(AEC_FRAME_LENGTH_MS, ALGORITHM_STREAM_DEFAULT_CHANNEL, ALGORITHM_STREAM_DEFAULT_AEC_MODE)) != NULL);
                                          ^~~~~~~~~~~~~~
                                          aec_create
/home/kearney/opt/esp/esp-adf/components/audio_stream/algorithm_stream.c:169:40: warning: assignment to 'void *' from 'int' makes pointer from integer without a cast [-Wint-conversion]
         _success &= ((algo->aec_handle = aec_pro_create(AEC_FRAME_LENGTH_MS, ALGORITHM_STREAM_DEFAULT_CHANNEL, ALGORITHM_STREAM_DEFAULT_AEC_MODE)) != NULL);
                                        ^
cc1: some warnings being treated as errors
ninja: build stopped: subcommand failed.
The terminal process "/bin/bash '-c', 'cmake --build .'" failed to launch (exit code: 1).
```

期待这块 ESP-32S 配上 INMP441 全向麦克风能跑起来，不想辜负黄花907第一次开光。

# FAQ

1. RuntimeError: Couldn't detect Bash version, shell completion is not supported.

这个问题发生在激活 idf 环境的时候，环境正常激活了，但也报出了这一串异常。

```bash
$ get_idf
Setting IDF_PATH to '/opt/esp-idf'
Detecting the Python interpreter
Checking "python" ...
Python 3.9.7
"python" has been detected
Adding ESP-IDF tools to PATH...
Using Python interpreter in /home/kearney/.espressif/python_env/idf4.3_py3.9_env/bin/python
Checking if Python packages are up to date...
Python requirements from /opt/esp-idf/requirements.txt are satisfied.
Added the following directories to PATH:
  /opt/esp-idf/components/esptool_py/esptool
  /opt/esp-idf/components/espcoredump
  /opt/esp-idf/components/partition_table
  /opt/esp-idf/components/app_update
  /home/kearney/.espressif/tools/xtensa-esp32-elf/esp-2020r3-8.4.0/xtensa-esp32-elf/bin
  /home/kearney/.espressif/tools/xtensa-esp32s2-elf/esp-2020r3-8.4.0/xtensa-esp32s2-elf/bin
  /home/kearney/.espressif/tools/xtensa-esp32s3-elf/esp-2020r3-8.4.0/xtensa-esp32s3-elf/bin
  /home/kearney/.espressif/tools/riscv32-esp-elf/1.24.0.123_64eb9ff-8.4.0/riscv32-esp-elf/bin
  /home/kearney/.espressif/tools/esp32ulp-elf/2.28.51-esp-20191205/esp32ulp-elf-binutils/bin
  /home/kearney/.espressif/tools/esp32s2ulp-elf/2.28.51-esp-20191205/esp32s2ulp-elf-binutils/bin
  /home/kearney/.espressif/tools/openocd-esp32/v0.10.0-esp32-20210401/openocd-esp32/bin
  /home/kearney/.espressif/python_env/idf4.3_py3.9_env/bin
  /opt/esp-idf/tools
Done! You can now compile ESP-IDF projects.
Go to the project directory and run:

  idf.py build

Traceback (most recent call last):
  File "/opt/esp-idf/tools/idf.py", line 812, in <module>
    main()
  File "/opt/esp-idf/tools/idf.py", line 730, in main
    cli(sys.argv[1:], prog_name=PROG, complete_var='_IDF.PY_COMPLETE')
  File "/home/kearney/.espressif/python_env/idf4.3_py3.9_env/lib/python3.9/site-packages/click/core.py", line 1137, in __call__
    return self.main(*args, **kwargs)
  File "/home/kearney/.espressif/python_env/idf4.3_py3.9_env/lib/python3.9/site-packages/click/core.py", line 1057, in main
    self._main_shell_completion(extra, prog_name, complete_var)
  File "/home/kearney/.espressif/python_env/idf4.3_py3.9_env/lib/python3.9/site-packages/click/core.py", line 1132, in _main_shell_completion
    rv = shell_complete(self, ctx_args, prog_name, complete_var, instruction)
  File "/home/kearney/.espressif/python_env/idf4.3_py3.9_env/lib/python3.9/site-packages/click/shell_completion.py", line 45, in shell_complete
    echo(comp.source())
  File "/home/kearney/.espressif/python_env/idf4.3_py3.9_env/lib/python3.9/site-packages/click/shell_completion.py", line 324, in source
    self._check_version()
  File "/home/kearney/.espressif/python_env/idf4.3_py3.9_env/lib/python3.9/site-packages/click/shell_completion.py", line 319, in _check_version
    raise RuntimeError(
RuntimeError: Couldn't detect Bash version, shell completion is not supported.
```

[【问题解决】RuntimeError: Couldn‘t detect Bash version, shell completion is not supported.. 麟枫 2021-08-06 ](https://blog.csdn.net/linfengXBB/article/details/119451686)给出了一种临时解决办法： 修改shell_completion.py 里305行那的两行代码

```python
output = subprocess.run(["bash", "-c", "echo $BASH_VERSION"], stdout=subprocess.PIPE)
match = re.search(r"(\d)\.(\d)\.\d", output.stdout.decode())
```

原来的代码（shell_completion.py L302-L321）是这样的

```bash
    def _check_version(self) -> None:
        import subprocess

        output = subprocess.run(["bash", "--version"], stdout=subprocess.PIPE)
        match = re.search(r"version (\d)\.(\d)\.\d", output.stdout.decode())

        if match is not None:
            major, minor = match.groups()

            if major < "4" or major == "4" and minor < "4":
                raise RuntimeError(
                    _(
                        "Shell completion is not supported for Bash"
                        " versions older than 4.4."
                    )
                )
        else:
            raise RuntimeError(
                _("Couldn't detect Bash version, shell completion is not supported.")
            )
```

可以从源码中看出这个问题抛出异常是因为没有检测到 bash 的版本号，然而我已经安装了最新版的 bash，而且通过 L305 行的 `bash --version` 是可以获取到版本号的，再看一下代码抛出异常的原因是 match 为 None,也就是说 L306 没有检测到 bash 的版本号，为什么呢？？通过下面的输出对比一下就知道了，它的版本号校验是通过检测 version 后面的数字，然而我使用的是中文环境，所以它一定不会检测到版本。

```bash
$ bash --version
GNU bash，版本 5.1.8(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2020 Free Software Foundation, Inc.
许可证 GPLv3+: GNU GPL 许可证第三版或者更新版本 <http://gnu.org/licenses/gpl.html>

本软件是自由软件，您可以自由地更改和重新发布。
在法律许可的情况下特此明示，本软件不提供任何担保。
```

那解决办法其实也比较简单，需要将这个语言环境变量设置为英文，短期方案是临时设置语言环境变量为英文 `export LANG=en_US`；中长期方案一是修改系统语言为英文，中长期方案二是把环境修改设置到启动命令中 `alias get_idf='export LANG=en_US && . /opt/esp-idf/export.sh'`，永久解决方案是反馈该问题到[上游](https://github.com/pallets/click/pull/1942),主线已经合并解决方案，下一个稳定版发布即可永久解决该问题。

# 参考

- [ESP-IDF 文档](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/get-started/index.html)
- [ vscode-esp-idf-extension/docs/tutorial/install.md ](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/install.md)
- [乐鑫开源 / esp-gitee-tools ](https://gitee.com/EspressifSystems/esp-gitee-tools/tree/master)
- [乐鑫开源 / esp-idf](https://gitee.com/EspressifSystems/esp-idf)
- [ESP32 语音框架开发文档](https://docs.espressif.com/projects/esp-adf/en/latest/get-started/index.html#step-1-set-up-esp-idf)
- [ RuntimeError when env variable LANG is set to anything except for en #1967 ](https://github.com/pallets/click/issues/1967)
- [ Bash version detection fails with certain locales #1940 ](https://github.com/pallets/click/issues/1940)