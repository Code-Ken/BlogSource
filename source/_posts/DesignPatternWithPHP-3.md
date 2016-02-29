title: PHP设计模式——抽象工厂
date: 2016-02-29 17:56:12
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式,抽象工厂介绍</center>

<!--more-->

## 抽象工厂


提供创建`一系列`相关或相互依赖对象的接口，而无需指定他们具体的类

抽象工厂模式：
多个抽象产品类，每个抽象产品类可以派生出多个具体产品类。 
一个抽象工厂类，可以派生出多个具体工厂类。 
每个具体工厂类可以创建多个具体产品类的实例。 
            
## 与工厂方法区别

工厂方法模式只有一个抽象产品类，而抽象工厂模式有多个。
工厂方法模式的具体工厂类只能创建一个具体产品类的实例，而抽象工厂模式可以创建多个。


## 优点
易于交换产品系列

## 缺点
新增功能时要增加的类比较多，至少要增加三个类

## PHP代码

```
<?php

namespace AbstractFactory;
//=====共同接口=====

interface db
{
    function conn();

}

//新增user接口
interface user
{
    function insertUser();
}

interface Factory
{
    function createDB();

    //新增createUser函数
    function createUser();
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
        echo "connect sqlite!";
    }
}

//新增user**类
class userMysql implements user
{
    function insertUser()
    {
        echo "insert mysql user!";
    }
}

class userSqlite implements user
{
    function insertUser()
    {
        echo "insert sqlite user!";
    }
}


class mysqlFactory implements Factory
{
    function createDB()
    {
        return new dbmysql();
    }

    function createUser()
    {
        return new userMysql();
    }
}

class sqliteFactory implements Factory
{
    function createDB()
    {
        return new dbsqlite();
    }

    function createUser()
    {
        return new userSqlite();
    }
}

//=====客户器端=====

$fact = new mysqlFactory();

$db = $fact->createDB();
$db->conn();

$user = $fact->createUser();
$user->insertUser();




```
