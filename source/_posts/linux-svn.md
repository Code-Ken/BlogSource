title: Linux常用svn指令
date: 2016-01-18 17:57:42
categories: Linux
tags: [Linux,svn]
---

<center>本文主要介绍linux os下控制行的常用svn指令</center>

<!--more-->

​
## 将文件checkout到本地目录 

```
$ svn --username=yourname co svn_path  local_path
//其中co是checkout的简写
$ svn --username=ken co https://121.222.222.21/svn/web  /home/ken/web 
```

## 更新文件


```
$ svn update //更新整体svn文件
$ svn update -r 27 demo.php //将demo.php文件更新到27版本
$ svn update ./    //跟新当前文件夹
$ svn up //简写格式
```

## 提交文件

```
$ svn commit -m "更新信息"   [更新的文件名|文件夹名]   
$ svn ci -m "修改了显示bug"   show.html    //ci是简写格式
```

## 查看日志

```
$ svn log 查看整体的日志   //建议不要这样干，日志太多的话就呵呵呵了
$ svn log --limit 10 //查看最新的10条日志
$ svn log --limt 10  --username=ken   info.php  //查看用户名是ken的对info.php操作的最新的十条日志 
```

## 查看仓库状态

```
$ svn status [PATH...]

ken@ken-K42JY:/var/www/weberp$ svn st   //st简写格式
?       .idea
M       trunk/source/Application/Common/Common/function.php
?       trunk/source/Application/Runtime/Cache/Home/9b1c26b1cee56f532f15c56b96a07c74.php
?       trunk/source/Application/Runtime/Cache/Setting
?       trunk/source/Application/Runtime/Cache/Stock
?       trunk/source/Application/Runtime/Logs/Setting
?       trunk/source/Application/Runtime/Logs/Stock
?       trunk/source/Application/Runtime/Logs/default
M       trunk/source/Application/Stock/Controller/ApiLogisticsSyncController.class.php
M       trunk/source/Application/Stock/Model/ApiLogisticsSyncModel.class.php

//'A'  新增;'D'  删除;'M'  修改;'R'  替代;'C'  冲突;
//'I'  忽略;'?'  未受控;'!'  丢失，一般是将受控文件直接删除导致
```

## 添加删除文件(夹)


```
$ svn add -m  "message" 文件名/目录名
$ svn delete/del -m "message" 文件名/目录名
```

## 比较不同

```
$ svn diff path      //将修改的文件与基础版本比较
$ svn diff test.php
$ svn diff -r m:n path    //对版本m和版本n比较差异
$ svn diff -r 200:201 test.php
$ svn di     //简写格式
```

## 查看文件详细信息

```
ken@ken-K42JY:/var/www/weberp$ svn info trunk/source/Application/Common/Common/function.php
Path: trunk/source/Application/Common/Common/function.php
Name: function.php
Working Copy Root Path: /var/www/weberp
URL: https://121.41.165.232/svn/weberp/trunk/source/Application/Common/Common/function.php
Relative URL: ^/trunk/source/Application/Common/Common/function.php
Repository Root: https://121.41.165.232/svn/weberp
Repository UUID: 40d744e1-39a9-8d4d-b592-13256b12d743
Revision: 2390
Node Kind: file
Schedule: normal
Last Changed Author: changtao
Last Changed Rev: 2387
Last Changed Date: 2015-12-13 22:43:46 +0800 (Sun, 13 Dec 2015)
Text Last Updated: 2015-12-14 10:11:26 +0800 (Mon, 14 Dec 2015)
Checksum: 158a7601292bf32085c74329a40d89f6eb03ee05
```



