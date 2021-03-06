title: 数据结构第0章--数据结构简介
date: 2017-11-16 19:26:53
categories: 数据结构
tags: [数据结构]
---

# 为什么学习数据结构
数据结构是计算机专业的基础课程之一，所有计算机系统和软件都要用到各种类型的数据结构。
学习数据结构，并不仅仅是学习其中现成的那些队列,堆栈,二叉树,图等经典结构,也不仅仅是学习其中的那些快速排序、冒泡排序等算法。
更重要的是学习到一种思想:`如何把现实问题转化为计算机语言的表示`
<!--more-->
计算机是一种很笨的机器，起语言本质是二进制的，及时是C,Java等高级语言也只不过是包装而已，而我们用的自然语言是典型的模糊，不精确的
我们要做的工作就是怎样把自然语言描述的问题转换成计算机语言表示。
到底该怎么转化,《数据结构》已经给出了指引:
> 设计出数据结构,在施加以算法就行
程序设计 = 数据结构  + 算法

这是一种非常重要的逻辑能力锻炼,也是程序员入门的条件
# 数据结构分类
数据结构分为两大类：逻辑结构与物理结构
## 逻辑结构
是指数据对象中数据元素之间的相互关系,是针对具体问题的,为了解决某个问题,在对问题理解的基础上,选择一个合适的数据结构来表示数据元素之间的`逻辑`关系
主要分为四种：
- 集合结构:集合结构中的数据元素除了同属于一个集合外,它们之间没有其他关系
- 线性结构:线性结构中的数据元素之间是一对一的关系
- 树形结构:树形结构中的元素之间是一对多的层次关系
- 图形结构:图形结构的数据元素之间是多对多的关系
![](jiegou.png)

## 物理结构
也称做存储结构,是指数据的逻辑结构在计算机中的存储形式,就是怎么存在计算机存储器(内存、硬盘、光盘等)中 
逻辑结构是面向问题的,而物理结构是面向计算机的,其基本的目标就是将数据及其逻辑关系存到计算机的内存中去 
物理结构分为两种: 
### 顺序储存结构 
是把数据存放在地址连续的存储单元里,其数据之间的逻辑关系和物理关系是一致的 
![](xianxing.png)
### 链式存储结构 
是把数据存放在任意的存储单元里,这组储存单元可以是连续的,也可以是不连续的
它不能反映其逻辑关系,因此需要用一个指针存放数据元素的地址,这样通过地址就可以找到相关数据元素的位置了
![](lianjia.png)

# 抽象数据类型ADT

## 定义
>是指一个数学模型及定义在该模型上的一组操作。抽象数据类型的定义仅取决于它的一组逻辑特性，而与其在计算机内部如何表示和实现无关。


## 意义
根据抽象数据类型的定义，它还包括定义在该模型上的一组操作。就像“超级玛丽”这个经典的任天堂游戏，里面的游戏主角是马里奥（Mario)。我们给他定义了几种基本操作，走（前进、后退、上、下)、跳、打子弹等。

一个抽象数据类型定义了：一个数据对象、数据对象中各数据元素之间的关系及对数据元素的操作。至于，一个抽象数据类型到底需要哪些操作，这就只能由设计者根据实际需要来定。像马里奥， 可能开始只有两种操作，走和跳，后来发现应该要增加一种打子弹的操作，再后来发现有些玩家希望它可以走得快一点，就有了按住打子弹键后前进就会“跑”的操作。这都是根据实际情况来设计的

`抽象的意思是抛开问题看本质～`
##  标准格式
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

















