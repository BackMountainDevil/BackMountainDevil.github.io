---
title: "MIPS_Dos"
date: 2020-11-06T14:21:37+08:00
lastmod: 2020-11-06T14:21:37+08:00
keywords: [MIPS, DOS]
description: ""
tags: [MIPS]
categories: [MIPS]
author: "筱氚"
---
# 功能号1 键盘输入
键盘输入的字符以ASCII码的形式放在累加器AL中，同时，显示出来。
```bash
MOV AH,01H
INT 21H
```
# 功能号2 字符显示
屏幕显示存放在DL寄存器中的字符。
```bash
MOV DL, <要显示的字符> ；
MOV AH,02H ;功能号02H→AH
INT 21H ;执行系统功能调用
```
# 功能号9 显示器显示字符串
用于在显示器上显示一个字符串，被显示的字符串必须以“＄”字符作为结束符。
```bash
MOV DX, <要显示字符串的首地址> ；
MOV AH,09H ;功能号09H→AH
INT 21H ;执行系统功能调用
```
# 功能号4C 返回DOS
一个程序执行完后，应使程序正常退出并返回到DOS，可使用DOS系统功能调用的4CH号功能。
```bash
MOV AH,4CH ;功能号送AH
INT 21H ;返回DOS
```
# 子程序
子程序调用指令：CALL和RET
子程序定义格式为：
```bash
子程序名 PROC [NEAR/FAR]
        ┆
        RET
子程序名 ENDP
```
# 跳转指令
JZ ZERO ; x=0转ZERO
JNS PLUS ;x>0转PLUS

# 比较指令
JL/JNGE     小于转移

# References
- [x86汇编指令集大全（带注释）](https://blog.csdn.net/qq_35939148/article/details/90354555)
- [汇编语言--常用DOS功能](https://blog.csdn.net/weixin_44225182/article/details/101051041)