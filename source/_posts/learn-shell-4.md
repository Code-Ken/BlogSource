title: Shell入门学习(四)
date: 2016-01-14 13:58:31
categories: Linux
tags: [Linux,Shell]
---

<center>Shell的学习手册(四)</center>
<!--more-->

# Shell语法



## Shell If语句
if 语句通过关系运算符判断表达式的真假来决定执行哪个分支。
Shell 有三种if ... else 语句:

- if ... fi 语句；
- if ... else ... fi 语句；
- if ... elif ... else ... fi 语句。

实例:

```
#!/bin/sh
a=10
b=20

if [ $a == $b ]
then
   echo "a is equal to b"
fi


a=10
b=20

if [ $a == $b ]
then
   echo "a is equal to b"
else
   echo "a is not equal to b"
fi



a=10
b=20

if [ $a == $b ]
then
   echo "a is equal to b"
elif [ $a -gt $b ]
then
   echo "a is greater than b"
elif [ $a -lt $b ]
then
   echo "a is less than b"
else
   echo "None of the condition met"
fi
```

## Shell Case语句

语法
```
case 值 in
模式1)
    command1
    command2
    command3
    ;;
模式2)
    command1
    command2
    command3
    ;;
*)
    command1
    command2
    command3
    ;;
esac
```
case工作方式如上所示。取值后面必须为关键字 in,每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后,其间所有命令开始执行直至 ;;。;; 与其他语言中的 break 类似,意思是跳到整个 case 语句的最后。

实例:
```
echo 'Input a number between 1 to 4'
echo 'Your number is:\c'
read aNum
case $aNum in
    1)  echo 'You select 1'
    ;;
    2)  echo 'You select 2'
    ;;
    3)  echo 'You select 3'
    ;;
    4)  echo 'You select 4'
    ;;
    *)  echo 'You do not select a number between 1 to 4'
    ;;
esac
```

## Shell For循环
语法:
```
for 变量 in 列表
do
    command1
    command2
    ...
    commandN
done
```
列表是一组值(数字、字符串等)组成的序列,每个值通过空格分隔。每循环一次,就将列表中的下一个值赋给变量。
in 列表是可选的,如果不用它,for 循环使用命令行的位置参数。

实例:
```
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

## Shell While循环
语法
```
while command
do
   Statement(s) to be executed if command is true
done
```
实例:
```
COUNTER=0
while [ $COUNTER -lt 5 ]
do
    COUNTER='expr $COUNTER + 1'
    echo $COUNTER
done
```

## Shell until循环
语法:
```
until command
do
   Statement(s) to be executed until command is true
done
```
实例:
```
#!/bin/bash

a=0

until [ ! $a -lt 10 ]
do
   echo $a
   a=`expr $a + 1`
done
```

## Shell 跳出循环
break 和 continue
### break
break命令允许跳出所有循环(终止执行后面的所有循环)。
在嵌套循环中,break 命令后面还可以跟一个整数,表示跳出第几层循环。例如:
```
break n
```
表示跳出第 n 层循环。
`break n 跳到指定的循环,如果n不指定 , 默认跳到最大的循环.,继续执行最大循环里面的语句；如果n大于最大循环,条件一旦成立则跳出最大循环。`
### continue
continue命令与break命令类似,只有一点差别,它不会跳出所有循环,仅仅跳出当前循环。




## Shell函数
## 函数格式
Shell 函数的定义格式如下:

```
function_name () {
    list of commands
    [ return value ]
}
```
如果你愿意,也可以在函数名前加上关键字 function:
```
function function_name () {
    list of commands
    [ return value ]
}
```

`函数返回值,可以显式增加return语句；如果不加,会将最后一条命令运行结果作为返回值。`

>Shell 函数返回值只能是整数,一般用来表示函数执行成功与否,0表示成功,其他值表示失败。如果 return 其他数据,比如一个字符串,往往会得到错误提示:“numeric argument required”。
如果一定要让函数返回字符串,那么可以先定义一个变量,用来接收函数的计算结果,脚本在需要的时候访问这个变量来获得函数返回值。

调用函数只需要给出函数名,不需要加括号。

## 参数调用
在Shell中,调用函数时可以向其传递参数。在函数体内部,通过 $n 的形式来获取参数的值,例如,$1表示第一个参数,$2表示第二个参数...
注意,$10 不能获取第十个参数,获取第十个参数需要${10}。当n>=10时,需要使用${n}来获取参数。

实例

```
#!/bin/bash
funWithParam(){
    echo "The value of the first parameter is $1 !"
    echo "The value of the second parameter is $2 !"
    echo "The value of the tenth parameter is $10 !"
    echo "The value of the tenth parameter is ${10} !"
    echo "The value of the eleventh parameter is ${11} !"
    echo "The amount of the parameters is $# !"  # 参数个数
    echo "The string of the parameters is $* !"  # 传递给函数的所有参数
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```