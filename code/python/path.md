# 自定义模块/包无法识别路径 PYTHONPATH sys.path
- date: 2023-07-08
- lastmod: 2022-07-08

结论：`export PYTHONPATH=$PWD`

原因：自己写的模块/包不在环境变量里，而py解释器是根据环境变量去找模块的，那么解决办法就是把模块/包所在路径加入py环境变量。IDE会自动处理环境变量，而终端则需要手动加入

## 查看 py 的环境变量的方式

1. 查看会话的环境变量：`echo $PYTHONPATH`
2. 通过 sys 库

    ```bash
    $ python
    >>> import sys
    >>> sys.path
    ['', '/usr/lib/python311.zip', '/usr/lib/python3.11', '/usr/lib/python3.11/lib-dynload', '/usr/lib/python3.11/site-packages']
    ```

## 修改py环境变量的方式

1. 终端 `export PYTHONPATH=$PWD`
2. 代码 `sys.path.append("..")`

# 参考

[python修改sys.path的三种方法](http://www.coolpython.net/python_senior/module_concept/modify-sys-path.html)

[完美解决，python自定义模块找不到问题 Biturd 2020-06-07](https://blog.csdn.net/qq_42873554/article/details/106604859)
> sys.path.append("..")
