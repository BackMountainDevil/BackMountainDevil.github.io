# Python 的版本与虚拟环境管理
- date: 2021-08-17
- lastmod: 2021-08-24

# 虚拟环境 - virtual environment

为什么需要虚拟环境？这样做有什么好处和弊端？

不同项目之间的包版本可能不同，需要相互隔离，互不影响，其次还需要方便二次使用，一般会配一个 requirement.txt，这样别人就可以一键安装需要的包，而不是先运行一下，看报错缺哪个再装再重复。

平时通过 pip 安装的包都会装到一个用户目录下，当新建一个工程，又装了一些包，这个工程给别人的时候，如果不告诉别人你用了哪些包和对于哪些版本，很有可能这个程序就跑不起来或者运行异常，更糟糕的是你也不完全知道自己究竟装了哪些包。。用 pip list 一看，哎呀妈呀，这么多，那些才是哦。所以需要有个东西能记录下来；然后就是不同的工程对包的版本可能有不同的要求，因此需要隔离开来，不能都装在一个用户目录下。

目前使用 pipenv 的弊端有：flask 的 env 异常，需要重进虚拟环境；镜像源需要每次手动改文件；在树莓派上安装 pipenv 失败了几次；在不同 py 版本中的兼容问题（自己开发和部署的 py 版本不一致，在部署环境编译安装一个 py 可不简单）

包管理器（Package management），常见的有 pip、conda（Anaconda、Miniconda）

# 虚拟环境包

这里讲述的是针对同一个 py 版本的包环境管理，推荐使用 venv 或者 poetry；如果需要管理不同版本的 py，参照 pyenv、conda（已知存在一些问题不便移植）。

| 名称       | py 支持版本 | 备注                                                                                        |
| ---------- | ----------- | ------------------------------------------------------------------------------------------- |
| pipenv     | 2.7, 3.4+   | ![GitHub User's stars](https://img.shields.io/github/stars/pypa/pipenv?style=social)          |
| poetry     | 2.7, 3.5+   | ![GitHub User's stars](https://img.shields.io/github/stars/python-poetry/poetry?style=social) |
| pyvenv     | 3.3, 3.4    | py 3.6+ 已经放弃该包，转用 venv                                                             |
| venv       | 3.3+        | 不支持 py2；入选 py 官方文档                                                                |
| virtualenv | 2.7, 3.5+   | ![GitHub User's stars](https://img.shields.io/github/stars/pypa/virtualenv?style=social)      |

为了增加对 py2 的支持，可以选用 pipenv 或者 virtualenv，但是 2021 年了，py2 已经消失在我的世界之中，因此打算转投 venv,因为它不需要额外安装，会作为 py3 的一个模块就安装好了。pipenv 的 star 虽多，但是 issue 也不少，目前 500 多个。

## pipenv

可以通过 pip 来安装这个工具包。需要注意的是，尽管 pip 已经配置了镜像源，但是 pipenv 的并不会自动使用该配置，而是使用默认的官方源，需要自己修改两个配置文件里面的"sources"的 "url"的值修改为 `https://pypi.tuna.tsinghua.edu.cn/simple`。推荐使用 **poetry** 替代该产品。

优点：

1. 可以区分生产包和开发包（非必需的 dev）
2. 导出依赖包会加入 pypi 的 url，即把镜像也写入其中

缺点：

1. lock 降低了速度
2. 导出依赖包目前无法单独导出开发包，会一起导出（待修复）
3. 不能直接使用 pip 操作，install 等都需要通过 pipenv，不然无法记录
4. 每次都需要自己修改 pypi 的镜像源（待修复）

pipenv 的速度据说比较慢；但可以简单的分享给别人用 pipenv 进行环境配置，所有的虚拟环境都存放在一个目录下（./home/kearney/.local/share/virtualenvs），但是时间长了也就记不住装了几个虚拟环境，只好打开那个隐藏目录看一看要删除哪一些。

<details>
<summary>查看常用命令</summary>

```cpp
# 创建工程文件夹、进入工程内
mkdir fb
cd fb
# 创建虚拟环境，如果有 pipfile 就会安装对应的包，dev 类的包需要加参数 --dev 才会安装
pipenv install
# 激活虚拟环境，没有就会创建
pipenv shell

# 搞事情，敲代码，996加班
pipenv install flask	# 只是假装摸鱼
pipenv install watchdog --dev # 将看门狗包归类为dev
# 下班，记得commit push哦

# 退出虚拟环境
exit

# 查看帮助
pipenv -h
# 安装包 - "packagename"
pipenv isntall "packagename"
# 安装包，归类为开发模式，需要指定dev模式才会安装该模块
pipenv isntall "packagename" --dev
# 使用python2创建环境
pipenv --two  
# 使用python3创建环境  
pipenv --three
# 指定某个Python版本创建环境         
pipenv --python 3.7   
# 指定某个位置的Python创建环境  
pipenv --python <path/to/python>   
# 卸载包
pipenv uninstall "packagename"
# 卸载所有包
pipenv uninstall --all
# 查看包依赖
pipenv graph
# 生成lockfile
pipenv lock
# 将依赖列表写入文件中
pipenv lock -r > requirements.txt
# 将测试依赖列表写入文件中（实测和上面一个一样，阿巴阿巴，也就是这个指令还没有实现，文档先行）
pipenv lock -r --dev > requirements-dev.txt
# 运行py文件
pipenv run python "temp.py"

# 删除当前所指的虚拟环境
pipenv --rm
# 卸载pipenv
pip uninstall pipenv
```

</details>

## venv

不需要额外安装，默认自带在 py 3.4+ 中，所以不支持 py2。

优点：

1. 可以直接使用 pip 操作，而不影响主环境
2. 会复制主环境中的 pip 配置，无需额外配置镜像源

缺点：

1. 导出依赖包不会加入 pypi 的 url（待改进）
2. 无法区分生产包和开发包
3. 需要自行导出依赖包以便重建
4. 不同平台的激活指令有所区别，powershell 是 <venv>\Scripts\Activate.ps1。更多请看[venv 文档](https://docs.python.org/zh-cn/3/library/venv.html)

```bash
mkdir weqq  # 假设项目叫做 weqq
cd weqq # 切换到项目目录中
# 在项目中创建虚拟环境 env （通常会以 venv 或者  env 作为虚拟环境名称），可将生成的 env 目录加入 `.gitignore`
python -m venv env 
# 激活环境， env 为上面创建的环境名称，可变
source env/bin/activate
# 测试
(env) $ pip install flask
# 导出包和版本到文件中
(env) $ pip freeze > requirements.txt
# 退出虚拟环境
deactivate

# 删除虚拟环境，谨慎使用这条命令！！！在文件管理器中删除也是一样的
rm -r env
```

如何快速重建这个环境呢？获取别人的依赖文件，建立一个虚拟环境（不建也行）并激活，然后一行搞定

`pip install -r requirements.txt`

需要注意的是，当虚拟环境所在的路径发生变化（如上级目录更名），需要重建虚拟环境，否则激活虚拟环境后会出现异常情况（如终端显示了虚拟环境名在前，但是 pip 依旧是全局的），这种情况的原因是虚拟环境中写了初始路径（如 env/bin/activate 中的 VIRTUAL_ENV），这个问题同样也会出现在 pipenv 中

通常来说，在 windows 执行自己的脚本是默认禁止的，根据[Win10 pipenv无法加载文件Scripts\Activate.ps1](https://blog.csdn.net/weixin_43031092/article/details/113412866)的结果，打开 pwoershell，输入 Set-ExecutionPolicy -Scope CurrentUser RemoteSigned 即可。

### bug

可以从下面看出虚拟环境中的 pip 自动复制的主环境中的配置（赞），但是虚拟环境中的 pip 版本和主环境中的版本不一致。

<details>
<summary>点击查看详情</summary>

```bash
[kearney@arch django]$ python -V
Python 3.9.6
[kearney@arch django]$ pip -V
pip 20.3.4 from /usr/lib/python3.9/site-packages/pip (python 3.9)
[kearney@arch django]$ python -m venv env
[kearney@arch django]$ source env/bin/activate
(env) [kearney@arch django]$ python -V
Python 3.9.6
(env) [kearney@arch django]$ pip -V
pip 21.1.3 from /home/kearney/Documents/code/python/django/env/lib/python3.9/site-packages/pip (python 3.9)
(env) [kearney@arch django]$ pip config list
global.index-url='https://pypi.tuna.tsinghua.edu.cn/simple'
install.trusted-host='https://pypi.tuna.tsinghua.edu.cn'
(env) [kearney@arch django]$ pip list
Package    Version
---------- -------
pip        21.1.3
setuptools 56.0.0
WARNING: You are using pip version 21.1.3; however, version 21.2.4 is available.
You should consider upgrading via the '/home/kearney/Documents/code/python/django/env/bin/python -m pip install --upgrade pip' command.
(env) [kearney@arch django]$ deactivate 

# 主环境升级 pip
$ python -m pip install --upgrade pip
$ pip -V
pip 21.2.4 from /home/kearney/.local/lib/python3.9/site-packages/pip (python 3.9)
[kearney@arch django]$ python -m venv env
[kearney@arch django]$ source env/bin/activate
# 可以看到虚拟环境里的 pip 版本并没有照抄主环境的新版 pip，重启之后该问题依然可以复现
(env) [kearney@arch django]$ pip list
Package    Version
---------- -------
pip        21.1.3
setuptools 56.0.0
WARNING: You are using pip version 21.1.3; however, version 21.2.4 is available.
You should consider upgrading via the '/home/kearney/Documents/code/python/django/env/bin/python -m pip install --upgrade pip' command.
```

</details>

## poetry

自称是 Python 依赖管理工具。在 repo 的 README这么写到，一是 1.2 版之后将不支持 2.7 和 3.5，二是关于 pipenv。我只能说服，不服你就改嘛，代码就在那里。

> Note: Python 2.7 and 3.5 will no longer be supported in the next feature release (1.2). You should consider updating your Python version to a supported one.
> What about Pipenv?
> In short: I do not like the CLI it provides, or some of the decisions made, and I think we can make a better and more intuitive one. Here are a few things that I don't like.

Arch 大法中这样安装：`sudo pacman -S python-poetry`，这种方式不支持 `poetry self update` 指令，官方 linux 版的安装方式为 `curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/install-poetry.py | python -`。

优点：

1. 可以自定义虚拟环境的位置，默认统一到一个隐藏目录下，也可以修改为当前目录下
2. 可以区分生产包和开发包（非必需的 dev）

缺点：

1. 在 code 中使用 add 的时候 resolving dependencies 的时间大约是在 konsole 中的五倍，flask 大约 30s
2. 依赖问题解决有问题，见 bug（可能是我安装方式问题）
3. 镜像需要每次手动修改（主要是防火墙问题）
4. pip 依旧是全局的 pip

<details>
<summary>查看常用命令</summary>

```bash
# 初始化环境，生成 pyproject.toml 文件，不懂就回车
poetry init
# 根据  pyproject.toml 文件建立虚拟环境
poetry install
# 只安装非 development 环境的依赖，一般部署时使用
poetry install --no-dev
# 激活并进入虚拟环境 ，退出是 exit
poetry shell
# 这个激活方式和 venv 的挺像，退出是 deactivate
source {path_to_venv}/bin/activate

poetry add flask ：安装最新稳定版本的flask
poetry add pytest --dev : 指定为开发依赖，会写到pyproject.toml中的[tool.poetry.dev-dependencies]区域
poetry add flask=2.22.0 : 指定具体的版本

# 删除包 numpy, 会将依赖一并删除
poetry remove numpy

# 删除虚拟环境，我的py是39,但是打39会报错，会保留 pyproject.toml 
poetry env remove python
poetry env remove python3

# 运行 python 脚本
poetry run python your_script.py
```
</details>

### bug

<details>
<summary>查看详情</summary>

```bash
(django-ulKECtDq-py3.9) [kearney@arch django]$ poetry run python -V
Python 3.9.6
(django-ulKECtDq-py3.9) [kearney@arch django]$ poetry add numpy
Using version ^1.21.2 for numpy

Updating dependencies
Resolving dependencies... (3.5s)

  SolverProblemError

  The current project's Python requirement (>=3.9,<4.0) is not compatible with some of the required packages Python requirement:
    - numpy requires Python >=3.7,<3.11, so it will not be satisfied for Python >=3.11,<4.0
  
  Because no versions of numpy match >1.21.2,<2.0.0
   and numpy (1.21.2) requires Python >=3.7,<3.11, numpy is forbidden.
  So, because django depends on numpy (^1.21.2), version solving failed.

  at /usr/lib/python3.9/site-packages/poetry/puzzle/solver.py:241 in _solve
      237│             packages = result.packages
      238│         except OverrideNeeded as e:
      239│             return self.solve_in_compatibility_mode(e.overrides, use_latest=use_latest)
      240│         except SolveFailure as e:
    → 241│             raise SolverProblemError(e)
      242│ 
      243│         results = dict(
      244│             depth_first_search(
      245│                 PackageNode(self._package, packages), aggregate_package_nodes

  • Check your dependencies Python requirement: The Python requirement can be specified via the `python` or `markers` properties
  
    For numpy, a possible solution would be to set the `python` property to ">=3.9,<3.11"

    https://python-poetry.org/docs/dependency-specification/#python-restricted-dependencies,
    https://python-poetry.org/docs/dependency-specification/#using-environment-markers
(django-ulKECtDq-py3.9) [kearney@arch django]$ 
```

</details>

# pyenv

py 版本管理器 ![GitHub User's stars](https://img.shields.io/github/stars/pyenv/pyenv?style=social)

> Pyenv只会管理通过Pyenv安装的Python版本，你自己在Python官网上下载的直接安装的Pyenv并不能被管理！！！同样除了系统自带的python包外,其他直接安装的python包是识别不出来的,即使使用的brew安装的也识别不出来.

Arch 大法中这样安装：`sudo pacman -S pyenv`

# Docker

如何将自己的环境简单的同步给其它人呢？上面的是根据 requirement.txt 来操作，跟着安装就完事了，但是部署又得在做额外的东西（如 Flask 的生产环境部署），虚拟机技术可以简单拷贝，但是占用资源大，后来有了容器，docker 是其实现方式之一，可以快速的部署相同的环境。所以 docker 更多用在部署的时候，而不是本地开发。

python 在 Docker Hub 上有[镜像](https://hub.docker.com/_/python)，不需要自己造轮子。

# 参考

- [Python虚拟环境 virtualenv 、 venv 、 pyvenv、pipenv And 解决shell中没有虚拟环境名的问题和Linux Deepin安装pipenv的问题. Kearney form An idea 2021-01-25](https://blog.csdn.net/weixin_43031092/article/details/113103853)
- [不要用 Pipenv。 李辉。 2019 年 8 月](https://zhuanlan.zhihu.com/p/80478490)：该书也是我入门虚拟环境的引路人，大概在 2021 年初的时候，文中提到的问题倒是没有太在意到底 21 年还有没有类似问题，因为我 21 年中才看到该文。。。不过该文的评论区提供了一种新思路： docker + pip

  > 注意：本文写于 2019 年 8 月，其中描述的内容在新版本的 Pipenv 中或已得到修复或改进，请谨慎参考。
  > 安装某个包会导致所有依赖版本被更新，Pipenv 在解决依赖的冲突上面也有一些不足
  > 所有的依赖（包括和 Flask 完全不相关的）又都被更新了

- [要不我们还是用回 virtualenv/venv 和 pip 吧. 李辉](https://zhuanlan.zhihu.com/p/81568689)
- [什么是Docker？看这一篇干货文章就够了！小灰。 2020-08-17](https://zhuanlan.zhihu.com/p/187505981)
- [ pyenv/pyenv ](https://github.com/pyenv/pyenv)
- [Python Arch Wiki](https://wiki.archlinux.org/title/Python)
- [Python包管理之poetry的使用。2020-07-18 20:07  -零 ](https://www.cnblogs.com/-wenli/p/13337188.html)
- [ python-poetry/poetry ](https://github.com/python-poetry/poetry)
- [初次使用 python poetry 包管理模块踩坑。Likianta Me 2020-08-09](https://blog.csdn.net/Likianta/article/details/107891071)

  > poetry add `<package>` 遭遇 “[ConnectionError]” - 清华镜像
  > 什么时候使用 ‘poetry.lock’, 什么时候不使用?

- [Disabling the PyPI repository ](https://python-poetry.org/docs/repositories/#disabling-the-pypi-repository)
