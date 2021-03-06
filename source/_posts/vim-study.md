title: Vim操作指南
date: 2016-01-28 00:26:32
categories: Linux
tags: [Linux,Vim]
---

<center>本文介绍vim的常用操作</center>

<!--more-->


## 为什么要学习vim

因为linux下别无选择。。。。

## vim模式

vim共分为三种模式:

- 一般模式
>用户打开文档直接进入的就是一般模式,可以用方向键移动光标查看能容,也可以进行复制、粘贴、删除操作

- 编辑模式
>按下i键进入编辑模式,编辑文件内容,按esc键退出编辑模式

- 命令行模式
>进行存储、离开、搜索、替换、显示行号等一些动作

![vim模式](http://images.cnitblog.com/blog/352838/201311/05111232-20205b3d047249fd86c324cbf5354036.png)

## 一般模式

### 移动光标的方法

|操作|解释|
|:----:|----|
|h(←)|左移一个字符|
|j(↓)|下移一个字符|
|k(↑)|上移一个字符|
|l(→)|右移一个字符|
|[Ctrl]+[f]|屏幕"向下"移动一页|
|[Ctrl]+[b]|屏幕"向上"移动一页|
|+|光标移动到非空格符的下一列|
|-|光标移动到非空格符的上一列|
|n+[space]|n表示"数字"按下数字后再按空格键,光标会向右移动这一行的n个字符</br>例如20+[space]|
|0|移动到这一行的最前面字符处|
|$|移动到这一行的最后面字符处|
|H|光标移动到这个屏幕的最上方那一行的第一个字符|
|M|光标移动到这个屏幕的中央那一行的第一个字符|
|L|光标移动到这个屏幕的最下方那一行的第一个字符|
|G|移动到这个档案的最后一行|
|nG|n为数字。移动到这个档案的第n行。例如 20G|
|gg|移动到这个档案的第一行|
|n+[Enter]|n为数字。光标向下移动n行|

### 搜索与取代


|操作|解释|
|:----:|----|
|/word|向光标之下寻找一个名称为"word"的字符串|
|?word|向光标之上寻找一个字符串名称为 word 的字符串|
|n|这个n是英文按键,表示向下继续搜索word|
|N|这个N是英文按键。与n刚好相反,表示向上搜索word|
|:n1,n2s/word1/word2/g|n1与n2 为数字。</br>在第n1与n2行之间寻找word1这个字符串,并将该字符串取代为word2</br> 在100到200行之间搜寻vbird并取代为VBIRD则：</br>[:100,200s/vbird/VBIRD/g]|
|[:1,$s/word1/word2/gc]|从第一行到最后一行寻找word1字符串,并将该字符串取代为word2</br>且在取代前显示提示字符给用户确认是否需要取代|

### 复制删除和粘贴

|操作|解释|
|:----:|----|
|x, X|x为向后删除一个字符,X为向前删除一个字符|
|nx|n为数字,连续向后删除 n 个字符。连续删除 10 个字符,"10x"|
|dd|删除游标所在的那一整行|
|ndd|n为数字。删除光标所在的向下n列,删除20列,"20dd"|
|d1G|删除光标所在到第一行的所有数据|
|dG|删除光标所在到最后一行的所有数据|
|d$|删除游标所在处,到该行的最后一个字符|
|d0|删除游标所在处,到该行的最前面一个字符|
|yy|复制游标所在的那一行|
|nyy|n为数字。复制光标所在的向下n列,"20yy",复制20列|
|y1G|复制游标所在列到第一列的所有数据|
|yG|复制游标所在列到最后一列的所有数据|
|y0|复制光标所在的那个字符到该行行首的所有数据|
|p, P|粘贴命令,p为粘贴在光标下一行,P为粘贴在上一行|
|u|恢复前一个动作|
|.|重复前一个动作|
|y+¥|复制光标所在的那个字符到该行行尾的所有数据|


### 区块选择

光标到想选择的地方按下ctrl+c,出现Visual Block后进行选择

|操作|解释|
|:----:|----|
|v|字符选择,会将光标经过的地方反白选择|
|V|行选择,会将光标经过的行反白选择|
|y|将反白的地方复制起来|
|d|将反白的地方删除掉|



## 插入模式
 按i a o r进入插入模式,没啥说的

## 命令行模式

|操作|解释|
|:----:|----|
|:w|将编辑的数据写入硬盘档案中|
|:w!|若文件属性为"只读"时,强制写入该档案|
|:q|离开|
|:q!|强制离开不储存档案|
|:wq|储存后离开|
|ZZ|若档案没有更动,则不储存离开,若档案已经被更动过,则储存后离开|
|:w [filename]|编辑文件另存为filename|
|:r [filename]|将filename内容插入到当前文件|
|:! command|暂时离开 vi 到指令列模式下执行 command 的显示结果</br>例如":! ls "即可在vim查看文件列表|
|:set nu|显示行号,设定之后,会在每一行的前缀显示该行的行号|
|:set nonu|与set nu相反,为取消行号|



## 多窗口功能

|操作|解释|
|:----:|----|
|:sp [filename]|开启一个新窗口,如果有加 filename, 表示在新窗口开启一个新档案</br>否则表示两个窗口为同一个档案内容(同步显示)|
|[ctrl]+w|切换窗口|

![](http://vbird.dic.ksu.edu.tw/linux_basic/0310vi_files/vim-window-01.jpg)


## 环境参数设置

|操作|解释|
|:----:|----|
|:set nu|设定行号|
|:set nonu|取消行号|
|:set autoindent|自动缩进|
|:set noautoindent|不自动缩进|
|:set ruler|显示右下角状态栏说明｜
|:set showmode|是否要显示 --INSERT-- 之类的字眼在左下角的状态栏|
|:set all|显示目前所有的环境参数设定值|
|:set|显示与系统默认值不同的设定参数|
|:syntax on|开启语法高亮|
|:syntax off|关闭语法高亮|
|:set bg=dark|加深字体颜色|
|:set bg=light|默认字体颜色|











## 参考文档

1.[鸟哥的Linux私房菜](http://vbird.dic.ksu.edu.tw/linux_basic/0310vi.php#vi)
2.《Linux命令行与Shell脚本编程大全》第2版