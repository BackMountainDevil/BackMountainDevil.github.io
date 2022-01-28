# ESP IDF 开发框架构建尝试 - esp32
- date: 2021-06-04
- lastmod: 2022-01-28

# ESP IDF
AUR 中有 esp-idf 的包可以直接一键安装，不过包里面是会从 github 中进行下载，所以没有科学超能力的用户使用还是会安装失败。

## 环境准备
根据[ vscode-esp-idf-extension/docs/tutorial/install.md ](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/install.md)所描述，我这里又是 Arch Linux,因此需要先安装 python 3.5+, pip, git, camke, Ninja-build。这些东东具体如何安装这里不做叙述，因系统而异。比如我的是 `sudo pacman -S --needed gcc git make flex bison gperf python-pip cmake ninja ccache dfu-util libusb`。

## 安装 ESP-IDF
根据 [ESP-IDF 文档](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/get-started/index.html#get-started-connect) 的描述，我可以选择安装 ESP-IDF 或者插件，我这里安装的是命令行版本的，插件版安装过程在文档里有，不过插件初始化配置速度靠运气了（选择ESPRESSIF的服务器不见得比 Github 要快，将近一个小时的下载安装。。操作系统我都能安好几个了）。

在[esp-idf 的代码库](https://github.com/espressif/esp-idf)的 release 中给出了两种比较便捷的办法，一种是直接克隆最近一个稳定版，一个是下载[压缩包（v4.4）](https://dl.espressif.com/github_assets/espressif/esp-idf/releases/download/v4.4/esp-idf-v4.4.zip).折腾了那么多次，压缩包是最方便的办法。

```bash
# 切换目录，之后会安装到 /opt/esp，用户需要拥有对该目录的读写权限。不懂就换到用户目录下，如 ～/opt/esp
sudo chown -hR /opt/esp
cd /opt/esp
# 推荐下载压缩包解压(我也使用这招)，因为 clone 容易在最后出 bug
# github 的方法比较方便，但是需要科学能力，以下针对的是 v4.4 版本的
git clone -b v4.4 --recursive https://github.com/espressif/esp-idf.git esp-idf-v4.4

cd esp-idf-v4.4/  # 进入目录

./install.sh  # 开始安装
```

下一步是设置[环境变量](#环境变量)

### gitee 笨办法安装 esp-idf

这个很久之前的了。作为一个记录吧，现在有压缩包直接下载方便多了。这里应该用不上了。主要是有些子模块在 github 上，所以蛮头疼的。

```bash
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
# 在最后粘贴
$ alias get_idf='. /opt/esp/esp-idf-v4.4/export.sh'

$ source ~/.bashrc
$ get_idf

Done! You can now compile ESP-IDF projects.
Go to the project directory and run:

  idf.py build
```

> 现在您可以在任何终端窗口中运行 get_idf 来设置或刷新 esp-idf 环境。  
这里不建议您直接将 export.sh 添加到 shell 的配置文件。因为这会导致在每个终端会话中都激活 IDF 虚拟环境（包括无需使用 IDF 的情况），从而破坏使用虚拟环境的目的，并可能影响其他软件的使用。
## 设置默认开发板

为了避免每次打开工程都要敲 `idf.py set-target esp32`，[这里](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/api-guides/build-system.html#selecting-idf-target)提供了一些办法，设置环境变量 `export IDF_TARGET=esp32`。或者设置 CMake 变量。对于特定项目，可以把 `CONFIG_IDF_TARGE`T 的值加入项目里的配置文件 `sdkconfig.defaults`。

但是我手头上都是 esp32 的板子，那还是设置环境变量舒服些。

> alias get_idf='export IDF_TARGET=esp32 && . /opt/esp/esp-idf-v4.4/export.sh'

# 使用

在 IDF 安装目录中有配套案例（example），拷贝一个 helloworld 出来试试

```bash
# 导入 IDF 环境变量，启动虚拟环境
get_idf
# 连接开发板
# 设置“目标”芯片，清除并初始化项目之前的编译和配置（如有）,
# 芯片组有 esp32,esp32s2,esp32s3,esp32c3 (idf.py --list-targets)
idf.py set-target esp32
# 配置，正常情况下会启动一个蓝白界面进行操作，否则就是安装出了问题
idf.py menuconfig
# 编译工程
idf.py build
# 烧录到设备，/dev/ttyUSB0 替换为实际的端口标识，如 /dev/ttyUSB1, COM1
idf.py -p /dev/ttyUSB0 flash
# 启动串口监视器，正确退出组合按键是 Ctrl+]
idf.py monitor
```

最后两部可以凑起来一起整 idf.py -p PORT flash monitor，实际上测试过不指明端口也是可以的 idf.py flash monitor

运行结果案例，大概十秒之后会重启并重复这一过程

```bash
Hello world!
This is esp32 chip with 2 CPU core(s), WiFi/BT/BLE, silicon revision 1, 2MB external flash
Minimum free heap size: 291424 bytes
```


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