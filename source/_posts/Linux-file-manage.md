
title: Linux文件管理
date: 2016-01-23 12:17:13
categories: Linux
tags: [Linux]
---

<center>本文主要介绍linux下关于文件管理的一些操作</center>
<!--more-->

## 目录基本操作

### ls命令

显示目标列表

#### 语法

```
ls     (选项)    (参数)
```

#### 选项

常用选项

```
-a：显示所有档案及目录（ls内定将档案名或目录名称为“.”的视为隐藏，不会列出）； 
-A：显示除影藏文件“.”和“..”以外的所有文件列表；
-l：所有输出信息用单列格式输出，不输出为多列；
-i：显示文件索引节点号（inode）。一个索引节点代表一个文件；
-s：显示文件和目录的大小，以区块为单位；
```

#### 参数

目录：指定要显示列表的目录，也可以是具体的文件

#### 实例

```
[root@AY130520180402842260Z home]# ls
2.php  a    Auth.class.php  cmd.php     empty.php     global.php  logs     pheanstalk  pids     s_w.php         utils.php    worker.php
3.php  api  b.sh            config.php  gen_addr.php  inc         modules  php.txt     s_c.php  task_state.php  watcher.php

[root@AY130520180402842260Z home]# ls -sail
total 144
2614648  4 drwxr-xr-x  9 root root  4096 Jan 13 16:58 .
2614647  4 drwxr-xr-x  3 root root  4096 Oct 21 10:23 ..
2615501  4 -rw-r--r--  1 root root   500 Jan 13 16:58 2.php
2615486  4 -rw-r--r--  1 root root   555 Sep  2 17:09 3.php
2630038  4 drwxr-xr-x  2 root root  4096 Nov 20 14:34 a
2614771  4 drwxr-xr-x 21 root root  4096 Aug 21 15:54 api
2615484  4 -rw-r--r--  1 root root  3719 Sep  2 17:01 Auth.class.php
2615485  4 -rw-r--r--  1 root root    59 Sep  2 14:40 b.sh
2615405  4 -rw-r--r--  1 root root  2717 Apr 22  2015 cmd.php
2615594  4 -rw-r--r--  1 root root   887 Aug  4 10:46 config.php
2615408  4 -rw-r--r--  1 root root   616 May  5  2015 empty.php
2615412  4 -rw-r--r--  1 root root  2012 Apr 22  2015 gen_addr.php
2615410  4 -rw-r--r--  1 root root  2208 Apr 22  2015 global.php
2621841  4 drwxr-xr-x  2 root root  4096 Apr 22  2015 inc
2621544  4 drwxr-xr-x 24 root root  4096 Jan 15 11:21 logs
2614653  4 drwxr-xr-x 10 root root  4096 Aug  4 13:48 modules
2621791  4 drwxr-xr-x  3 root root  4096 Apr 22  2015 pheanstalk
2615479 32 -rw-r--r--  1 root root 31305 Sep  2 10:21 php.txt
2621789  4 drwxr-xr-x  2 root root  4096 Apr 22  2015 pids
2615500  4 -rw-r--r--  1 root root   365 Jan 11 18:01 s_c.php
2615499  4 -rw-r--r--  1 root root   539 Jan 11 18:01 s_w.php
2615407  4 -rw-r--r--  1 root root   484 Apr 22  2015 task_state.php
2615487 12 -rw-r--r--  1 root root  9805 Sep  2 15:47 utils.php
2614651  4 -rw-r--r--  1 root root  3734 Apr 22  2015 watcher.php
2615404 12 -rw-r--r--  1 root root  9855 Apr 22  2015 worker.php

[root@AY130520180402842260Z home]# 
```

### pwd命令

pwd命令以绝对路径的方式显示用户当前工作目录

#### 语法

```
pwd
```

#### 实例

```
[root@AY130520180402842260Z home]# pwd
/root/home
```

### cd命令

cd命令用来切换工作目录至dirname。

#### 语法

```
cd dirname
```

#### 实例

```
cd 进入用户主目录； 
cd ~ 进入用户主目录； 
cd - 返回进入此目录之前所在的目录； 
cd .. 返回上级目录
cd ../.. 返回上两级目录； 
cd !$ 把上个命令的参数作为cd参数使用
```

### mv命令

mv命令用来对文件或目录重新命名，或者将文件从一个目录移到另一个目录中
`注意事项`：mv与cp的结果不同，mv好像文件“搬家”，文件个数并未增加。而cp对文件进行复制，文件个数增加了。

#### 语法

```
mv source target
```

#### 实例

```
#将文件ex3改名为new1 
mv ex3 new1 
#将目录/usr/men中的所有文件移到当前目录（用.表示）中： 
mv /usr/men/* .
```

### cp命令

cp命令用来将一个或多个源文件或者目录复制到指定的目的文件或目录。

#### 语法

```
cp source target
```

#### 实例

```
#将文件file复制到目录/usr/下，并改名为file1 
cp file /usr/file1
```

### rm命令

rm命令可以删除一个目录中的一个或多个文件或目录，也可以将某个目录及其下属的所有文件及其子目录均删除掉。对于链接文件，只是删除整个链接文件，而原有文件保持不变。

#### 语法

```
rm     (选项)    (参数)
```

#### 选项

```
-d：直接把欲删除的目录的硬连接数据删除成0，删除该目录； 
-f：强制删除文件或目录； 
-i：删除已有文件或目录之前先询问用户； 
-r或-R：递归处理，将指定目录下的所有文件与子目录一并处理； 
```

#### 参数

文件：指定被删除的文件列表，如果参数中含有目录，则必须加上-r或者-R选项。


#### 实例

```
[root@AY130520180402842260Z ~]# rm hello.php 
```

### mkdir命令
 
mkdir命令用来创建目录

#### 语法

```
mkdir 	(选项)	(参数)
```

#### 选项

```
-m<目标属性>或--mode<目标属性>建立目录的同时设置目录的权限；
```

#### 参数

目录：指定要创建的目录列表，多个目录之间用空格隔开。

#### 实例

```
mkdir -m 700 dirname
```

### rmdir命令

用来删除`空目录`。

#### 语法

```
rmdir dirname
```

#### 实例

```
rmdir emptyfile
```

## 文件内容查看

### tail命令

tail命令用于输入文件中的尾部内容。tail命令默认在屏幕上显示指定文件的末尾10行。如果给定的文件不止一个，则在显示的每个文件前面加一个文件名标题。如果没有指定文件或者文件名为“-”，则读取标准输入。

#### 语法

```
tail	(选项)	(参数)
```

#### 选项

```
-n或——line=：输出文件的尾部N（N位数字）行内容
+<N>:输出文件从第N行开始到文件尾部的内容
-c或——bytes=：输出文件尾部的N（N为整数）个字节内容；
```

#### 参数

文件名称列表

#### 实例


```
tail file （显示文件file的最后10行） 
tail +20 file （显示文件file的内容，从第20行至文件末尾） 
tail -n 20 file （显示文件file的最后20行）
tail -c 10 file （显示文件file的最后10个字符)
```

### head命令

head命令用于显示文件的开头的内容。在默认情况下，head命令显示文件的头10行内容。

#### 语法

```
head  (选项)  (参数)
```

#### 选项

```
-n<数字>：指定显示头部内容的行数； 
-c<字符数>：指定显示头部内容的字符数;
```

#### 参数
文件列表：指定显示头部内容的文件列表。

### more命令

more命令是一个基于vi编辑器文本过滤器，它以全屏幕的方式按页显示文本文件的内容，
支持vi中的关键字定位操作。
more名单中内置了若干快捷键，常用的有

- H（获得帮助信息）
- Enter（向下翻滚一行）
- 空格（向下滚动一屏）
- B(向前滚动一屏)
- Q（退出命令）

#### 语法

```
more     (选项)    (参数)
```

#### 选项

```
-<数字>：指定每屏显示的行数； 
-d：显示“[press space to continue,'q' to quit.]”和“[Press 'h' for instructions]”； 
-c：不进行滚屏操作。每次刷新这个屏幕； 
-s：将多个空行压缩成一行显示； 
-u：禁止下划线； 
+<数字>：从指定数字的行开始显示。
```

#### 参数

文件：指定分页显示内容的文件。


### less命令
 less命令的作用与more十分相似，都可以用来浏览文字档案的内容，不同的是less命令允许用户向前或向后浏览文件，而more命令只能向前浏览。用less命令显示文件时，用PageUp键向上翻页，用PageDown键向下翻页。要退出less程序，应按Q键。


## 文件查找和比较

### which命令

which命令用于查找并显示给定命令的绝对路径，环境变量PATH中保存了查找命令时需要遍历的目录。which指令会在环境变量$PATH设置的目录里查找符合条件的文件。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。

#### 实例

```
[root@AY130520180402842260Z home]# which php
/usr/local/bin/php
```

### whereis命令
which只能查找二进制程序
whereis命令用来定位指令的二进制程序、源代码文件和man手册页等相关文件的路径。 

whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。 


### find命令

#### 语法

```
find(选项)(参数)
```

#### 选项

```
-name<范本样式>：指定字符串作为寻找文件或目录的范本样式；
-iname<范本样式>：此参数的效果和指定“-name”参数类似，但忽略字符大小写的差别；

-path<范本样式>：指定字符串作为寻找目录的范本样式；
-ipath<范本样式>：此参数的效果和指定“-path”参数类似，但忽略字符大小写的差别； 

-regex<范本样式>：指定字符串作为寻找文件或目录的范本样式；
-iregex<范本样式>：此参数的效果和指定“-regexe”参数类似，但忽略字符大小写的差别；

-type<文件类型>：只寻找符合指定的文件类型的文件；
	类型参数列表： f 普通文件 l 符号连接 d 目录 c 字符设备 b 块设备 s 套接字 p Fifo

-maxdepth<目录层级>：设置最大目录层级； 
-mindepth<目录层级>：设置最小目录层级；

-amin<分钟>：查找在指定时间曾被存取过的文件或目录，单位以分钟计算； 
-cmin<分钟>：查找在指定时间之时被更改过的文件或目录； 
-mmin<分钟>：查找在指定时间曾被更改过的文件或目录，单位以分钟计算； 

-atime<24小时数>：查找在指定时间曾被存取过的文件或目录，单位以24小时计算； 
-ctime<24小时数>：查找在指定时间之时被更改的文件或目录，单位以24小时计算；
-mtime<24小时数>：查找在指定时间曾被更改过的文件或目录，单位以24小时计算；

-size<文件大小>：查找符合指定的文件大小的文件；

-delete：删除符号要求的文件

-perm<权限数值>：查找符合指定的权限数值的文件或目录；

-group<群组名称>：查找符合指定之群组名称的文件或目录；
-nogroup：找出不属于本地主机群组识别码的文件或目录；

-user<拥有者名称>：查找符和指定的拥有者名称的文件或目录；
-nouser：找出不属于本地主机用户识别码的文件或目录；

-empty  列出所有为空的文件

```

#### 实例


```

#列出当前目录及子目录下所有文件和文件夹 
find .

#在/home目录下查找以.txt结尾的文件名
find /home -name "*.txt"

#同上，但忽略大小写 
find /home -iname "*.txt"

#当前目录及子目录下查找所有以.txt和.pdf结尾的文件 
find . \( -name "*.txt" -o -name "*.pdf" \) 
#或
find . -name "*.txt" -o -name "*.pdf" 

#匹配文件路径或者文件 
find /usr/ -path "*local*"

#基于正则表达式匹配文件路径 
find . -regex ".*\(\.txt\|\.pdf\)$"

#同上，但忽略大小写 
find . -iregex ".*\(\.txt\|\.pdf\)$"

#找出/home下不是以.txt结尾的文件 
find /home ! -name "*.txt" 

#根据文件类型进行搜索 
find . -type 类型参数 

#基于目录深度搜索 向下最大深度限制为3 
find . -maxdepth 3 -type f 

#搜索出深度距离当前目录至少2个子目录的所有文件 
find . -mindepth 2 -type f


#UNIX/Linux文件系统每个文件都有三种时间戳： 
#访问时间（-atime/天，-amin/分钟）：用户最近一次访问时间。 
#修改时间（-mtime/天，-mmin/分钟）：文件最后一次修改时间。 
#变化时间（-ctime/天，-cmin/分钟）：文件数据元（例如权限等）最后一次修改时间。

#搜索最近七天内被访问过的所有文件 
find . -type f -atime -7 

#搜索恰好在七天前被访问过的所有文件 
find . -type f -atime 7 

#搜索超过七天内被访问过的所有文件 
find . -type f -atime +7


#根据文件大小进行匹配 
find . -type f -size 文件大小单元 
#文件大小单元： b —— 块（512字节） c —— 字节 w —— 字（2字节） k —— 千字节 M —— 兆字节 G —— 吉字节 
#搜索大于10KB的文件 
find . -type f -size +10k

#搜索小于10KB的文件 
find . -type f -size -10k 

#搜索等于10KB的文件 
find . -type f -size 10k

#删除当前目录下所有.txt文件 
find . -type f -name "*.txt" -delete

#找出当前目录下权限不是644的php文件 
find . -type f -name "*.php" ! -perm 644

#找出当前目录用户tom拥有的所有文件 
find . -type f -user tom 
#找出当前目录用户组sunk拥有的所有文件 
find . -type f -group sunk

#要列出所有长度为零的文件 
find . -empty

```


### locate/slocate命令

locate命令和slocate命令都用来查找文件或目录。 
locate命令其实是find -name的另一种写法，但是要比后者快得多，原因在于它不搜索具体目录，而是搜索一个数据库/var/lib/locatedb，这个数据库中含有本地所有文件信息。Linux系统自动创建这个数据库，并且每天自动更新一次，所以使用locate命令查不到最新变动过的文件。为了避免这种情况，可以在使用locate之前，先使用`updatedb`命令，手动更新数据库。


#### 实例

```
搜索用户主目录下，所有以m开头的文件 
locate ~/m
```


### diff命令

diff命令在最简单的情况下，比较给定的两个文件的不同。
diff命令是以逐行的方式，比较文本文件的异同处。
如果该命令指定进行目录的比较，则将会比较该目录中具有相同文件名的文件，而不会对其子目录文件进行任何比较操作。


#### 实例

```

localhost:Documents Ken$ diff aaaaaaa bbbbbbbb 
1c1
< aaaaaaa
\ No newline at end of file
---
> bbbbbbbb

```


### cmp命令

cmp命令用来比较两个文件是否有差异。当相互比较的两个文件完全一样时，则该指令不会显示任何信息。
若发现有差异，预设会标示出第一个不通之处的字符和列数编号

#### 实例

```
localhost:Documents Ken$ cmp aaaaaaa bbbbbbbb 
aaaaaaa bbbbbbbb differ: char 1, line 1
```


## 文件处理

### iconv命令

用来转换文件的编码方式，比如它可以将UTF8编码的转换成GB18030的编码

#### 语法

```
iconv -f encoding [-t encoding] [inputfile]... 
```

#### 选项

```
-f encoding :把字符从encoding编码开始转换。 
-t encoding :把字符转换到encoding编码。 
-l :列出已知的编码字符集合 
-o file :指定输出文件 
-c :忽略输出的非法字符 
-s :禁止警告信息，但不是错误信息 
--verbose :显示进度信息

```


#### 实例

```
#1. 文件装换
iconv -f ISO88592 -t UTF8 < input.txt > -o < output.txt >
#ISO88592：原编码格式
#UTF8：要转换的编码格式

#2. 转换网页
curl -s http://www.dreamdu.com/ | iconv -f utf8 -t gbk
curl -s http://www.google.com.hk/ | iconv -f big5 -t gbk
```



### touch命令

touch命令有两个功能：

- 是用于把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来；
- 是用来创建新的空文件。 语法 touch(选项)(参数) 

#### 实例 

```
touch demo.php
```

### unlink命令

同`rm`，都是用于删除文件


## 文件权限属性设置

### chmod命令

chmod命令用来变更文件或目录的权限。

在UNIX系统家族里，文件或目录权限的控制分别以读取、写入、执行3种一般权限来区分，另有3种特殊权限可供运用。用户可以使用chmod指令去变更文件与目录的权限，设置方式采用文字或数字代号皆可。符号连接的权限无法变更，如果用户对符号连接修改权限，其改变会作用在被连接的原始文件。 

权限范围的表示法如下： 

```
u User，即文件或目录的拥有者； 
g Group，即文件或目录的所属群组； 
o Other，除了文件或目录拥有者或所属群组之外，其他用户皆属于这个范围； 
a All，即全部的用户，包含拥有者，所属群组以及其他用户； 
r 读取权限，数字代号为“4”; 
w 写入权限，数字代号为“2”； 
x 执行或切换权限，数字代号为“1”； 
- 不具任何权限，数字代号为“0”； 
s 特殊功能说明：变更文件或目录的权限。
```

#### 实例

![](http://man.linuxde.net/wp-content/uploads/2013/11/chmod.gif)

```
chmod u+x,g+w f01　　//为文件f01设置自己可以执行，组员可以写入的权限 
chmod u=rwx,g=rw,o=r f01 
chmod 764 f01 
chmod a+x f01　　//对文件f01的u,g,o都设置可执行属性

chown user:market f01　　//把文件f01给uesr，添加到market组 
```


### stat命令

stat命令用于显示文件的状态信息。stat命令的输出信息比ls命令的输出信息要更详细。

### file命令

file命令用来探测给定文件的类型。file命令对文件的检查分为文件系统、魔法幻数检查和语言检查3个过程

#### 实例

```
localhost:Documents Ken$ file aaaaaaa 
aaaaaaa: ASCII text, with no line terminators
```

## 文件过滤分割与合并

### grep命令

grep是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。

#### 选项

```
-c 计算符合范本样式的列数
-w 只显示全字符合的列 
-x 只显示全列符合的列
-i 忽略字符大小写的差别
-y 此参数效果跟“-i”相同 
-o 只输出文件中匹配到的部分
-n 在显示符合范本样式的那一列之前，标示出该列的编号
-l 列出文件内容符合指定的范本样式的文件名称
-E 使用扩展正则表达式
-q 不显示任何信息
-R/-r 递归的查找
```

#### 实例

```
#在文件中搜索一个单词，命令会返回一个包含“match_pattern”的文本行： 
grep "match_pattern" file_name 

#在多个文件中查找： 
grep "match_pattern" file_1 file_2 file_3 ...

#标记匹配颜色 --color=auto 选项： 
grep "match_pattern" file_name --color=auto

#使用正则表达式 -E 选项： 
grep -E "[1-9]+"  file_name

#统计文件或者文本中包含匹配字符串的行数 -c 选项： 
grep -c "text" file_name


#输出包含匹配字符串的行数 -n 选项： 
grep "text" -n file_name 
#多个文件 
grep "text" -n file_1 file_2

#搜索多个文件并查找匹配文本在哪些文件中： 
grep -l "text" file1 file2 file3...

#在多级目录中对文本进行递归搜索： 
grep "text" . -r -n

#在grep搜索结果中包括或者排除指定文件： 
#只在目录中所有的.php和.html文件中递归搜索字符"main()" 
grep "main()" . -r --include *.{php,html} 
#在搜索结果中排除所有README文件 
grep "main()" . -r --exclude "README" 
#在搜索结果中排除filelist文件列表里的文件 
grep "main()" . -r --exclude-from filelist
```

### wc命令
wc命令用来计算数字。利用wc指令我们可以计算文件的Byte数、字数或是列数，若不指定文件名称，或是所给予的文件名为“-”，则wc指令会从标准输入设备读取数据。 

#### 语法 

```
wc(选项)(参数) 
```

#### 选项 

```
-c或--bytes或——chars：只显示Bytes数； 
-l或——lines：只显示列数； 
-w或——words：只显示字数。
```

#### 实例

```
localhost:Server-v1 Ken$ wc  api.php 
      94     248    2096 api.php
localhost:Server-v1 Ken$ wc -w  api.php 
     248 api.php
localhost:Server-v1 Ken$ wc -l  api.php 
      94 api.php
localhost:Server-v1 Ken$ wc -c  api.php 
    2096 api.php

```

## 文件压缩与解压

### tar命令 

利用tar命令，可以把一大堆的文件和目录全部打包成一个文件，这对于备份文件或将几个文件组合成为一个文件以便于网络传输是非常有用的。

首先要弄清两个概念：`打包和压缩`

- 打包是指将一大堆文件或目录变成一个总的文件；
- 压缩则是将一个大的文件通过一些压缩算法变成一个小文件。 

为什么要区分这两个概念呢？

>这源于Linux中很多压缩程序只能针对一个文件进行压缩，这样当你想要压缩一大堆文件时，你得先将这一大堆文件先打成一个包（tar命令），然后再用压缩程序进行压缩（gzip bzip2命令）。

#### 语法 

```
tar (选项)  (参数)
```

#### 选项 

```
-A或--catenate：新增文件到以存在的备份文件； 
-B：设置区块大小； 
-c或--create：建立新的备份文件； 
-C <目录>：这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项。 
-d：记录文件的差别； 
-x或--extract或--get：从备份文件中还原文件； 
-t或--list：列出备份文件的内容； 
-z或--gzip或--ungzip：通过gzip指令处理备份文件； 
-Z或--compress或--uncompress：通过compress指令处理备份文件； 
-f: 使用档名，请留意，在 f 之后要立即接档名喔！不要再加参数！
-v或--verbose：显示指令执行过程； 
-r：添加文件到已经压缩的文件； 
-u：添加改变了和现有的文件到已经存在的压缩文件； 
-j：支持bzip2解压文件； 
-v：显示操作过程； 
-l：文件系统边界设置； 
-k：保留原有文件不覆盖； 
-m：保留文件不被覆盖； 
-w：确认压缩文件的正确性；
-x：解压 
-p或--same-permissions：用原来的文件权限还原文件； 
-P或--absolute-names：文件名使用绝对名称，不移除文件名称前的“/”号； 
```

#### 参数 

```
文件或目录：指定要打包的文件或目录列表。
```

#### 实例

```
#范例一：将整个 /etc 目录下的文件全部打包成为 /tmp/etc.tar
[txw1958@redhat ~]# tar -cvf /tmp/etc.tar /etc      　　　　<==仅打包，不压缩！ 
[txw1958@redhat ~]# tar -zcvf /tmp/etc.tar.gz /etc  　　　　<==打包后，以 gzip 压缩 
[txw1958@redhat ~]# tar -jcvf /tmp/etc.tar.bz2 /etc　　　　 <==打包后，以 bzip2 压缩 
# 特别注意，在参数 f 之后的文件档名是自己取的，我们习惯上都用 .tar 来作为辨识。 
# 如果加 z 参数，则以 .tar.gz 或 .tgz 来代表 gzip 压缩过的 tar file ～ 
# 如果加 j 参数，则以 .tar.bz2 来作为附档名啊～ 
# 上述指令在执行的时候，会显示一个警告讯息： 
# 『tar: Removing leading `/" from member names』那是关於绝对路径的特殊设定。

#范例二：查阅上述 /tmp/etc.tar.gz 文件内有哪些文件？
[txw1958@redhat ~]# tar -ztvf /tmp/etc.tar.gz 
# 由於我们使用 gzip 压缩，所以要查阅该 tar file 内的文件时， 
# 就得要加上 z 这个参数了！这很重要的！

#范例三：将 /tmp/etc.tar.gz 文件解压缩在 /usr/local/src 底下
[txw1958@redhat src]# tar -zxvf /tmp/etc.tar.gz 
# 在预设的情况下，我们可以将压缩档在任何地方解开的！以这个范例来说， 
# 我先将工作目录变换到 /usr/local/src 底下，并且解开 /tmp/etc.tar.gz ， 
# 则解开的目录会在 /usr/local/src/etc 呢！另外，如果您进入 /usr/local/src/etc 
# 则会发现，该目录下的文件属性与 /etc/ 可能会有所不同喔！

#范例四：在 /tmp 底下，我只想要将 /tmp/etc.tar.gz 内的 etc/passwd 解开而已
[txw1958@redhat ~]# cd /tmp 
[txw1958@redhat tmp]# tar -zxvf /tmp/etc.tar.gz etc/passwd 
# 我可以透过 tar -ztvf 来查阅 tarfile 内的文件名称，如果单只要一个文件， 
# 就可以透过这个方式来下达！注意到！ etc.tar.gz 内的根目录 / 是被拿掉了！

```
















































































## 参考文献
1. [Linux命令大全](http://man.linuxde.net/)
2. [方倍工作室](http://www.cnblogs.com/txw1958/archive/2012/09/13/linux-tar.html)
















