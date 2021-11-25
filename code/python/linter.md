# Lint 和 Format 工具对比
- date: 2021-09-12
- lastmod: 2021-09-15

# Lint

直接说 Lint 可能会不太明白是个啥，但是要说起常见的 Linter 你就会知道它的作用是干啥的了，比如 [pylint](https://github.com/PyCQA/pylint)、[flake8](https://github.com/PyCQA/flake8)。前者的 star、commit、issue 大都是后者的两倍以上，不过后者 2010 年才破壳，比前者晚了 7 年，

这些 Linter 会对你写的代码进行错误检查，比如变量未声明、变量未使用、参数过多过少、括号没匹配上等比较常见的错误，这些错误通常在运行时会造成异常，这些工具能在开始前就进行检查，减少了运行调试的时间。

当然有的 Linter 还会进行更一步的检查，这些检查并不影响程序的运行，比如运算符前后都要空格、注释的井号和注释之间需要保持一个空格、一行不能写太长等等（美观整洁检查）

flake8 和 pylint 现在都可以集成的 IDE 中，这样就会自动检查代码规范并自动规范化。当然也可以手动调用工具进行检查，如使用 flake8 检查 main.py 的代码规范 - `flake8 main.py`，规范没问题的时候啥输出也没有，只有存在不规范的点才会有结果。

## PEP8

在 Python 中每次提到代码的书写规范（整洁美观），都会提起 [PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008)，这是一个 Python 的社区指导性规范，指导性意味着不是强制的，可以不按这个规范写，代码也能跑起来。

但是谁想看到杂乱无章的代码呢，整洁美观影响维护的速度，现在连自己想怎么整洁都省略了，直接按着规范来就好了。不过，人类大都是懒惰的生物，懒得阅读、学习规范，不如写好代码，自动检查一下那里不规范，再进一步，自动修正一下不是更好，于是诞生了下面的格式化工具（感觉翻译为“规范化”更好，格式化在中文里意味着删除全部数据）

# Format

规范化中有好多神器，这里挑两个用的比较多的：[black](https://github.com/psf/black)、[yapf](https://github.com/google/yapf)，前者是后起之秀，用最简的参数完成配置，后者 yapf 是 google 出品，有大厂的背书，可以选择不同的规范进行规范化操作

由于 black 能配置的比较少，经常被吐槽的一点有它会将所有的 ' 替换成 "。关于它的代码规范可以在其仓库的 docs/the_black_code_style.md 中进行查看。

下面是 VS Code 中的配置案例（/.vscode/settings.json），使用了 flake8 和 black

```json
{
  "python.formatting.provider": "black",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "files.insertFinalNewline": true,
  "editor.formatOnSave": true,
  "python.linting.flake8Args": [
    "--max-line-length=88"
  ],
}
```

flake8 会检测一行的长度是否超过 79，一般有三种解决办法，将太长的写到第二行，扩大 flake8 的长度限制，或者是直接过滤掉这个长度检测。上面是将长度阈值修改为 black 默认的 88。

# 相关参考

- [让 Python 代码更易维护的七种武器——代码风格（pylint、Flake8、Isort、Autopep8、Yapf、Black）测试覆盖率（Coverage）CI（JK）。 bonelee 2019-06-18](https://www.cnblogs.com/bonelee/p/11045196.html)
- []()
- []()