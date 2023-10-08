# TTS 文本转语音
- date: 2023-8-11
- lastmod: 2023-10-8

最近在使用 firefox 的时候，最上面会提示我没有安装 Speech Dispatcher，打开其提示链接才知道其是一个视障帮助工具，可以用来朗读网页，同时页面上可以看到不同 linux 发行版的安装方式了。这个朗读网页的工具在 edge 浏览器是默认安装的，linux 主打一个需要再装。语音转文本的话可以看看 whisper

我在 pamac 里安装 speech-dispatcher 提示我从 festival 或 espeak-ng 选一个作为依赖，我就选了后者。然后在 wiki 找到了如何测试的方式

> spd-say "Arch Linux is the best"

说人话就是，它朗读了这句话，或者是文本转语音，效果一听就知道是一个电子合成音，吐字不清晰。然后我尝试安装 festival 并卸载 espeak-ng，然后就不正常朗读了，又重装 espeak-ng 就又能朗读了。wiki 有说这种情况可能是没开服务

## espeak-ng

espeak 开发不活跃，就有人开新分支搞了 espeak-ng，espeak 以及被丢到 AUR 里了，下面的几行指令可以看出来安装 espeak-ng 之后也会安装 espeak，不过 espeak 只是指向 espeak-ng 的快捷方式而已。espeak 的首页说其支持粤语，espeak-ng 增加了很多语音支持，其中包括普通话

```bash
$ which espeak
/usr/bin/espeak
$ which espeak-ng
/usr/bin/espeak-ng
$ file /usr/bin/espeak
/usr/bin/espeak: symbolic link to espeak-ng
```

指令 `espeak-ng --voices` 可查看其支持的语言，下面是看一下中文支持

```bash
$ espeak-ng --voices | grep Chinese
 5  cmn             --/M      Chinese_(Mandarin,_latin_as_English) sit/cmn              (zh-cmn 5)(zh 5)
 5  cmn-latn-pinyin --/M      Chinese_(Mandarin,_latin_as_Pinyin) sit/cmn-Latn-pinyin  (zh-cmn 5)(zh 5)
 5  hak             --/M      Hakka_Chinese      sit/hak              
 5  yue             --/M      Chinese_(Cantonese) sit/yue              (zh-yue 5)(zh 8)
 5  yue             --/M      Chinese_(Cantonese,_latin_as_Jyutping) sit/yue-Latn-jyutping (zh-yue 5)(zh 8)
```

```bash
espeak-ng "解放全人类" # 由于没有指定语言，默认为英文，会念好几次 "Chinese letter"
espeak-ng -v yue "解放全人类" # 粤语朗读，测试通过
espeak-ng -v cmn-latn-pinyin "解放全人类" # 普通话朗读，测试通过，老外念中文的那种感觉
espeak-ng -v cmn-latn-pinyin "jie fang quan ren lei" # 居然读出来和上面一样，看样子声调掌握的还可以
espeak-ng -v cmn-latn-pinyin "12345" # 读出来是“十二千三百四十五”，说明在分词上是按照英文的三位数字拆分的

espeak-ng -v cmn "jie fang quan ren lei"  # ！！！不知道念的什么
```

## festival

安装 festival festival-english festival-freebsoft-utils 之后，首先要开启后台服务

```bash
festival --server &
```

然后使用命令 `spd-conf --test-festival` 来测试该服务

```
$ festival --server &
[1] 54884
[mifen@hp ~]$ server    Sun Aug 13 10:21:50 2023 : Festival server started on port 1314
client(1) Sun Aug 13 10:21:53 2023 : accepted from localhost
client(1) Sun Aug 13 10:21:53 2023 : disconnected

$ spd-conf --test-festival

Speech Dispatcher configuration tool

Testing whether Festival works as a server
Festival contains freebsoft-utils.
Festival server seems to work correctly
```

使用其进行文本转语音

## ekho

这个包是支持普通话、粤语等口语朗读的，在 archlinuxcn 中可以直接安装，不需要自己去编译操作。一下是两行测试，第一行是普通话，第二行是用粤语朗读

```bash
ekho "解放全人类"
ekho -v Cantonese "解放全人类"
```

# 参考

[Speech dispatcher](https://wiki.archlinux.org/title/Speech_dispatcher)

[eGuideDog 黄冠能](http://www.eguidedog.net/cn/index.php)：ekho 是其中一个子项目

[Accessibility](https://wiki.archlinux.org/title/List_of_applications/Other#Accessibility)：无障碍应用，如语音识别、朗读、鼠标自动点击

[如何使用火狐浏览器朗读网页文本 2020年5月5日 逸云蓝天](https://www.eskysky.com/1701.html):“切换阅读器视图”的快捷键是 ctrl+alt+r
> 打开一个新闻网页。可以看到，地址栏上有一个“切换阅读器视图”的按钮
> 如果你的火狐浏览器没有这个“切换阅读器视图”的按钮也不用着急，我们可以通过在网址前加上“about:reader?url=”的方式强制打开阅读模式

[PaddleSpeech](https://gitee.com/paddlepaddle/PaddleSpeech)

[read-aloud 浏览器插件](https://github.com/ken107/read-aloud/discussions/339)：在火狐上装了可以直接用，声音很像机器合成的
