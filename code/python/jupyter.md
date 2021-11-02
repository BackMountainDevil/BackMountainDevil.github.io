# miniconda3 安装 jupyter-lab
- date: 2021-09-14
- lastmod: 2021-09-14

jupyter 是个啥？很久之前接触 python 机器学习的时候最早接触的是 jupyter notebook，编写 python 的代码不再是写完全部再运行调试，notebook 可以将程序分为一小块一小块（cell）的运行，和在 python 交互式终端中类似，但比后者更加方便修改运行。2021 年 jupyter-lab 不仅拥有 notebook 的功能，还有就是 notebook 一个文件会打开一个浏览器标签页，而 lab 就像在一个浏览器标签页内整了一个轻量化的 IDE。

从[官网的安装详情](https://jupyter.org/install.html)中可知有那么以下几种安装方式 conda、mamba、pip，经本人查询，也可从通过 pacman 安装。下面记录下从 conda 安装后因依赖问题后卸载，再通过发行版仓库安装。

# 安装
## conda 方式
conda 的安装参见前面的[文章](./conda.md)。

```bash
$ conda install -c conda-forge jupyterlab
# EnvironmentNotWritableError 报错可以在上面的指令前加 sudo

$ jupyter-lab
command not found

$ conda activate base
(base) $ jupyter-lab
[I 2021-09-14 09:39:53.000 ServerApp] jupyterlab | extension was successfully linked.
[I 2021-09-14 09:39:53.009 ServerApp] Writing Jupyter server cookie secret to /home/kearney/.local/share/jupyter/runtime/jupyter_cookie_secret
[I 2021-09-14 09:39:53.279 ServerApp] nbclassic | extension was successfully linked.
[I 2021-09-14 09:39:53.307 ServerApp] nbclassic | extension was successfully loaded.
[I 2021-09-14 09:39:53.308 LabApp] JupyterLab extension loaded from /opt/miniconda3/lib/python3.9/site-packages/jupyterlab
[I 2021-09-14 09:39:53.308 LabApp] JupyterLab application directory is /opt/miniconda3/share/jupyter/lab
[I 2021-09-14 09:39:53.310 ServerApp] jupyterlab | extension was successfully loaded.
[I 2021-09-14 09:39:53.311 ServerApp] 启动notebooks 在本地路径: /home/kearney/Documents/BackMountainDevil.github.io
[I 2021-09-14 09:39:53.311 ServerApp] Jupyter Server 1.11.0 is running at:
[I 2021-09-14 09:39:53.311 ServerApp] http://localhost:8888/lab?token=2eeb8249dbb23e7963195c9dacae6bb719be57914564ea95
[I 2021-09-14 09:39:53.311 ServerApp]  or http://127.0.0.1:8888/lab?token=2eeb8249dbb23e7963195c9dacae6bb719be57914564ea95
[I 2021-09-14 09:39:53.311 ServerApp] 使用control-c停止此服务器并关闭所有内核(两次跳过确认).
[C 2021-09-14 09:39:53.330 ServerApp] 
    
    To access the server, open this file in a browser:
        file:///home/kearney/.local/share/jupyter/runtime/jpserver-2409-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/lab?token=2eeb8249dbb23e7be57914564ea95
     or http://127.0.0.1:8888/lab?token=2eeb8249dbb23e79631914564ea95
[W 2021-09-14 09:39:57.055 LabApp] Could not determine jupyterlab build status without nodejs
[I 2021-09-14 09:41:11.601 ServerApp] New terminal with automatic name: 1
TermSocket.open: 1
not able to serialize: expected string or bytes-like object
TermSocket.open: Opened 1
```

上面报出了一个警告 `[W 2021-09-14 09:39:57.055 LabApp] Could not determine jupyterlab build status without nodejs`，目前好像没啥影响，乍一看就知道是 nodejs的问题，[stackoverflow](https://stackoverflow.com/questions/51027976/could-not-determine-jupyterlab-build-status-without-nodejs) 在 2019 年就有这个问题的解决办法了，安装 LTS 版本的 nodejs。

不知道是我安装哪里出现了问题，按照官方的要求下载的。。。依赖问题没有解决好，手动安装 nodejs 会使它成为一个孤包，依赖关系没有在系统中得到记录。而我的系统中暂时不装 pip,仅通过虚拟环境中使用 pip，主要是担心用户 pip 安装的东西和系统安装的混搭在一起。于是剩下了从发行版的仓库中安装，先把刚才通过 conda 安装的 jupyterlab 给卸载了（conda也会卸载其依赖）

`sudo conda uninstall -c conda-forge jupyterlab`

## pacman

我使用的是 Manjaro Linux，其官方中有相关的软件包，可以直接进行安装 `sudo pacman -S jupyterlab`，启动方式也很简单，直接在终端中敲入 jupyter-lab 即可启动。
