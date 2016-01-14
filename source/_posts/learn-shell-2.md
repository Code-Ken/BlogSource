title: Shell入门学习(二)
date: 2016-01-14 13:53:31
categories: Linux
tags: [Linux,Shell]
---

<center>Shell的学习手册(二)</center>
<!--more-->



# Shell语法
## Shell变量
运行shell时,会同时存在三种变量:
1) 局部变量
局部变量在脚本或命令中定义,仅在当前shell实例中有效,其他shell启动的程序不能访问局部变量。
2) 环境变量
所有的程序,包括shell启动的程序,都能访问环境变量,有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3) shell变量
shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量,有一部分是局部变量,这些变量保证了shell的正常运行

### 局部变量
#### 定义
`定义变量时,变量名不加美元符号($)!!!`如:

```
variableName="value"
```

`注意,变量名和等号之间不能有空格`

变量名的命名须遵循如下规则:
- 首个字符必须为字母(a-z,A-Z)。
- 中间不能有空格,可以使用下划线(_)。
- 不能使用标点符号。
- 不能使用bash里的关键字(可用help命令查看保留关键字)

#### 使用
使用一个定义过的变量要在变量名前面加美元符号($),如

```
your_name="mozhiyan"   
echo $your_name
echo ${your_name}
```

`变量名外面的花括号是可选的,加不加都行,加花括号是为了帮助解释器识别变量的边界`

`推荐给所有变量加上花括号,这是个好的编程习惯。`
已定义过的变量可以重新定义:

```
aaa="my aaa"
echo ${myUrl}
aaa="your bbb"
echo ${myUrl}
```
#### 只读变量
使用`readonly`命令可以将变量定义为只读变量,只读变量的值不能被改变。
#### 删除变量
使用`unset`命令可以删除变量。

### shell变量
shell变量即特殊变量
#### 特殊变量列表
![](/shell-2.png)


运行实例:

```
#!/bin/bash
echo "File Name: $0"
echo "First Parameter : $1"
echo "First Parameter : $2"
echo "Quoted Values: $@"
echo "Quoted Values: $*"
echo "Total Number of Parameters : $#"
```

![](/shell-3.png)




#### $*和$@的区别

```
$* 和 $@ 都表示传递给函数或脚本的所有参数,不被双引号(" ")包含时,都以"$1" "$2" … "$n" 的形式输出所有参数。
但是当它们被双引号(" ")包含时,"$*" 会将所有的参数作为一个整体,以"$1 $2 … $n"的形式输出所有参数；"$@" 会将各个参数分开,以"$1" "$2" … "$n" 的形式输出所有参数。
```

### 环境变量

bash shell用一个称作`环境变量`的特性来存储有关shell会话和工作环境的信息。
它允许我们在内存中储存数据,以便运行在shell上的程序和脚本访问。
在bash shell中,环境变量分为两类:
- 全局变量
- 局部变量

#### 全局环境变量

`全局变量不仅对shell会话可见,对所有shell创建的子进程也可见；
局部变量仅对他们创建的shell可见。`

查看全局变量用`printenv`命令:

![](/shell-4.png)

如只要显示单个环境变量的值,可用`echo`命令,当引用环境变量时,必须在环境变量的名称前放置一个`$`符号

![](/shell-5.png)

#### 局部环境变量
即上文的局部变量!!!
查看局部变量的列表有些复杂,Linux并没有一个命令只显示局部变量。`set`命令会显示为某个特定进程设置的所有环境变量,包括全局变量。

![](/shell-6.png)

除去全局变量剩下的就是局部变量了 ＝ ＝

#### 设置全局变量
创建全局变量的方法是先创建一个局部变量,然后再将它通过`export`命令导入到全局变量中。

![](/shell-7.png)

#### 设置PATH环境变量
PATH环境变量是Linux系统上造成最多问题的变量。它定义了命令行输入命令的搜索路径,如果找不到命令,它会产生一个错误。

![](/shell-8.png)


PATH中的目录之间使用 `:` 分隔,如果想要添加新的目录到现有的PATH环境变量,无需重头定义,只需要像这样即可:

```
$ PATH=$PATH:/home/user/test
```

也可以这样添加

```
$ PATH=$PATH:.
```

这个单点符代表将当前目录添加到PATH变量中去。






## Shell替换

### 特殊字符替换
如果表达式中包含特殊字符,Shell 将会进行替换。例如,在双引号中使用变量就是一种替换,转义字符也是一种替换。


```
#!/bin/bash

a=10
echo -e "Value of a is $a \n"
#这里 -e 表示对转义字符进行替换。如果不使用 -e 选项,将会原样输出
echo "Value of a is $a \n"
```

![](/shell-9.png)




### 命令替换
命令替换是指Shell可以先执行命令,将输出结果暂时保存,在适当的地方输出。
命令替换的语法:

```
`command`
```

```
#!/bin/bash
DATE=`date`
echo "Date is $DATE"
```

输出

![](/shell-10.png)

### 变量替换
变量替换可以根据变量的状态(是否为空、是否定义等)来改变它的值

![](/shell-11.png)


```
#!/bin/bash

echo ${var:-"Variable is not set"}
echo "1 - Value of var is ${var}"

echo ${var:="Variable is not set"}
echo "2 - Value of var is ${var}"

unset var
echo ${var:+"This is default value"}
echo "3 - Value of var is $var"

var="Prefix"
echo ${var:+"This is default value"}
echo "4 - Value of var is $var"

unset var
echo ${var:?"Print this message"}
echo "5 - Value of var is ${var}"
```

![](/shell-12.png)