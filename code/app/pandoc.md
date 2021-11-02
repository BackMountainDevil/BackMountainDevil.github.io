# 在 Arch 上用 Pandoc 将 Markdown 文档导出为 PDF
- date: 2021-05-01
- lastmod: 2021-05-01

# 简介

自己平时写文档主要是用 md 写，但是甲方和乙方有时候说喜欢 pdf 或者 word. docsify 没 的自带的 pdf 转换（实际上插件可以导出），最基本的王者就是这个 pandoc 了。

# pandoc

一个文档格式转换大神级工具，支持HTML、CSL YAML、Ebooks、Interactive notebook formats、Word processor formats

[Pandoc installing](https://pandoc.org/installing.html#linux)  
>For PDF output, you’ll need LaTeX. We recommend installing TeX Live via your package manager. (On Debian/Ubuntu, apt-get install texlive.)

如果想输出为pdf,需要安装 LaTeX 引擎

## 安装

```bash
# 安装pandoc
sudo pacman -S pandoc

# 安装 texlive 和中文支持
sudo pacman -S texlive-core texlive-langchinese
```

#### 测试

运行下面的指令能输出对于的版本号说明起码这个软件是装上了的

```bash
$ pandoc --version
pandoc 2.11.4
Compiled with pandoc-types 1.22, texmath 0.12.1.1, skylighting 0.10.3,
citeproc 0.3.0.9, ipynb 0.1.0.1
User data directory: /home/kearney/.local/share/pandoc or /home/kearney/.pandoc
Copyright (C) 2006-2021 John MacFarlane. Web:  https://pandoc.org
This is free software; see the source for copying conditions. There is no
warranty, not even for merchantability or fitness for a particular purpose.
```

### demo

```bash
$ tex '\empty Hello world!\bye'
This is TeX, Version 3.14159265 (TeX Live 2020/Arch Linux) (preloaded format=tex)
[1]
Output written on texput.dvi (1 page, 224 bytes).
Transcript written on texput.log.

$ pdftex '\empty Hello world!\bye'
pdftex: error while loading shared libraries: libpoppler.so.108: cannot open shared object file: No such file or directory
# emm版本问题后面升级解决了这个问题，网上搜的基本是ls软连接，不能治本
# Arch 不支持部分升级（含软件），具体可参考 [Arch Pacman & Yay & 更新中无法满足依赖关系的解决办法](./pacman.md)
```

中文问题继续往下说，如果在 demo 里直接加中文会报错`Missing character: There is no � in font cmr10!`

![Hello Woild! 的 PDF 文件内容预览](https://img-blog.csdnimg.cn/20210317105611405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)
## Demo: markdown

quickstart.md 的内容

```bash
# 快速入门

## 环境准备

软件使用需要满足的前置条件

## 安装

软件的安装方法

## 设置

软件的设置

```

除了输入输出的文件名，还有latex引擎，当然最主要的是中文字体，如何查看本机上的中文字体呢？

>[Pandoc 使用及踩坑 点半久 Edited on 2019-08-31](https://www.dianbanjiu.com/post/pandoc-%E4%BD%BF%E7%94%A8%E5%8F%8A%E8%B8%A9%E5%9D%91/)  
>使用 `fc-list :lang=zh` 命令查看一下你自己系统上都安装了哪些中文字体：  
>每一行第一个冒号到其后第一个逗号之间的内容

```bash
pandoc quickstart.md -o quickstart.md.pdf --pdf-engine=xelatex -V CJKmainfont='Sarasa UI SC' 
```

![quickstart.md 的 PDF 输出预览](https://img-blog.csdnimg.cn/20210317111714686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzAzMTA5Mg==,size_16,color_FFFFFF,t_70)

# 卸载

万一呢，目前两个月就体验了一把 PDF 输出。。。

```bash
$ sudo pacman -Rs pandoc
```

# 参考

[Pandoc](https://pandoc.org/)

[TeX Live (简体中文) - Arch WiKi](https://wiki.archlinux.org/index.php/TeX_Live_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85TeXLive)


[libpoppler.so.76: cannot open shared object file: No such file or directory carolynlmk 2019-01-25](https://blog.csdn.net/carolynlmk/article/details/86650840):已经有了libpoppler.so.19库，这个更新。所以通过建立19到18链接来暂时避免

[Mangaro下安装TextLive遇到的问题 Tender_Li 2020-02-06](https://blog.csdn.net/Tender_Li/article/details/104201764):建立19到18链接，没有证明自己有没有存在这个库。。

[error while loading shared libraries: libpoppler.so.101: cannot open shared object file: No such file or directory  2020](https://emacs.stackexchange.com/questions/60490/error-while-loading-shared-libraries-libpoppler-so-101-cannot-open-shared-obje)
>After check the directory /usr/lib/ I found there is no libpoppler.so.101, instead, we get libpoppler.so.102. Then I checked the pamac history and found the poppler related packages were updated from 0.90.1-1 to 20.08.1-1. I think this is why libpoppler.so updated from 101 to 102.
The problem here is, after the update, pdf-tools tries to rebuild itself. However, it does not really do the rebuild, which I don't know why. (maybe make does not find the change of source code so it just skips all the rebuild. sorry, I only know little about compile.)
So, what I did is go to pdf-tools' directory which is path/to/.emacs.d/elpa/pdf-tools-xxx/build and run make clean to remove these object files manually. I also did make clean in the server subdirectory under build, but I am not sure if it is necessary. Then reopen emacs and let it rebuild the pdf-tools (or run pdf-tools-install, to rebuild it manually).

所以问题无法确定是不是升级导致的

[phodal/ebook-boilerplate](https://github.com/phodal/ebook-boilerplate):A Markdown convert to Ebook arrow_right html、mobi、epub、pdf、rtf Template 

[ Shieber / Text2docx2pdf ](https://gitee.com/QMHTMY/Text2docx2pdf?_from=gitee_search)：linux下三个工具软件，可在命令行实现txt转docx，txt转pdf，docx转pdf。依赖python-docx

[mstamy2/PyPDF2](https://github.com/mstamy2/PyPDF2)：PyPDF2 是一个纯 Python PDF 库，能够分割、合并、裁剪和转换 PDF 文件页面

[Python 爬虫：把廖雪峰的教程转换成 PDF 电子书](https://gitee.com/liuzhijun1/crawler_html2pdf/tree/master/pdf)：pdfkit、wkhtmltopdf

[英强 / MD2File](https://gitee.com/cevin15/MD2File?_from=gitee_search)：Java - 文档导出工具类，能将markdown格式的内容，转为office word，PDF，HTML等等格式的文档。不使用markdown格式的内容，直接调用MD2File的api，生成word，pdf等文档也是可以的。 另外，还可以将MD2File作为markdown转HTML的工具类。

[Pandoc 使用及踩坑 点半久 Edited on 2019-08-31](https://www.dianbanjiu.com/post/pandoc-%E4%BD%BF%E7%94%A8%E5%8F%8A%E8%B8%A9%E5%9D%91/):Nice note, 不过同样是pacman安装的，我的没有sty文件问题。。或许是因为我安装了中文加持包的缘故吧


# End

