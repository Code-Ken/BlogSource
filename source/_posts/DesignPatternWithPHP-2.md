title: PHP设计模式——工厂方法
date: 2016-02-29 17:53:43
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式——工厂方法</center>

<!--more-->

​
## 工厂方法

工厂方法模式实现时，客户端需要决定实例化哪个工厂来实现运算类，选择判断问题还是存在，但工厂方法把简单工厂的内部逻辑判断问题移到了客户端代码来进行。我们想加功能，本来是要改工厂类的，现在只需要改客户端。

## 优点
增加一个类，只需要增加类和相对应的工厂，两个类，不需要修改工厂类。

## 缺点
增加类，会修改客户端代码，工厂方法只是把简单工厂的内部逻辑判断移到了客户端进行。

## PHP代码

```
<?php

//=====共同接口=====

interface db
{
    function conn();
}

interface Factory
{
    function createDB();
}

//=====服务器端=====

class dbmysql implements db
{
    function conn()
    {
        echo "connect mysql!";
    }
}

class dbsqlite implements db
{
    function conn()
    {
        echo "connect mysql!";
    }
}


class mysqlFactory implements Factory
{
    function createDB()
    {
        return new dbmysql();
    }
}




class sqliteFactory implements Factory
{
    function createDB()
    {
        return new dbsqlite();
    }
}


//=====服务器端新增oracle类=====

class dboracle implements db{
    function conn()
    {
        echo "connect oracle";
    }
}

class oracleFactory implements Factory
{
    function createDB()
    {
        return new dboracle();
    }
}



//=====客户器端=====

$fact = new mysqlFactory();
$db = $fact->createDB();
$db->conn();

$fact1 = new oracleFactory();
$db1 = $fact1->createDB();
$db->conn();
```
