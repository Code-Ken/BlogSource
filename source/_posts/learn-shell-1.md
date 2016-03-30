title: Shell入门学习(一)
date: 2016-01-14 13:53:31
categories: Linux
tags: [Linux,Shell]
---

<center>Shell的学习手册(一)</center>
<!--more-->
​
# Shell介绍
## 定义
Shell本身是一个用C语言编写的程序,它是用户使用Unix/Linux的桥梁,用户的大部分工作都是通过Shell完成的。
`Shell既是一种命令语言,又是一种程序设计语言`
作为命令语言,它交互式地解释和执行用户输入的命令；
作为程序设计语言,它定义了各种变量和参数,并提供了许多在高级语言中才具有的控制结构,包括循环和分支。

## Shell有两种执行命令的方式
- 交互式(Interactive):解释执行用户的命令,用户输入一条命令,Shell就解释执行一条。
- 批处理(Batch):用户事先写一个Shell脚本(Script),其中有很多条命令,让Shell一次把这些命令执行完,而不必一条一条地敲命令。

# 几种常见Shell
Shell是一种脚本语言,那么,就必须有解释器来执行这些脚本。
我们常说有多少种Shell,其实说的是Shell脚本解释器:
- bash <=bash是Linux标准默认的shell,本文的Shell都是基于bash环境
- sh
- ash
- csh
- ksh

# 第一个Shell脚本
打开文本编辑器,新建一个文件,扩展名为sh(sh代表shell),扩展名并不影响脚本执行,但一般都以`sh`做扩展名。
## 编写Shell
输入一些代码:

```
#!/bin/bash   <=“#!” 是一个约定的标记,它告诉系统这个脚本需要什么解释器来执行,即使用哪一种Shell
echo "Hello World !" <=echo命令用于向窗口输出文本
```

## 运行Shell
两种方式运行Shell
1.作为可执行程序
将上面的代码保存为test.sh,并cd到相应目录:

```
chmod +x ./test.sh  #使脚本具有执行权限 
./test.sh  #执行脚本
```

`注意,一定要写成./test.sh,而不是test.sh`
>直接写test.sh,linux系统会去PATH里寻找有没有叫test.sh的,而只有/bin, /sbin, /usr/bin,/usr/sbin等在PATH里,你的当前目录通常不在PATH里,所以写成test.sh是会找不到命令的,要用./test.sh告诉系统说,就在当前目录找。

2.作为解释器参数

这种运行方式是,直接运行解释器,其参数就是shell脚本的文件名,如:
```
/bin/sh test.sh
```
![](/shell-1.png)