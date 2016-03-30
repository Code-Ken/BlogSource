title: memcached第一章——基础
date: 2016-03-03 17:14:43
categories: memcached
tags: [memcached,PHP]
---


<center>对memcached进行简单介绍并进行安装</center>

<!--more-->

​


## memcached是什么

Today 许多 Web 应用都将数据保存到 RDBMS 中,应用服务器从中读取数据并在浏览器中显示。但随着数据量的增大、访问的集中,就会出现RDBMS的负担加重、数据库响应恶化、网站显示延迟等重大影响。这时就该 memcached 大显身手了。

memcached 是高性能的分布式内存缓存服务器。一般的使用目的是

>通过缓存数据库查询结果,减少数据库访问次数,以提高动态 Web应用的速度、提高可扩展性。 


![](1.png)

## memcached的特征

memcached 作为高速运行的分布式缓存服务器,具有以下的特点。

- 协议简单
memcached 的服务器客户端通信并不使用复杂的 XML 等格式,而使用简单的基于文本行的协议。 因此,通过 telnet 也能在 memcached 上保存数据、取得数据 
- 基于libevent的事件处理
- 内置内存存储方式
为了提高性能,memcached 中保存的数据都存储在memcached内置的内存存储空间中。由于数据仅 存在于内存中,因此重启 memcached、重启操作系统会导致全部数据消失。另外,内容容量达到指 定值之后,就基于LRU(Least Recently Used)算法自动删除不使用的缓存。memcached 本身是为缓存 而设计的服务器,因此并没有过多考虑数据的永久性问题。
- memcached不互相通信的分布式 
memcached 尽管是“分布式”缓存服务器,但`服务器端并没有分布式功能`。各个memcached不会互相通信以共享信息。那么,怎样进行分布式呢?这完全取决于客户端的实现。


## memcache的安装与php扩展的安装

只介绍Mac环境下的安装，通过homebrew安装。

```
//安装memcached
brew install memcached

//安装php扩展

brew install php55-memcached
```

memcached安装好后启动memcached：

```
memcached -p 11211 -m 128 -u root -vvv
```

使用 memcached -h 查看
```
localhost:~ Ken$ memcached -h
//列出一些比较常用的
//memcached的版本号
memcached 1.4.24

//memcached的TCP端口号，默认是112111
-p <num>      TCP port number to listen on (default: 11211)
//memcached的UDP端口号
-U <num>      UDP port number to listen on (default: 11211, 0 is off)
//以守护进程方式启动
-d            run as a daemon
-u <username> assume identity of <username> (only when run as root)
//分配给用户的最大内存数，默认64MB
-m <num>      max memory to use for items in megabytes (default: 64 MB)
//最大连接数  默认1024个
-c <num>      max simultaneous connections (default: 1024)
//增长因子，默认1.25
-f <factor>   chunk size growth factor (default: 1.25)
//输出错误信息
-v            verbose (print errors/warnings while in event loop)
//输出所有信息
-vv           very verbose (also print client commands/reponses)
```

## memcached的连接

memcached的客户端连接使用telnet即可

```
//telnet hostname port
localhost:~ Ken$ telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
```

PHP端的连接

```
<?php

$mem = new Memcached();

$mem->addServer("127.0.0.1",11211);

$mem->set("name","ken");

echo $mem->get("name");
```



