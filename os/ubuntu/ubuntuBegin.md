体验：Ubuntu 20.04.2 LTS 蓝牙适配了 Arch、Win 都没有适配上的型号（我耳机的问题。。），Fn 键适配的比 Arch 多一个 PrtSc，但是对多屏切换按键适配（F10）的不是很好，按下马上切换，我都没选要使用哪一种。 Kunbuntu 21 对多屏切换按键适配完美，但是点击开启蓝牙没啥反映，终端操作了一下还是不行，遂选择了 Ubuntu LTS。 KUb 没有 LTS 的 ipv6 资源..

桌面版最小化安装： 136.3 GB 刚装完已用 8.8 GB，还剩下 120.5 GB。已经使用了半年多的 Arch 在 115.7 GB 中已用 54.8 GB，还剩下 55 GB。单开一文本编辑器内存使用 2.7 GB，又在 Firefox 中开 7 个标签页，内存占用上升到 3.3 GB。2 GB 交换区使用 0。

最小化安装没有 Libre Office，这一点个人比较喜欢，比较 WPS 靠谱点。GNOME 动画效果无法修改，尤其是按下菜单键的动画时间，过于长了。Ubuntu Software 的网络状态不是很给力，教育网也不给力，但是这个软件切换最大化和原状的时候暴露出了 GNOME 的动画效果的缺点。 KDE 的桌面着实给力，但是预装了很多我使用不上的软件，因此在安装 KDE 的时候选择最小化安装，不需要安装 KDE 的工具包 kde-applications


# Mirror
[Ubuntu 镜像使用帮助](https://mirrors6.tuna.tsinghua.edu.cn/help/ubuntu/)

```bash
# 将系统自带的软件源配置文件做个备份
sudo cp /etc/apt/sources.list /etc/apt/sources_backup.list

# 用 TUNA 的软件源镜像
sudo nano /etc/apt/sources.list

# 
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

```

# python
- [pypi 镜像使用帮助](https://mirrors6.tuna.tsinghua.edu.cn/help/pypi/)
Ubuntu 20.04.2 LTS 中 Python 默认只有 3.8.10，没有 py2（这点我喜欢），不过命令需要敲 python3 而不是 python（这点很不舒服），也没有自带 pip..

```bash
# 给 py3 装 pip
sudo apt install python3-pip

pip install pip -U
# 出现的警告信息：pip 脚本不在 PATH 中
WARNING: The scripts pip, pip3 and pip3.8 are installed in '/home/kearney/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed pip-21.1.3

# 向 ~/.bashrc 文件末尾添加 export PATH="${PATH}:/home/kearney/.local/bin" 来
# 修改环境变量 ，复制时需要注意替换用户名，注意是 >> 而不是 >，前者文末追加，后者覆盖
echo export PATH="${PATH}:/home/kearney/.local/bin" >> ~/.bashrc
# 设置 pypi 镜像
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

```

# re
- [Ubuntu 中的 root 用户：你应该知道的重要事情. 作者： Abhishek Prakash 译者： LCTT 郑| 2020-01-31](https://linux.cn/article-11837-1.html)：有说怎么开启 root 的办法，不过用 sudo 即可，临时 root：sudo -i
- [.bashrc删掉了肿么办。阿宅的前行之路 2019-04-02](https://blog.csdn.net/qq_36323837/article/details/88963083)
  > cp /etc/skel/.bashrc ~
 
- []()
- []()
- []()
