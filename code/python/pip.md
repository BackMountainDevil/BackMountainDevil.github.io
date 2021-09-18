# pip 设置国内镜像提高下载速度
time: 2021-09-18

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
    

# 相关参考

- [Python 永修修改pip设置国内默认镜像源，提高下载速度](https://blog.csdn.net/weixin_43031092/article/details/108690238)
