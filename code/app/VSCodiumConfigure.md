# VS Codium 配置
- date: 2020-11-15
- lastmod: 2021-09-19

# Intro
VS Codium 是VS Code的开源版本，没有微软的图标和用户Track功能。但是谁想被Tracked呢？？下面记录Deepin 20系统下VS Codium配置不同编程语言的的过程。由于采用的是VSCodium，插件市场会滤掉那些不是free的插件，配置起来会有点找不到资料。

VS Code在终端中敲code即可启动，VS Codium则是敲codium

在 Arch Linux 上可以借助 AUR(`yay -S vscodium-bin`) 来快速安装 [vscodium](https://wiki.archlinux.org/title/Visual_Studio_Code_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))，插件市场有问题的话还可以安装 vscodium-bin-marketplace 试一下，不过我卸载 code 之后直接装 codium,插件市场没问题，之前的插件也还在。

# code runner
这个 vs code 插件可以让人可以轻松运行大部分代码而无需单独设置环境，缺点就是无法调试，此外还需要对其进行两个设置，勾选 File Directory as Cwd 和 Run in terminal，不然运行的时候路径和输入会有问题。

# python
## 一键运行
这里用 Coidum 配置插件的方式按下 F5 即可允许程序而不用在终端里面自己敲了。需要安装插件 Python from ms-python 或者是 CodeRunner，前者是F5运行，后者是右键菜单运行，我更喜欢前者一键运行。

此时建立一个python工作区文件夹，然后新建 .vscode/lanch.json 并在里面加入以下内容

```bash
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "stopOnEntry": false,
            "cwd": "${fileDirname}"
        }
    ]
}
```

- "stopOnEntry": false	： 设置一次F5即可运行py，而不是两次
- "cwd": "${fileDirname}"	： 设置在运行文件时进入对应的路径下，不然在使用相对路径的代码中就会出现路径问题，相同的问题也会出现在coderunner插件中
   
然后新建.vscode/setting.json 并在其中写入以下内容，设置好之后对选中的代码按下快捷键Alt+Shift+F即可自动格式化代码，我设置了却无效？？在快捷键里面查看了“格式”的相关快捷键，发现是Ctrl+K、Ctrl+F

```bash
{ 
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "python.formatting.provider": "black",
  "files.insertFinalNewline": true,
  "editor.formatOnSave": true,
  "python.linting.flake8Args": [
    "--max-line-length=88"
  ],
  "python.pythonPath": "env/bin/python",
}

```

- "python.linting.enabled"：        开启代码分析功能，不开启的话下面两行相当寂寞
- "python.linting.pylintEnabled":   禁用默认的pylint分析
- "python.linting.flake8Enabled":   开启flake8分析代码
- "python.formatting.provider":     设置代码格式化工具，推荐 black、yapf
- "files.insertFinalNewline":       在文件末尾插入一行空行
- "editor.formatOnSave":            保存文件时进行自动格式化
- "python.pythonPath":              设置py解释器路径

### 代码规范检查工具 - linter

PEP 8 是 Python 社区推广的代码风格规范，它规定了类似行长度、缩进、多行表达式、变量命名约定等内容。而像我这样的懒人也懒得看人家写了啥，只想着用轮子就行，我写好代码，轮子帮我检查一下那里不符合规范。。  
比如注释#前要有两空格，#后有一个空格;逗号、冒号后面要整个空格;四个空格作为缩进等等  
- pep8(pycodestyle)
- pylint
- [flake8](https://gitlab.com/pycqa/flake8)

### 代码自动规范化工具 - formatter

这个就更让我省心了，帮我检查顺带帮我更正一下好吧，black超越了yapf！！！因为black比yapf更简洁 - Less is more.（没有太多的自定义选项），不过在广大网友的要求下终于给出了不要规范化字符串双引号的配置选项，默认不开启，需要手动设置
- [autopep8](https://github.com/hhatto/autopep8)： Star相比其它两个还是很少的。。
- [yapf](https://github.com/google/yapf): google出品
- [black](https://github.com/psf/black): 2021 Star领先

## snippet

开头前两行是表示本文件是 python3 文件，并且编码为 UTF-8（py2 需要设置，py3 默认支持 utf8）。文件-首选项-用户片段，为python新建全局代码片段文件，将内容替换为  

```bash
{
	"HEADER": {
		"prefix": "header",
		"body": [
            "#!/usr/bin/env python3",
			"# -*- encoding: UTF-8 -*-",
			"'''",
			"@File    :  $TM_FILENAME",
			"@Time    :  $CURRENT_YEAR/$CURRENT_MONTH/$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND",
			"@Author  :  Kearney",
			"@Contact :  191615342@qq.com",
			"@Desc    :  ",
			"'''",
			"$0"
		],
	  }
}
```

# C/C++
## gcc
写完c程序tmp.c可以在终端里面敲入
```bash
# 编译 C 程序
gcc -W  tmp.c -o tmp
# 运行程序
./tmp

# 编译 C++ 程序
g++ -W  tmp.cpp -o tmp
# 运行程序
./tmp
```

## c-extend
marketplace没有C/C++插件。。。然后我跑去微软C/C++ rpo发isuue请人家以后发一个version到open-vsx.org。。。现在采用的是下载插件的vsix文件，命令行安装  
`codium --install-extension myextension.vsix`

## 自动运行与调试
coderunner 这里就不再赘述了，把 C/C++ 插件、gdb、gcc、g++ 安装完成之后，新建 .vscode/launch.json 和 .vscode/tesk.json。内容如下

```json
// .vscode/launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "C/C++",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "preLaunchTask": "compile",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}

```

> 如果需要是 c 语言,需要将下面的 command 项由 g++ 改为 gcc

```json
// .vscode/tesk.json
{
    "version": "2.0.0",
    "tasks": [{
            "label": "compile",
            "command": "g++",  // c 改为 gcc
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "problemMatcher": {
                "owner": "cpp",
                "fileLocation": [
                    "relative",
                    "${workspaceRoot}"
                ],
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            },
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}

```


## snippet
```json
{
		"添加注释：作者和版权信息模板": {
		"prefix": "author",
		"body": [
			"/*@File    :  $TM_FILENAME",
			"  @Desc    :  对文件的简述",
			"  @Author  :  Kearney",
			"  @Contact :  191615342@qq.com",
			"  @version :  0.0.0",
			"  @Time    :	$CURRENT_YEAR/$CURRENT_MONTH/$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND",
			"  @License :  MIT",
			"  @Encoding:  UTF-8  */",
			""
		],
		"description": "添加作者、版权信息、文件内容的基本信息"
	}
}
```

# References
- [ vscodium/DOCS.md ](https://github.com/VSCodium/vscodium/blob/master/DOCS.md)
- [python没有安装pip么？-Deepin](https://bbs.deepin.org/post/132143#mod=viewthread&tid=132143)
- [Deepin15.10 python3安装、更新pip](https://blog.csdn.net/github_39533414/article/details/90678597)
- [Windows 10下Python pip快速设置国内镜像源](https://blog.csdn.net/weixin_43031092/article/details/108690238?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160548571119724838556584%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=160548571119724838556584&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_default-1-108690238.pc_v2_rank_blog_default&utm_term=pip&spm=1018.2118.3001.4450)
- [用VScode配置Python开发环境](https://www.cnblogs.com/xiaojwang/p/11331202.html)
- [gcc预处理、编译、汇编、链接案例](https://blog.csdn.net/weixin_43031092/article/details/105482255?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160549311519725222419396%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=160549311519725222419396&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_default-2-105482255.pc_v2_rank_blog_default&utm_term=gcc&spm=1018.2118.3001.4450)
- [让 Python 代码更易维护的七种武器——代码风格（pylint、Flake8、Isort、Autopep8、Yapf、Black）测试覆盖率（Coverage）CI（JK）2018/09/29 ](https://www.cnblogs.com/bonelee/p/11045196.html)
- [Linux/Ubuntu中Vs Code配置C++/C环境. CS_Lee_ 2020-06-19](https://blog.csdn.net/zimuzi2019/article/details/106861692)