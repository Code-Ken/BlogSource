title: PHP大小写问题
date: 2016-01-10 21:20:02
categories: PHP
tags: [PHP,大小写]
---

<center>本文主要总结了PHP各种情况大小写是否敏感的问题</center>
<!--more-->
- 变量名区分大小写
- 常量名默认区分大小写
- 函数名、方法名、类名不区分大小写
下面的引用要格外的注意！！！parent,self,static
>"parent" and "self", "static" LSB tokens behave inconsistent when it comes to case-sensitivity. Class names in PHP are case-insensitive, but these three keywords aren't always, as the parser sees them as raw T_STRINGs.
- 魔术变量不区分大小写,但默认大写
- null、true、false不区分大小写 