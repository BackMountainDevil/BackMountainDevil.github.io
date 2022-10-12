# 设置环境变量PATH 之 which is not on PATH.  Consider adding this directory to PATH
- date: 2021-06-01
- lastmod: 2022-10-12

# 环境变量

环境变量是什么？通俗的说就是变量，这些变量设置的内容可能是当前的语言、地区、shell、程序的位置、日志等级等。而比较常见的 $PATH 变量就是告诉系统当我敲命令的时候，你要去哪里找这些命令对于的可执行文件，相当与windows 里的 PATH 环境变量。也就是说 PATH 只是众多环境变量中的一个而已。

# 配置文件

下面是两个常见的环境变量的配置文件，当然还有其它，这里不作过多的深入。

Arch Wiki在[Environment_variables](https://wiki.archlinux.org/title/Environment_variables)中提示说这两配置文件（~/.bashrc、 ~/.pam_environment）多多少少存在点问题，不建议继续使用

-  `/etc/profile` 全局环境变量配置文件
-  `~/.bash_profile` 用户环境变量配置文件

# 查看环境变量
## 全部变量
`printenv` 和 `export` 两个指令都可以查看全部环境变量，后者的输出会比较美观有序。

```bash
$ printenv

.......省略........
TERM_PROGRAM_VERSION=1.56.2
GTK_IM_MODULE=fcitx
APPLICATION_INSIGHTS_NO_DIAGNOSTIC_CHANNEL=true
LANGUAGE=zh_CN:en_US
D_DISABLE_RT_SCREEN_SCALE=1

$ export
.......省略........
declare -x DESKTOP_SESSION="plasma"
declare -x DISPLAY=":0"
declare -x D_DISABLE_RT_SCREEN_SCALE="1"
declare -x GDK_BACKEND="x11"
```

输出内容太多的时候，可以加上参数 `| less`，如 `printenv | less`， `export | less`，按 下或回车显示下一行，按空格显示下一页，按 Q 退出

## 单个变量

- `echo $变量名称` 
- `printenv 变量名称`

要查看单个环境变量，如 PATH ，可以用 `echo $PATH` 或者 `printenv PATH`。注意使用 printenv 的时候没有 `$`

```bash
$ echo $PATH
/home/kearney/.local/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/sbin:/usr/sbin

$ printenv GTK_IM_MODULE
fcitx
```

# 搜索变量

有的时候设置了某个变量，但是又记不住这个变量的全名，除了查看全部变量逐个查找之外还可以搜索相似名称的变量

- `printenv | grep [变量名]`
- `export | grep [变量名]`

```bash
$ printenv | grep LANG
LANGUAGE=zh_CN:en_US
LANG=zh_CN.UTF-8

$ export | grep SEAT
declare -x XDG_SEAT="seat0"
declare -x XDG_SEAT_PATH="/org/freedesktop/DisplayManager/Seat0"
```

# 设置环境变量

- `export 变量名=变量值`

如果变量值中含有空格，最好给变量值加上引号

## 临时性修改

有一次，我在使用 pacman 更新系统的时候出了不少问题，错误和警告都是中文的，搜了一下对于的错误没找到合适的解决方案。但是本着百分之九十九的问题都有人解决过了的年头，应该换成英文去搜索。这个时候就需要临时设置一下语言为英文了。当当前会话结束的时候，被修改的变量还是没修改之前的值

```bash
# 中文错误提示ing
$ export LANG=c
# 重现错误
# 现在是英文提示
```

## 永久性修改

在安装某些软件包的时候，会出现下面这样的警告，这个时候就需要我们在变量 PATH 中加入这个值了。

```bash
WARNING: The script XXXXXX is installed in '/home/kearney/.local/bin' which is not on PATH.
  Consider adding this directory to PATH
```

举个例子

```bash
# 改全局的配置用这个
$ sudo nano /etc/profile

# 改个人的配置用这个
$ nano ~/.bash_profile

# 配置文件里的注释是 # 开头的哦
export PATH="${PATH}:/home/kearney/.local/bin"
export OPENCV_LOG_LEVEL=ERROR
```

粘贴（Ctrl + Shift + V）完成后 Ctrl + S 保存， Ctrl + X 退出 nano。别忘了刷新一下环境变量 `source /etc/profile` 或 `source ~/.bash_profile` 。或者直接开启新的会话。

再举个例子，为了配置 fcitx5 中文输入法的用户环境变量，不要忘记了每一行前面都加一个 export

```
#
# ~/.bash_profile
#

[[ -f ~/.bashrc ]] && . ~/.bashrc
# fcitx5
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export SDL_IM_MODULE=fcitx
export GLFW_IM_MODULE=ibus
```

# References

- [如何在Linux中向$PATH添加目录 - 醉落红尘](https://m.linuxidc.com/Linux/2019-08/159846.htm)
- [linux下查看和添加PATH环境变量 - BruceZhang](https://blog.csdn.net/DLUTBruceZhang/article/details/8811456)
- [How to Set Environment Variables in Linux 2020](https://phoenixnap.com/kb/linux-set-environment-variable#:~:text=1%20To%20set%20permanent%20environment%20variables%20for%20a,changes%20are%20applied%20at%20the%20next%20logging%20in.)

