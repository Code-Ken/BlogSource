title: Shell入门学习(三)
date: 2016-01-14 13:53:31
categories: Linux
tags: [Linux,Shell]
---

<center>Shell的学习手册(三)</center>
<!--more-->



# Shell语法


## Shell运算符
### 算术运算符

`原生bash不支持简单的数学运算,但是可以通过其他命令来实现,expr 是一款表达式计算工具,使用它能完成表达式的求值操作。`

两点注意:
- 表达式和运算符之间要有空格,例如 2+2 是不对的,必须写成 2 + 2,这与我们熟悉的大多数编程语言不一样。
- 完整的表达式要被 `` 包含,注意这个字符不是常用的单引号,在 Esc 键下边

![](/shell-13.png)

注意:条件表达式要放在方括号之间,并且要有空格!!!
例如 [$a==$b] 是错误的,必须写成 [ $a == $b ]。

### 关系运算符
关系运算符只支持数字,不支持字符串,除非字符串的值是数字。

![](/shell-14.png)

### 布尔运算符

![](/shell-15.png)

### 字符串运算符

![](/shell-16.png)


### 文件运算符

![](/shell-17.png)

## Shell注释
以#开头的就是注释

## Shell字符串
### 单引号双引号的区别
单引号字符串的限制:
- 单引号里的任何字符都会原样输出,单引号字符串中的变量是无效的；
- 单引号字串中不能出现单引号(对单引号使用转义符后也不行)。

双引号的优点:
- 双引号里可以有变量
- 双引号里可以出现转义字符

所以一般都用双引号！！！

### 字符串拼接

```
your_name="qinjx"
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"

echo $greeting $greeting_1

//hello, qinjx ! hello, qinjx !
```

### 字符串长度获取

```
string="abcd"
echo $｛#string｝
```




### 提取子字符串

```
string="alibaba is a great company"
echo ${string:1:4} #输出liba
```



## Shell数组
### 数组定义的三种方式

```
＃1
array_name=(value0 value1 value2 value3)

＃2
array_name=(
    value0
    value1
    value2
    value3
)

＃3
array_name[0]=value0
array_name[1]=value1
array_name[2]=value2
```

### 读取数组

```
${array_name[index]} #读取数组值的一般方式 eg:${NAME[0]}

${array_name[*]}  #使用@ 或 * 可以获取数组中的所有元素
${array_name[@]}
```

### 获取数组长度

```
#取得数组元素的个数
length=${＃array_name[@]}
#或者
length=${＃array_name[*]}
#取得数组单个元素的长度
lengthn=${＃array_name[n]}
```




## Shell echo命令

```
#显示转移字符
echo "\"It is a test\""
#"It is a test"

#将结果重定向至文件
echo "It is a test" > myfile

#显示命令执行结果
echo `date`
```

## Shell Printf命令
printf 命令用于格式化输出, 是echo命令的增强版。它是C语言printf()库函数的一个有限的变形,并且在语法上有些不同。

语法:

```
printf  format-string  [arguments...]
```

实例:

```
#format-string为双引号
$ printf "%d %s\n" 1 "abc"
1 abc
#单引号与双引号效果一样 
$ printf '%d %s\n' 1 "abc" 
1 abc
#没有引号也可以输出
$ printf %s abcdef
abcdef
#格式只指定了一个参数,但多出的参数仍然会按照该格式输出,format-string 被重用
$ printf %s abc def
abcdef
$ printf "%s\n" abc def
abc
def
$ printf "%s %s %s\n" a b c d e f g h i j
a b c
d e f
g h i
j
#如果没有 arguments,那么 %s 用NULL代替,%d 用 0 代替
$ printf "%s and %d \n" 
and 0
#如果以 %d 的格式来显示字符串,那么会有警告,提示无效的数字,此时默认置为 0
$ printf "The first program always prints'%s,%d\n'" Hello Shell
-bash: printf: Shell: invalid number
The first program always prints 'Hello,0'

```

