# python 安装到入门（解决 pip 缺失、python 弹出 MicrosoftStore）
- date: 2022-03-19
- lastmod: 2022-03-19

## 安装

Linux、Mac 都自带了 python，要是发现没有就使用系统自带的包管理进行安装即可。因此这里说一下 windows 10+ 安装的办法，打开[官方下载页面](https://www.python.org/downloads/windows/)。在 Stable Releases （稳定版） 里边选择一个版本（一般选最新版），然后有 Windows embeddable package 和 Windows installer 的区别。

一般来说，小白请选 Windows installer 64-bit 进行下载安装即可，你过你的电脑是 2015 年之前的再考虑选择 64 还是 32 位。下载之后一路安装下去就可以使用了，安装包会自己修改注册表、登记环境变量、安装 pip等，目前需要注意解决的就是小问题系列的弹出 Microsoft Store 问题。

Windows embeddable package 是一个绿色压缩包，解压即可使用，但是需要自行配置环境变量、自行安装 pip（配置环境变量可以解决，但是 pip 模块我测试了一下装上了但是用不了，所以建议使用上面的 Windows installer 进行安装）。

安装完成之后在终端里敲一下 `python -V` 然后回车，如果输出了 Python 的版本号，说明安装成功。为了获得更好的体验，鉴于当下的国际形势，建议下一步配置一下 pip 镜像，[介里走](/code/python/pip.md)

## 小问题修复系列
### pip 缺失

1. 更新导致的 pip 出问题，即原先有 pip，现在出 bug（No module named 'pip'）

```bash
python -m ensurepip
easy_install pip
```

2. 本身没有安装 pip

UNIX 系列通过包管理器安装 python-pip 大多可以解决。 [pip 文档](https://pip.pypa.io/en/stable/installation/)给出了两种解决方案：

1. python 具有 ensurepip 模块的情况

```bash
python -m ensurepip --upgrade
```

2. 没有 ensurepip 模块下，需要先从从[网络](https://bootstrap.pypa.io/get-pip.py)下载安装脚本，然后再运行脚本进行安装。

```bash
curl https://bootstrap.pypa.io/get-pip.py # 下载脚本
python get-pip.py # 运行脚本进行安装
```

此方案在 win 11 上的 Windows embeddable package Python 3.10.3 测试表面可以安装 pip，但是将路径添加到环境变量之后，测试发现 pip 无法使用

<details>
<summary>详细终端输出如下</summary>

```bash
# 已经把 python-3.10.3-amd64.exe 安装的 py 从环境变量中删除，cmd 测试无法识别 py 符合结果
# 下面的 pip 使用了 清华镜像是因为 python-3.10.3-amd64.exe 安装好的时候我配置了 pip 的镜像源，删除对应 PATH 不会删除用户目录的 pip 配置文件 
C:\Users\kearney\Downloads>python get-pip.py
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Collecting pip
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/4d/16/0a14ca596f30316efd412a60bdfac02a7259bf8673d4d917dc60b9a21812/pip-22.0.4-py3-none-any.whl (2.1 MB)
     ---------------------------------------- 2.1/2.1 MB 358.7 kB/s eta 0:00:00
Collecting setuptools
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/7c/5b/3d92b9f0f7ca1645cba48c080b54fe7d8b1033a4e5720091d1631c4266db/setuptools-60.10.0-py3-none-any.whl (1.1 MB)
     ---------------------------------------- 1.1/1.1 MB 453.9 kB/s eta 0:00:00
Collecting wheel
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/27/d6/003e593296a85fd6ed616ed962795b2f87709c3eee2bca4f6d0fe55c6d00/wheel-0.37.1-py2.py3-none-any.whl (35 kB)
Installing collected packages: wheel, setuptools, pip
Installing collected packages: wheel, setuptools, pip
  WARNING: The script wheel.exe is installed in 'C:\ProgramFile\python-3.10.3-embed-amd64\Scripts' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
  WARNING: The scripts pip.exe, pip3.10.exe and pip3.exe are installed in 'C:\ProgramFile\python-3.10.3-embed-amd64\Scripts' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed pip-22.0.4 setuptools-60.10.0 wheel-0.37.1

# 把 C:\ProgramFile\python-3.10.3-embed-amd64\Scripts 加入环境变量 PATH
# 新开 cmd 测试 pip

C:\Users\kearney>pip
Traceback (most recent call last):
  File "runpy.py", line 196, in _run_module_as_main
  File "runpy.py", line 86, in _run_code
  File "C:\ProgramFile\python-3.10.3-embed-amd64\Scripts\pip.exe\__main__.py", line 4, in <module>
ModuleNotFoundError: No module named 'pip'

C:\Users\kearney>python -m pip
C:\ProgramFile\python-3.10.3-embed-amd64\python.exe: No module named pip

C:\Users\kearney>python -m ensurepip --upgrade
C:\ProgramFile\python-3.10.3-embed-amd64\python.exe: No module named ensurepip
```
</details>

- [Python pip 安装与使用](https://www.runoob.com/w3cnote/python-pip-install-usage.html)

### python 弹出 Microsoft Store

- [解决windows终端里运行python或python3会自动弹出MicrosoftStore的两种办法](https://blog.csdn.net/weixin_43031092/article/details/109196437)：解决方案二选一
> 1. 按下win键，输入管理应用，然后打开“管理应用执行别名”，关闭 python 对应的 ‘应用安装程序’
> 2. 删除环境变量 PATH 中的 %USERPROFILE%\AppData\Local\Microsoft\WindowsApps