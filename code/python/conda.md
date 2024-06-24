# miniconda3 miniforge 的安装和使用 'zsh: command not found: conda'
- date: 2021-09-11
- lastmod: 2024-06-24

python 中管理虚拟环境可以使用其自带的 venv,而管理不同版本的 python 环境的时候就可以使用 conda。conda 又有 anaconda 和 miniconda 两种，后者相对请量化一点。

# 安装

miniforge 是社区驱动的替代品，用来替代 Anaconda 的产品，直接下载比较慢，从 [TUNA Mirror](https://mirrors.tuna.tsinghua.edu.cn/github-release/conda-forge/miniforge/) 下载比较快

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/github-release/conda-forge/miniforge/Release%2024.3.0-0/Miniforge3-24.3.0-0-Linux-x86_64.sh
chmod +x Miniforge3-24.3.0-0-Linux-x86_64.sh
./Miniforge3-24.3.0-0-Linux-x86_64.sh
mamba activate /home/yicairun/opt/miniforge3
```

之后的使用和 conda 一样

```bash
# Arch 系列可以从 AUR 中下载 miniconda3 软件包 
$ yay -S miniconda3
# 默认会将 conda 加入 bash 的环境变量中，zsh 见下面的 FAQ
$ conda -V
conda 4.10.3

## 添加国内 mirror
$ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
$ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
$ conda config --set show_channel_urls yes
# 添加 conda-forge
$ conda config --add channels conda-forge
$ conda config --set channel_priority strict
```

# 常见使用办法

```
升级 conda: conda update conda
查看已安装的软件包：conda list
安装软件包：conda install python=3.6.8
卸载软件包：conda uninstall python
清理无用的包: conda clean -p 
清理tar包: conda clean -t
清理所有安装包及cache: conda clean -y --all
```

## 虚拟环境

创建虚拟环境：  
conda create -n env_name python=3.6.8

env_name为你虚拟环境的名字，python=3.6.8是指定虚拟环境中python的版本，如果不指定，则默认是安装Miniconda时的版本。报错请检查是否输入的是中文的短线（-）

激活虚拟环境：  
conda activate env_name

退出虚拟环境：  
conda deactivate

删除自定义环境：
conda remove --name env_name --all

查看所有虚拟环境: conda env list 或 conda info --envs

# FAQ

1. zsh: command not found: conda

conda 安装的时候默认把环境变量加入到 bash 中，没有加入到 zsh 中，因此需要在 zsh 加入 conda 的环境变量。常见的[回答](https://stackoverflow.com/questions/31615322/zsh-conda-pip-installs-command-not-found)会告诉你在 `~/.zshrc` 中加入 `export PATH="pathtoconda/bin:$PATH"`，这种办法或许可以解决这个问题，确实是把 conda 加入环境变量，问题在于加了多少？不确定以及不知道；[回答](https://stackoverflow.com/questions/35246386/conda-command-not-found#:~:text=If%20you%27re%20using%20zsh%20and%20it%20has%20not,then%20reopen%20the%20terminal.%20conda%20command%20should%20work.)里还有更离谱的，直接在 `~/.zshrc` 中加入 `source ~/.bash_profile` 导入全部 bash 中的环境变量。本着求知的精神，正常操作不是应该把 conda 在 .bashrc 加入的设置复制到 .zshrc 中吗？我想应该是这样的，stof 中也有持这种看法的回答。  

在我的 .bashrc 中搜索 conda,关于它的就这么一行 `[ -f /opt/miniconda3/etc/profile.d/conda.sh ] && source /opt/miniconda3/etc/profile.d/conda.sh`。复制到 .zshrc 末尾后该问题就解决了。随后我的疑惑来了，这行语句和答案中的大多数修改 PATH 都不太一样，我怀疑是 AUR 打包着的手笔，于是我[下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)了旧版和新版的两个安装脚本（ Miniconda-1.6.0-Linux-x86_64.sh， Miniconda3-py39_4.9.2-Linux-x86_64.sh）

Miniconda-1.6.0-Linux-x86_64.sh 中关于 bash 的设置如下（L241～L281）

```sh
if [[ $BATCH == 0 ]] # interactive mode
then
    BASH_RC=$HOME/.bashrc
    DEFAULT=no
    echo -n "Do you wish the installer to prepend the Miniconda install location
to PATH in your $BASH_RC ? [yes|no]
[$DEFAULT] >>> "
    read ans
    if [[ $ans == "" ]]; then
        ans=$DEFAULT
    fi
    if [[ ($ans != "yes") && ($ans != "Yes") && ($ans != "YES") &&
                ($ans != "y") && ($ans != "Y") ]]
    then
        echo "
You may wish to edit your .bashrc or prepend the Miniconda install location:

$ export PATH=$PREFIX/bin:\$PATH
"
    else
        if [ -f $BASH_RC ]; then
            echo "
Prepending PATH=$PREFIX/bin to PATH in $BASH_RC
A backup will be made to: ${BASH_RC}-anaconda.bak
"
            cp $BASH_RC ${BASH_RC}-anaconda.bak
        else
            echo "
Prepending PATH=$PREFIX/bin to PATH in
newly created $BASH_RC"
        fi
        echo "
For this change to become active, you have to open a new terminal.
"
        echo "
# added by Miniconda 1.6.0 installer
export PATH=\"$PREFIX/bin:\$PATH\"" >>$BASH_RC
    fi

    echo "Thank you for installing Miniconda!"
fi # !BATCH
```

里面确实在 .bashrc 加入了 `export PATH="$PREFIX/bin:$PATH"`，但是在最新版的安装脚本(Miniconda3-py39_4.9.2-Linux-x86_64.sh)里（L469~L508）似乎换了一种方式

```sh
if [ "$BATCH" = "0" ]; then
    # Interactive mode.
    BASH_RC="$HOME"/.bashrc
    DEFAULT=no
    printf "Do you wish the installer to initialize Miniconda3\\n"
    printf "by running conda init? [yes|no]\\n"
    printf "[%s] >>> " "$DEFAULT"
    read -r ans
    if [ "$ans" = "" ]; then
        ans=$DEFAULT
    fi
    if [ "$ans" != "yes" ] && [ "$ans" != "Yes" ] && [ "$ans" != "YES" ] && \
       [ "$ans" != "y" ]   && [ "$ans" != "Y" ]
    then
        printf "\\n"
        printf "You have chosen to not have conda modify your shell scripts at all.\\n"
        printf "To activate conda's base environment in your current shell session:\\n"
        printf "\\n"
        printf "eval \"\$($PREFIX/bin/conda shell.YOUR_SHELL_NAME hook)\" \\n"
        printf "\\n"
        printf "To install conda's shell functions for easier access, first activate, then:\\n"
        printf "\\n"
        printf "conda init\\n"
        printf "\\n"
    else
        if [[ $SHELL = *zsh ]]
        then
            $PREFIX/bin/conda init zsh
        else
            $PREFIX/bin/conda init
        fi
    fi
    printf "If you'd prefer that conda's base environment not be activated on startup, \\n"
    printf "   set the auto_activate_base parameter to false: \\n"
    printf "\\n"
    printf "conda config --set auto_activate_base false\\n"
    printf "\\n"

    printf "Thank you for installing Miniconda3!\\n"
fi # !BATCH
```

里面似乎没有直接修改 .bashrc，我能直接在 bash 里直接使用 conda 完全是因为那一行 `[ -f /opt/miniconda3/etc/profile.d/conda.sh ] && source /opt/miniconda3/etc/profile.d/conda.sh`。把这一行复制到 .zshrc 之后在 zsh 中也能使用 conda 了。但是通过观察上面的安装脚本可以知道有两个东西 `conda init zsh` 和 `conda init`，分别在 bash 中使用这两个命令，就会自动修改 bash 和 zsh 的配置，这个时候删除之前复制到 zshrc 中那一行也没有关系了。

总结一下，在 zsh 中输入 bash 切换到 bash,这样就可以启动 conda 了，此时键入 `conda init zsh` 就会自动配置 zsh 了，这个问题就这么解决啦。

2. conda install xxpkg 报错 EnvironmentNotWritableError: The current user does not have write permissions to the target environment.

这个问题很显然就是当前用户没有对应的写[权限等级](https://www.runoob.com/linux/linux-comm-chmod.html)（写、读、执行）。一种办法是在指令前面加 sudo； 还有一种办法就是给当前用户加权，这个报错后面会给出对应的路径，如 ` environment location: /opt/miniconda3`，这个时候就可以修改用户对这个目录的操作权限。

```bash
$ ls -ld /opt/miniconda3
drwxr-xr-x 16 root root 4096  9月 14 07:33 /opt/miniconda
# d 代表文件夹，rwx 代表文件夹所有者（root）具有三种权限，r-x 表示文件所有者所在的组（root）的其它组员的权限只有读、执行，后一个 r-x 代表其它用户的权限也只有读、执行。
# 下面是给其它用户添加写权限
$ sudo chmod o+w -R /opt/miniconda3
```

两个方法都可以使用，二选一就行，在弄清楚为什么是 755 之前，我还是选择 sudo 办法

3. VS Code 终端总是自动默认启动 conda base 环境，Konsole 也是如此。

我不想打开终端就默认进入 conda 的虚拟环境，因为有的项目有自己的 venv 虚拟环境，当 IDE 使用终端调试的时候，尽管配置了 venv 虚拟环境，还是会发现 conda base 环境也被激活了，这个时候要关闭 conda 自启功能，只需要敲入 conda config --set auto_activate_base false 即可。

4. 创建虚拟环境报错 CondaValueError: The target prefix is the base prefix. Aborting.

```bash
$ conda create –n fast python=3.8

CondaValueError: The target prefix is the base prefix. Aborting.
```

结论：把 - 换成英文的 -，如果是中文的短线会引发这个错误
- [在linux服务器上使用conda创建虚拟环境时报错：CondaValueError: The target prefix is the base prefix. Aborting. A Duter. 2021-04-15](https://blog.csdn.net/qq_42710637/article/details/115719035)

5. 如何清空base环境的其它包（重置）？

    pip 没有向 pacman 那样的依赖清楚，pip安装一个包会把依赖包也安装上，但是移除的时候并不会清理没有被其它包依赖的包。如果不是base环境的话，可以直接整个环境都移除，[python - 如何从 Anaconda 的 Base 环境中删除不需要的 python 包 ](https://www.coder.work/article/3156577)中提到的`conda install --rev 1`说是可能引起问题，我尝试新装一个环境（miniconda 4.12.0-1 py3.8.13），通过pip freeze查看，结果txt里边是 “certifi @ file:///opt/conda/conda-bld/certifi_1655968806487/work/certifi“，然而我的目录是 /opt/miniconda3,压根没有/opt/conda/，以为是freeze的尖括号写的太近了，又加了空格再试以便还是一样。用 conda list --revisions 得到我的base环境只有 2022-04-21 09:03:40  (rev 0)。。。然而我在好几个不同时间段都用pip装过包（black、jupyterlab），尬住了，可能这也是miniconda的一个bug吧。
    <details>
    <summary>bash operate</summary>

    ```bash
    $ conda create -n py38 python=3.8
    Collecting package metadata (current_repodata.json): done
    Solving environment: done

    ## Package Plan ##

    environment location: /home/kearney/.conda/envs/py38

    added / updated specs:
        - python=3.8

    The following packages will be downloaded:

    _libgcc_mutex      anaconda/pkgs/main/linux-64::_libgcc_mutex-0.1-main
    _openmp_mutex      anaconda/pkgs/main/linux-64::_openmp_mutex-5.1-1_gnu
    ca-certificates    anaconda/pkgs/main/linux-64::ca-certificates-2022.4.26-h06a4308_0
    certifi            anaconda/pkgs/main/linux-64::certifi-2022.6.15-py38h06a4308_0
    ld_impl_linux-64   anaconda/pkgs/main/linux-64::ld_impl_linux-64-2.38-h1181459_1
    libffi             anaconda/pkgs/main/linux-64::libffi-3.3-he6710b0_2
    libgcc-ng          anaconda/pkgs/main/linux-64::libgcc-ng-11.2.0-h1234567_1
    libgomp            anaconda/pkgs/main/linux-64::libgomp-11.2.0-h1234567_1
    libstdcxx-ng       anaconda/pkgs/main/linux-64::libstdcxx-ng-11.2.0-h1234567_1
    ncurses            anaconda/pkgs/main/linux-64::ncurses-6.3-h5eee18b_3
    openssl            anaconda/pkgs/main/linux-64::openssl-1.1.1q-h7f8727e_0
    pip                anaconda/pkgs/main/linux-64::pip-22.1.2-py38h06a4308_0
    python             anaconda/pkgs/main/linux-64::python-3.8.13-h12debd9_0
    readline           anaconda/pkgs/main/linux-64::readline-8.1.2-h7f8727e_1
    setuptools         anaconda/pkgs/main/linux-64::setuptools-61.2.0-py38h06a4308_0
    sqlite             anaconda/pkgs/main/linux-64::sqlite-3.38.5-hc218d9a_0
    tk                 anaconda/pkgs/main/linux-64::tk-8.6.12-h1ccaba5_0
    wheel              anaconda/pkgs/main/noarch::wheel-0.37.1-pyhd3eb1b0_0
    xz                 anaconda/pkgs/main/linux-64::xz-5.2.5-h7f8727e_1
    zlib               anaconda/pkgs/main/linux-64::zlib-1.2.12-h7f8727e_2

    # To deactivate an active environment, use
    #
    #     $ conda deactivate

    $ conda activate py38
    (py38) $ pip list
    Package    Version
    ---------- ---------
    certifi    2022.6.15
    pip        22.1.2
    setuptools 61.2.0
    wheel      0.37.1
    (py38) $ pip freeze>py38initial.txt
    (py38) $ pip freeze > py38initial.txt
    ```
    </details>

    但是在base下 `(base) [kearney@xx cs61a2022]$ pip freeze>re.txt` 得到的txt就有东西，看来可以通过这个文件把多于的包清理掉，删掉txt里的certifi之后进行实践，pip uninstall -r re.txt，回复一堆y之后黄色警告接着红色错误，光敲y没注意卸载的啥，最后把conda移除了，conda都没法用了，卸载重装miniconda

    <details>
    <summary>装了很多包的 base re.txt</summary>

    ```
    anyio @ file:///tmp/build/80754af9/anyio_1617783277988/work/dist
    argon2-cffi @ file:///tmp/build/80754af9/argon2-cffi_1613037499734/work
    astor==0.8.1
    async-generator @ file:///home/ktietz/src/ci/async_generator_1611927993394/work
    attrs @ file:///tmp/build/80754af9/attrs_1620827162558/work
    Babel @ file:///tmp/build/80754af9/babel_1620871417480/work
    backcall @ file:///home/ktietz/src/ci/backcall_1611930011877/work
    backports.entry-points-selectable==1.1.1
    bce-python-sdk==0.8.64
    black==21.12b0
    bleach @ file:///tmp/build/80754af9/bleach_1628110601003/work
    brotlipy==0.7.0
    certifi==2021.10.8
    cffi @ file:///opt/conda/conda-bld/cffi_1642701102775/work
    cfgv==3.3.1
    charset-normalizer @ file:///tmp/build/80754af9/charset-normalizer_1630003229654/work
    click==8.1.3
    colorama @ file:///tmp/build/80754af9/colorama_1607707115595/work
    colorlog==6.6.0
    conda==4.12.0
    conda-content-trust @ file:///tmp/build/80754af9/conda-content-trust_1617045594566/work
    conda-package-handling @ file:///tmp/build/80754af9/conda-package-handling_1649105784853/work
    contextlib2==21.6.0
    cryptography @ file:///tmp/build/80754af9/cryptography_1639414572950/work
    debugpy @ file:///tmp/build/80754af9/debugpy_1637091799509/work
    decorator @ file:///tmp/build/80754af9/decorator_1632776554403/work
    defusedxml @ file:///tmp/build/80754af9/defusedxml_1615228127516/work
    dill==0.3.4
    distlib==0.3.4
    easydict==1.9
    entrypoints==0.3
    execnet==1.9.0
    filelock==3.4.0
    flake8==4.0.1
    future==0.18.2
    gunicorn==20.1.0
    h5py==3.6.0
    identify==2.4.0
    idna @ file:///tmp/build/80754af9/idna_1637925883363/work
    importlib-metadata @ file:///tmp/build/80754af9/importlib-metadata_1631916692253/work
    iniconfig==1.1.1
    ipykernel==6.5.1
    ipython @ file:///tmp/build/80754af9/ipython_1635944169458/work
    ipython-genutils @ file:///tmp/build/80754af9/ipython_genutils_1606773439826/work
    itsdangerous==2.0.1
    jedi @ file:///tmp/build/80754af9/jedi_1611333729159/work
    jieba==0.42.1
    Jinja2 @ file:///tmp/build/80754af9/jinja2_1635780242639/work
    json5 @ file:///tmp/build/80754af9/json5_1624432770122/work
    jsonschema @ file:///Users/ktietz/demo/mc3/conda-bld/jsonschema_1630511932244/work
    jupyter-client @ file:///tmp/build/80754af9/jupyter_client_1637148478538/work
    jupyter-core @ file:///tmp/build/80754af9/jupyter_core_1636537028672/work
    jupyter-server @ file:///tmp/build/80754af9/jupyter_server_1616084066671/work
    jupyterlab @ file:///home/conda/feedstock_root/build_artifacts/jupyterlab_1637169796215/work
    jupyterlab-pygments @ file:///tmp/build/80754af9/jupyterlab_pygments_1601490720602/work
    jupyterlab-server @ file:///tmp/build/80754af9/jupyterlab_server_1633419203660/work
    MarkupSafe @ file:///tmp/build/80754af9/markupsafe_1621523467000/work
    matplotlib-inline @ file:///tmp/build/80754af9/matplotlib-inline_1628242447089/work
    mccabe==0.6.1
    mistune @ file:///tmp/build/80754af9/mistune_1607364877025/work
    mock==4.0.3
    multiprocess==0.70.12.2
    mypy-extensions==0.4.3
    nbclassic @ file:///tmp/build/80754af9/nbclassic_1616085367084/work
    nbclient @ file:///tmp/build/80754af9/nbclient_1614364831625/work
    nbconvert @ file:///tmp/build/80754af9/nbconvert_1624472883256/work
    nbformat @ file:///tmp/build/80754af9/nbformat_1617383369282/work
    nest-asyncio @ file:///tmp/build/80754af9/nest-asyncio_1613680548246/work
    nodeenv==1.6.0
    notebook @ file:///tmp/build/80754af9/notebook_1637161762562/work
    onnx==1.9.0
    packaging @ file:///tmp/build/80754af9/packaging_1637314298585/work
    pandocfilters @ file:///tmp/build/80754af9/pandocfilters_1605120906940/work
    parso @ file:///tmp/build/80754af9/parso_1617223946239/work
    path==16.2.0
    path.py==12.5.0
    pathspec==0.9.0
    pexpect @ file:///tmp/build/80754af9/pexpect_1605563209008/work
    pickleshare @ file:///tmp/build/80754af9/pickleshare_1606932040724/work
    platformdirs==2.4.0
    pluggy==1.0.0
    pre-commit==2.16.0
    prometheus-client @ file:///tmp/build/80754af9/prometheus_client_1637050397234/work
    prompt-toolkit @ file:///tmp/build/80754af9/prompt-toolkit_1633440160888/work
    protobuf==3.19.1
    ptyprocess @ file:///tmp/build/80754af9/ptyprocess_1609355006118/work/dist/ptyprocess-0.7.0-py2.py3-none-any.whl
    py==1.11.0
    pycodestyle==2.8.0
    pycosat==0.6.3
    pycparser @ file:///tmp/build/80754af9/pycparser_1636541352034/work
    pycryptodome==3.12.0
    pyflakes==2.4.0
    Pygments @ file:///tmp/build/80754af9/pygments_1629234116488/work
    pyOpenSSL @ file:///opt/conda/conda-bld/pyopenssl_1643788558760/work
    pyparsing @ file:///tmp/build/80754af9/pyparsing_1635766073266/work
    pyrsistent @ file:///tmp/build/80754af9/pyrsistent_1636110951836/work
    PySocks @ file:///tmp/build/80754af9/pysocks_1605305812635/work
    pytest==6.2.5
    pytest-shutil==1.7.0
    python-dateutil @ file:///tmp/build/80754af9/python-dateutil_1626374649649/work
    pytz==2021.3
    pyzmq @ file:///tmp/build/80754af9/pyzmq_1628275385016/work
    rarfile==4.0
    requests @ file:///opt/conda/conda-bld/requests_1641824580448/work
    ruamel-yaml-conda @ file:///tmp/build/80754af9/ruamel_yaml_1616016711199/work
    Send2Trash @ file:///tmp/build/80754af9/send2trash_1632406701022/work
    seqeval==1.2.2
    shellcheck-py==0.8.0.2
    six @ file:///tmp/build/80754af9/six_1644875935023/work
    sniffio @ file:///tmp/build/80754af9/sniffio_1614030464178/work
    termcolor==1.1.0
    terminado==0.9.4
    testpath @ file:///tmp/build/80754af9/testpath_1624638946665/work
    toml==0.10.2
    tomli==1.2.3
    tornado @ file:///tmp/build/80754af9/tornado_1606942317143/work
    tqdm @ file:///opt/conda/conda-bld/tqdm_1647339053476/work
    traitlets @ file:///tmp/build/80754af9/traitlets_1636710298902/work
    typing_extensions==4.0.1
    urllib3 @ file:///opt/conda/conda-bld/urllib3_1643638302206/work
    virtualenv==20.10.0
    wcwidth @ file:///Users/ktietz/demo/mc3/conda-bld/wcwidth_1629357192024/work
    webencodings==0.5.1
    Werkzeug==2.0.2
    zipp @ file:///tmp/build/80754af9/zipp_1633618647012/work
    ```
    </details>

    折腾了好一会装回了miniconda3,还是base下freeze得到的txt，看来复原base的时候要从txt中剔除掉19个包，不单单是剔除certifi
    <details>
    <summary>base原状的re.txt</summary>

    ```
    brotlipy==0.7.0
    certifi==2021.10.8
    cffi @ file:///opt/conda/conda-bld/cffi_1642701102775/work
    charset-normalizer @ file:///tmp/build/80754af9/charset-normalizer_1630003229654/work
    colorama @ file:///tmp/build/80754af9/colorama_1607707115595/work
    conda==4.12.0
    conda-content-trust @ file:///tmp/build/80754af9/conda-content-trust_1617045594566/work
    conda-package-handling @ file:///tmp/build/80754af9/conda-package-handling_1649105784853/work
    cryptography @ file:///tmp/build/80754af9/cryptography_1639414572950/work
    idna @ file:///tmp/build/80754af9/idna_1637925883363/work
    pycosat==0.6.3
    pycparser @ file:///tmp/build/80754af9/pycparser_1636541352034/work
    pyOpenSSL @ file:///opt/conda/conda-bld/pyopenssl_1643788558760/work
    PySocks @ file:///tmp/build/80754af9/pysocks_1605305812635/work
    requests @ file:///opt/conda/conda-bld/requests_1641824580448/work
    ruamel-yaml-conda @ file:///tmp/build/80754af9/ruamel_yaml_1616016711199/work
    six @ file:///tmp/build/80754af9/six_1644875935023/work
    tqdm @ file:///opt/conda/conda-bld/tqdm_1647339053476/work
    urllib3 @ file:///opt/conda/conda-bld/urllib3_1643638302206/work
    ```
    </details>

# Problem

## AttributeError: module 'cryptography.hazmat.backends' has no attribute 'openssl'

AUR 中的 miniconda3 更新到 23.5.2-1 之后出现的问题（更新前后我有按照说明注释掉 `.bashrc` 中的 conda）。当时更新之后去看 AUR 评论也没有发现啥，过了两天系统升级后该 bug 还在。第三天拿下 CPR 之后看下评论区发现一个可能的办法
> gru: solved it by activating the base conda env and updating with pip pyopenssl and cryptography

然而我尝试 `conda activate base` 得到相同的错误

```bash
[mifen@hp ~]$ conda env list
Traceback (most recent call last):
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/exception_handler.py", line 16, in __call__
    return func(*args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/cli/main.py", line 70, in main_subshell
    p = generate_parser()
        ^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/cli/conda_argparse.py", line 65, in generate_parser
    p = ArgumentParser(
        ^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/cli/conda_argparse.py", line 152, in __init__
    self._subcommands = context.plugin_manager.get_hook_results("subcommands")
                        ^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/base/context.py", line 502, in plugin_manager
    from ..plugins.manager import get_plugin_manager
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/plugins/__init__.py", line 3, in <module>
    from .hookspec import hookimpl  # noqa: F401
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/plugins/hookspec.py", line 9, in <module>
    from .types import CondaSolver, CondaSubcommand, CondaVirtualPackage
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/plugins/types.py", line 7, in <module>
    from ..core.solve import Solver
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/core/solve.py", line 41, in <module>
    from .index import _supplement_index_with_system, get_reduced_index
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/core/index.py", line 24, in <module>
    from .subdir_data import SubdirData, make_feature_record
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/core/subdir_data.py", line 53, in <module>
    from ..trust.signature_verification import signature_verification
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/trust/signature_verification.py", line 12, in <module>
    from conda_content_trust.authentication import verify_delegation, verify_root
  File "/opt/miniconda3/lib/python3.11/site-packages/conda_content_trust/authentication.py", line 34, in <module>
    from .common import (
  File "/opt/miniconda3/lib/python3.11/site-packages/conda_content_trust/common.py", line 66, in <module>
    import cryptography.hazmat.backends.openssl.ed25519
  File "/opt/miniconda3/lib/python3.11/site-packages/cryptography/hazmat/backends/openssl/__init__.py", line 6, in <module>
    from cryptography.hazmat.backends.openssl.backend import backend
  File "/opt/miniconda3/lib/python3.11/site-packages/cryptography/hazmat/backends/openssl/backend.py", line 61, in <module>
    from cryptography.hazmat.bindings.openssl import binding
  File "/opt/miniconda3/lib/python3.11/site-packages/cryptography/hazmat/bindings/openssl/binding.py", line 232, in <module>
    Binding.init_static_locks()
  File "/opt/miniconda3/lib/python3.11/site-packages/cryptography/hazmat/bindings/openssl/binding.py", line 206, in init_static_locks
    cls._ensure_ffi_initialized()
  File "/opt/miniconda3/lib/python3.11/site-packages/cryptography/hazmat/bindings/openssl/binding.py", line 195, in _ensure_ffi_initialized
    _legacy_provider_error(cls._legacy_provider_loaded)
  File "/opt/miniconda3/lib/python3.11/site-packages/cryptography/hazmat/bindings/openssl/binding.py", line 104, in _legacy_provider_error
    raise RuntimeError(
RuntimeError: OpenSSL 3.0's legacy provider failed to load. This is a fatal error by default, but cryptography supports running without legacy algorithms by setting the environment variable CRYPTOGRAPHY_OPENSSL_NO_LEGACY. If you did not expect this error, you have likely made a mistake with your OpenSSL configuration.

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/opt/miniconda3/condabin/conda", line 13, in <module>
    sys.exit(main())
             ^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/cli/main.py", line 129, in main
    return conda_exception_handler(main, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/exception_handler.py", line 376, in conda_exception_handler
    return_value = exception_handler(func, *args, **kwargs)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/exception_handler.py", line 19, in __call__
    return self.handle_exception(exc_val, exc_tb)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/exception_handler.py", line 75, in handle_exception
    return self.handle_unexpected_exception(exc_val, exc_tb)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/exception_handler.py", line 88, in handle_unexpected_exception
    self.print_unexpected_error_report(error_report)
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/exception_handler.py", line 159, in print_unexpected_error_report
    from .cli.main_info import get_env_vars_str, get_main_info_str
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/cli/main_info.py", line 15, in <module>
    from ..core.index import _supplement_index_with_system
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/core/index.py", line 24, in <module>
    from .subdir_data import SubdirData, make_feature_record
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/core/subdir_data.py", line 53, in <module>
    from ..trust.signature_verification import signature_verification
  File "/opt/miniconda3/lib/python3.11/site-packages/conda/trust/signature_verification.py", line 12, in <module>
    from conda_content_trust.authentication import verify_delegation, verify_root
  File "/opt/miniconda3/lib/python3.11/site-packages/conda_content_trust/authentication.py", line 34, in <module>
    from .common import (
  File "/opt/miniconda3/lib/python3.11/site-packages/conda_content_trust/common.py", line 334, in <module>
    cryptography.hazmat.backends.openssl.ed25519._Ed25519PrivateKey, # DANGER
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
AttributeError: module 'cryptography.hazmat.backends' has no attribute 'openssl'
```

### 解决办法

进入 conda 安装目录下的 bin 目录，使用其下的 pip 删除旧版的包，然后重装就会装新版的了

```bash
$ pwd
/opt/miniconda3/bin
$ ./pip list | grep pyOpenSSL
pyOpenSSL               23.0.0
$ ./pip list | grep cryptography
cryptography            39.0.1

$ sudo ./pip uninstall cryptography pyOpenSSL
$ sudo ./pip install cryptography pyOpenSSL
Successfully installed cryptography-41.0.3 pyOpenSSL-23.2.0

$ conda env list
# conda environments:
#
310                      /home/mifen/.conda/envs/310
vpy                      /home/mifen/.conda/envs/vpy
base                     /opt/miniconda3
```
