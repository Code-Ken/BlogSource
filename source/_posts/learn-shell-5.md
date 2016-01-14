title: Shell入门学习(五)
date: 2016-01-14 13:59:31
categories: Linux
tags: [Linux,Shell]
---

<center>Shell的学习手册(五)</center>
<!--more-->




# Shell重定向
Unix 命令默认从标准输入设备(stdin)获取输入,将结果输出到标准输出设备(stdout)显示。一般情况下,标准输入设备就是键盘,标准输出设备就是终端,即显示器。

输出重定向是大于号(>),输入重定向是小于号(<)。


```
$ who > users ＃输出重定向会覆盖文件内容
$ who >> users #使用 >> 追加到文件末尾
$ command < file #输入重定向,这样本来需要从键盘获取输入的命令会转移到文件读取内容
```

## 深入理解重定向
一般情况下,每个 Unix/Linux 命令运行时都会打开三个文件:

1.标准输入文件(stdin):stdin的文件描述符为0,Unix程序默认从stdin读取数据。
2.标准输出文件(stdout):stdout 的文件描述符为1,Unix程序默认向stdout输出数据。
3.标准错误文件(stderr):stderr的文件描述符为2,Unix程序会向stderr流中写入错误信息。

默认情况下,command > file 将 stdout 重定向到 file,command < file 将stdin 重定向到 file。

如果希望 stderr 重定向到 file,可以这样写:

```
$command 2 > file
$command 2 >> file
```

如果希望将 stdout 和 stderr 合并后重定向到 file,可以这样写:

```
$command > file 2>&1
$command >> file 2>&1
```

如果希望对 stdin 和 stdout 都重定向,可以这样写:

```
$command < file1 >file2
```

![](/shell-18.png)

## Here Document

Here Document 是 Shell 中的一种特殊的重定向方式,它的基本的形式如下:

```
command << delimiter
    document
delimiter
```

它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。

注意:

- 结尾的delimiter 一定要顶格写,前面不能有任何字符,后面也不能有任何字符,包括空格和 tab 缩进。
- 开始的delimiter前后的空格会被忽略掉。

实例:

![](/shell-19.png)


## /dev/null

如果希望执行某个命令,但又不希望在屏幕上显示输出结果,那么可以将输出重定向到 /dev/null

```
$ command > /dev/null
```

/dev/null 是一个特殊的文件,写入到它的内容都会被丢弃；如果尝试从该文件读取内容,那么什么也读不到。但是 /dev/null 文件非常有用,将命令的输出重定向到它,会起到”禁止输出“的效果。

如果希望屏蔽 stdout 和 stderr,可以这样写:

```
$ command > /dev/null 2>&1
```

# Shell文件包括

像其他语言一样,Shell 也可以包含外部脚本,将外部脚本的内容合并到当前脚本。
Shell 中包含脚本可以使用:

```
. filename
source filename
#两种方式的效果相同,简单起见,一般使用点号(.),但是注意点号(.)和文件名中间有一空格。
```

实例

```
#!/bin/bash
. ./subscript.sh
echo $url
```










# 参考文档
1.  [C语言中文网Shell教程](http://c.biancheng.net/cpp/shell/)
2. Linux命令行与Shell脚本编程大全 第2版









