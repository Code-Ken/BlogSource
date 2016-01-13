title: 【数据结构与算法】第0章一一绪论
date: 2016-01-08 14:15:54
categories: 数据结构与算法
tags: [PHP,JavaScript,数据结构,算法]
---

<center>数据结构与算法 第0章 绪论</center>
<!--more-->
## 数据结构绪论
### 数据结构是什么？
数据结构是一门研究非数值计算的程序设计问题中的操作对象,以及它们之间的关系和操作等相关问题的学科
>程序设计 ＝ 数据结构 ＋ 算法

### 基本概念和术语

- 数据：
描述客观事物的符号,是计算机中可以操作的对象,是能被计算机识别,并输入给计算机处理的符号
- 数据元素
是组成数据的、有一定意义的基本单位,在计算机中通常作为整体处理。也被成为记录。
- 数据项
数据项是数据不可分割的最小单位,一个数据元素可以由多个数据项组成。
- 数据对象
是性质相同的数据元素的集合,是数据的子集。
- 数据结构
是相互之间存在一种或多种特定关系的数据元素的集合

### 逻辑结构与物理结构
我们把数据结构分为逻辑结构与物理结构。
#### 逻辑结构
是指数据对象中数据元素之间的相互关系,是针对具体问题的,为了解决某个问题,在对问题理解的基础上,选择一个合适的数据结构来表示数据元素之间的`逻辑`关系
主要分为四种：
1.集合结构
集合结构中的数据元素除了同属于一个集合外,它们之间没有其他关系

![](/sa0-1.png)

2.线性结构
线性结构中的数据元素之间是一对一的关系

![](/sa0-2.png)

3.树形结构
树形结构中的元素之间是一对多的层次关系

![](/sa0-3.png)

4.图形结构
图形结构的数据元素之间是多对多的关系

![](/sa0-4.png)

#### 物理结构
也称做存储结构,是指数据的逻辑结构在计算机中的存储形式,就是怎么存在计算机存储器(内存、硬盘、光盘等)中
逻辑结构是面向问题的,而物理结构是面向计算机的,其基本的目标就是将数据及其逻辑关系存到计算机的内存中去
物理结构分为两种:
1.顺序储存结构
是把数据存放在地址连续的存储单元里,其数据之间的逻辑关系和物理关系是一致的

![](/sa0-5.png)

2.链式存储结构
是把数据存放在任意的存储单元里,这组储存单元可以是连续的,也可以是不连续的
它不能反映其逻辑关系,因此需要用一个指针存放数据元素的地址,这样通过地址就可以找到相关数据元素的位置了

![](/sa0-6.png)


### 抽象数据类型
#### 数据类型
是指一组性质相同的值的集合及定义在此集合上的一些操作的总称。
数据类型可以分为两类：
1.原子类型：是不可再分的基本类型,包括整型、实型、字符型等。
2.结构类型：由若干个类型组合而成,是可以再分解的。例如,整型数组是由若干个整型数据组成的。

#### 抽象数据类型
抽象的意思是抛开问题看本质～
抽象数据类型(Abstract Date Type,`ADT`)是指一个数学模型及定义在该模型上的一组操作。
抽象数据模型的标准格式：

```
ADT 抽象数据类型名
Date 
    数据元素之间的逻辑关系定义
Operation
    操作1
        初始条件
        操作结果描述
    操作2
        ......
    操作n
        ......
endADT
```

## 算法绪论
### 什么是算法？
>算法是解决特定问题求解步骤的描述,在计算机中表现为指令的有限序列,并且每条指令表示一个或者多个操作

### 算法的特性
1.输入输出
算法具有零个或多个输入,至少一个输出的
2.有穷性
算法在执行有限的步骤后,自动结束而不会出现无限的循环,并且每个步骤都在可以接受的时间内完成
3.确定性
不会出现二义性
4.可行性

### 算法设计的要求
1.正确性
2.可读性
3.健壮性
当输入不合法时,算法也能做出相关处理,而不是产生异常或莫名奇妙的结果
4.时间效率高储存量低

### 算法时间复杂度
#### 定义
>在进行算法分析时,语句总的执行次数T(n)是关于问题规模n的函数,进而分析T(n)随n的变化情况确定T(n)数量级。算法的时间复杂度,记作：T(n)＝ O(f(n))。   

这样用大写O()来体现算法时间复杂度的记法,我们称之为大O记法。
一般情况下,随着n的增大,T(n)增长最缓的算法我们称为最优算法。
#### 推导大O阶方法
>推导大O阶
1.用常数1取代运行时间中所有的加法常数。
2.在修改后的运行次数函数中，指保留最高阶项。
3.如果最高阶项存在且不是1，则去除与这个项目相乘的常数
得到的结构就是大O阶

#### 常见的时间复杂度


|阶|非正式术语 | 
|:------------:|:-----------|
|O(1)|常数阶|
|O(*n*)|线性阶|
|O($n^2$)|常数阶|
|O(log*n*)|对数阶|
|O(*n*log*n*|*n*log*n*阶|
|O($n^3$)|立方阶|
|O($2^n$)|指数阶|

以上表格耗费时间从大到小排列

![](/sa0-7.png)







































