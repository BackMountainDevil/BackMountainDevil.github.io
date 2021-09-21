# ESP IDF 开发框架构建 与 ADF 语音识别框架尝试 - esp 32s
- date: 2021-06-04
- lastmod: 2021-06-04

# ESP-ADF
[ESP32 语音框架开发文档](https://docs.espressif.com/projects/esp-adf/en/latest/get-started/index.html#step-1-set-up-esp-idf)

## ESP IDF
### 环境准备
根据[ vscode-esp-idf-extension/docs/tutorial/install.md ](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/install.md)所描述，我这里又是 Arch Linux,因此需要先安装 python 3.5+, pip, git, camke, Ninja-build。这些东东具体如何安装这里不做叙述，因系统而异。比如我的是 `sudo pacman -S ninja`。

### 安装 ESP-IDF
根据 [ESP-IDF 文档](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/get-started/index.html#get-started-connect) 的描述，我可以选择安装 ESP-IDF 或者插件，我这里安装的是它的 VS Code 插件，安装过程在文档里也有（选择ESPRESSIF的服务器不见得比 Github 要快，将近一个小时的下载安装。。操作系统我都能安好几个了）

```bash
$ printenv | grep IDF
IDF_PYTHON_ENV_PATH=/home/kearney/.espressif/python_env/idf4.2_py3.9_env
IDF_PATH=/home/kearney/opt/esp/esp-idf/
IDF_TARGET=esp32
IDF_COMPONENT_MANAGER=1
```
### 环境变量

```bash
nano ~/.bashrc 
# 粘贴
alias get_idf='. /home/kearney/opt/esp/esp-idf/export.sh'

source ~/.bashrc 
```

>现在您可以在任何终端窗口中运行 get_idf 来设置或刷新 esp-idf 环境。
这里不建议您直接将 export.sh 添加到 shell 的配置文件。因为这会导致在每个终端会话中都激活 IDF 虚拟环境（包括无需使用 IDF 的情况），从而破坏使用虚拟环境的目的，并可能影响其他软件的使用。

## 安装 ESP-ADF
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

# 参考
- [ESP-IDF 文档](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/get-started/index.html)
- [ vscode-esp-idf-extension/docs/tutorial/install.md ](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/install.md)
- [ESP32 语音框架开发文档](https://docs.espressif.com/projects/esp-adf/en/latest/get-started/index.html#step-1-set-up-esp-idf)