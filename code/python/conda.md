miniconda
# install
$ conda -V
conda 4.10.3

## mirror
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

# usage
升级
conda update conda

（1）查看已安装的软件包：conda list
（2）安装软件包：conda install python=3.6.8
（3）卸载软件包：conda uninstall python

conda clean -p        # 清理无用的包
conda clean -t        # 清理tar包
conda clean -y --all  # 清理所有安装包及cache

## venv
创建虚拟环境：conda create –n env_name python=3.6.8
env_name为你虚拟环境的名字，python=3.6.8是指定虚拟环境中python的版本，如果不指定，则默认是安装Miniconda时的版本。

进入虚拟环境：
conda activate env_name
退出
conda deactivate
删除自定义环境：
conda remove --name env_name --all

查看所有环境
conda env list