# miniconda3 的安装和使用 'zsh: command not found: conda'

python 中管理虚拟环境可以使用其自带的 venv,而管理不同版本的 python 环境的时候就可以使用 conda。conda 又有 anaconda 和 miniconda 两种，后者相对请量化一点。

# 安装

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
```

# 常见使用办法

升级 conda: conda update conda
查看已安装的软件包：conda list
安装软件包：conda install python=3.6.8
卸载软件包：conda uninstall python
清理无用的包: conda clean -p 
清理tar包: conda clean -t
清理所有安装包及cache: conda clean -y --all

## 虚拟环境

创建虚拟环境：  
conda create –n env_name python=3.6.8
env_name为你虚拟环境的名字，python=3.6.8是指定虚拟环境中python的版本，如果不指定，则默认是安装Miniconda时的版本。

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
