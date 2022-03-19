# python 安装到入门（解决 pip 缺失、python 弹出 MicrosoftStore）
- date: 2022-03-19
- lastmod: 2022-03-19

## 安装

Linux、Mac 都自带了 python，要是发现没有就使用系统自带的包管理进行安装即可。因此这里说一下 windows 10+ 安装的办法，打开[官方下载页面](https://www.python.org/downloads/windows/)。在 Stable Releases （稳定版） 里边选择一个版本（一般选最新版），然后有 Windows embeddable package 和 Windows installer 的区别。

一般来说，小白请选 Windows installer 64-bit 进行下载安装即可，你过你的电脑是 2015 年之前的再考虑选择 64 还是 32 位。下载之后一路安装下去就可以使用了，安装包会自己修改注册表、登记环境变量、安装 pip等，目前需要注意解决的就是小问题系列的弹出 Microsoft Store 问题。

Windows embeddable package 是一个绿色压缩包，解压即可使用，但是需要自行配置环境变量、自行安装 pip（其它隐藏问题暂未发现）。

安装完成之后在终端里敲一下 `python -V` 然后回车，如果输出了 Python 的版本号，说明安装成功。为了获得更好的体验，鉴于当下的国际形势，建议下一步配置一下 pip 镜像，[介里走](./pip.md)

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

- [Python pip 安装与使用](https://www.runoob.com/w3cnote/python-pip-install-usage.html)

### python 弹出 Microsoft Store

- [解决windows终端里运行python或python3会自动弹出MicrosoftStore的两种办法](https://blog.csdn.net/weixin_43031092/article/details/109196437?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164767578516782248545616%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=164767578516782248545616&biz_id=0)：解决方案二选一
> 1. 按下win键，输入管理应用，然后打开“管理应用执行别名”，关闭 python 对应的 ‘应用安装程序’
> 2. 删除环境变量 PATH 中的 %USERPROFILE%\AppData\Local\Microsoft\WindowsApps