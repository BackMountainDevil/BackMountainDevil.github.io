---
title: "美观带颜色的控制台菜单"
date: 2021-07-14T10:47:28+08:00
lastmod: 2021-07-14T10:47:28+08:00
keywords: []
description: ""
tags: []
categories: [Blog]
author: "筱氚"
---
# 简介
## v1
控制台菜单是不少刚开始学编程的人都要面临的问题，最后大部分都是这样子的，大体上都一个[模子](https://www.w3cschool.cn/java/codedemo-484048230.html)，这里将 ta 称之为 v1。 首先输出菜单项和其对应的编号，然后用户输入编号进行操作。其实这是有进一步提升的空间的。
```cpp
  cout << "|   MENU SELECTION DEMO    |" << endl;
  cout << "| Options:                 |" << endl;
  cout << "|        1. Option 1       |" << endl;
  cout << "|        2. Option 2       |" << endl;
  cout << "|        3. Option 3       |" << endl;
  cout << "|        4. Exit           |" << endl;
  cout << "Select option: ";
  int command = -1;
  do {
    cin >> command;
    if (command < 1 || command > 4) {
      cout << "Unknown option: " << command << ". Please choose again: ";
    }
  } while (command < 1 || command > 4);
```

之前编译过 Linux 内核，ta 的菜单也是在控制台里的，之后在[ESP - IDF](../espidf)中也看到了类似的菜单，两者大体上和 BIOS 操作逻辑差不多，比 v1 多了啥呢？这里将这个类型归类为 v2

## v2
使用一下就会发现，v2 比 v1 多出的点：
- 键盘上下键选择菜单项，回车确认，无需输入对应的菜单项编号
- 支持多个功能键： 如 F10
- 带多种颜色
- 带长宽自适应

## v3

# 参考
- [C语言实现一个Window控制台带彩色，且可以用方向键选择并确认的菜单式列表(一)Engineer-Bruce_Yang 2017-08-22](https://yangyuanxin.blog.csdn.net/article/details/77485367)：这就是我想要的效果，看起来极像 GUI,但实际不是， IDF 中也有类似的菜单
> 比如Linux内核中的Make menuconfig。
我们可以看到像下面这样的GUI菜单式界面，这样的界面用Linux VT100控制码结合C语言是很容易就可以实现出来的。
- [Java I/O - 如何创建控制台菜单](https://www.w3cschool.cn/java/codedemo-484048230.html):最基本的菜单
- [漂亮的C++控制台界面（格式化输入输出）慕宵子 2015-07-03](https://blog.csdn.net/u011134502/article/details/46746823);无预览图，比 print 多了点居中，谈不上漂亮
> 现在呢，它不仅可以格式化输出，还可以格式化输入，并且可以根据数据的长度自由控制表格的宽度，标题和表格都是居中显示的。但是，它也有一些缺点，就是如果数据过于长且超过窗口的最大宽度，它会乱行吐舌头
- [Shang / SimpleMenu ](https://gitee.com/shangcode_happy/SimpleMenu)：无预览，无说明，无法鉴别效果
> 用C语言写的一个运行在Windows控制台内的菜单。