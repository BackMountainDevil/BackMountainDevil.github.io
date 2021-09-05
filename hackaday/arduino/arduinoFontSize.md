---
title: "ArduinoFontSize"
date: 2021-06-04T12:35:47+08:00
lastmod: 2021-06-04T12:35:47+08:00
keywords: []
description: ""
tags: []
categories: [Blog]
author: "筱氚"
---
菜单字体太小，设置里没有可以更改的选项。

设置里只可以修改代码区（编辑器）的字体大小！

https://forum.arduino.cc/t/how-to-change-arduino-ide-menu-font-size/138861/7

尝试调节系统字体大小，重启 Arduino 发现没变。

修改 arduino 启动脚本（/usr/bin/arduino）办法尝试无效

```bash
# file /usr/bin/arduino
java -Dswing.defaultlaf=com.sun.java.swing.plaf.gtk.GTKLookAndFeel processing.app.Base "$@"
java processing.app.Base "$@"
JAVA_OPTIONS=("-DAPP_DIR=$APPDIR" "-Dswing.defaultlaf=com.sun.java.swing.plaf.nimbus.NimbusLookAndFeel")
JAVA_OPTIONS=("-DAPP_DIR=$APPDIR" "-Dswing.defaultlaf=com.sun.java.swing.plaf.motif.MotifLookAndFeel")
JAVA_OPTIONS=("-DAPP_DIR=$APPDIR")
```
