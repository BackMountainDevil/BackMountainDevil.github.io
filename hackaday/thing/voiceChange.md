# 变声器与声音模仿“造假”
- date: 2022-08-18
- lastmod: 2022-08-18

AI技术不断发展，换脸技术之后换声技术已经市场化应用了，主播变脸变声那都是家常便饭，相关事件参考“乔碧萝”。

针对换脸目前可以要求转头进行辨别，换声暂时不太清楚

## 方法

“甜酱捞面“ 回答了一种借助 Audacity 对录音进行变声的[简单办法](https://www.zhihu.com/question/35354987/answer/99064650)。打开软件后点击红色圆圈按钮进行录音，按下方块按钮结束录音，点击绿色的三角形播放听一下自己的声音；点击下面音轨中的波形，Ctrl+A 全选波形，之后从菜单栏选择“效果”-“改变音高”，在新窗口中改变音高或者频率，点击左下角的预览可以试听，合适后确定；此时再播放就会是变声之后的声音了，测试有效

- [lyrebird](https://github.com/lyrebird-voice-changer/lyrebird)：测试有效

- [Linux Voice Changer (Mumble, Team Speak). 2020](https://unix.stackexchange.com/questions/596309/linux-voice-changer-mumble-team-speak)

当然还有个5s即可模仿声音发[论文](https://matheo.uliege.be/handle/2268.2/6801)的，原文训练数据是英语，中文不是很适用，见[CorentinJ/Real-Time-Voice-Cloning ](https://github.com/CorentinJ/Real-Time-Voice-Cloning)。