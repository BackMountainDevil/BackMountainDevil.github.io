# pip 设置国内镜像提高下载速度
- date: 2021-09-18
- lastmod: 2022-03-19

设置 pip 镜像会加快 python 包的下载速度。pip 与 pip3 都有的，将下面的 pip 换为 pip3 在复制粘贴一遍即可。
参见 [pypi 镜像使用帮助 - TUNA](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U # 使用 tuna 镜像更新到最新版的 pip，非必须步骤
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

查看 pip 的设置，对比一下设置前后的区别看看

```bash
pip config list
```

按照上面的设置之后，是这样滴
    
    global.index-url=‘https://pypi.tuna.tsinghua.edu.cn/simple’
    install.trusted-host=‘https://pypi.tuna.tsinghua.edu.cn’

## 重置

[pip / conda 恢复默认源. 2Young2Simple.于 2019-12-26 16:48:41](https://blog.csdn.net/weixin_41677877/article/details/103718534)
> 
    进入pip配置文件夹
    cd ~/.config/pip
    打开pip.conf
    将里面的内容删掉或者用#注释掉
    如果只是想临时用默认源，可用：
    pip install 模块名 -i https://pypi.org/simple
    conda直接输入
    conda config --remove-key channels

# 相关参考

- [Python 永修修改pip设置国内默认镜像源，提高下载速度](https://blog.csdn.net/weixin_43031092/article/details/108690238)
