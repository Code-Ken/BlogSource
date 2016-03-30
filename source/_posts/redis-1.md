title: Redis介绍
date: 2016-02-01 14:24:28
categories: Redis
tags: [Redis]
---

<center>本文主要介绍redis的安装、简介与数据类型</center>
<!--more-->

## 介绍

### 简介

Redis 是一款开源的，基于 BSD 许可的，高级键值 (key-value) 缓存 (cache) 和存储 (store) 系统。由于 Redis 的键包括 string，hash，list，set，sorted set，bitmap 和 hyperloglog，所以常常被称为数据结构服务器。

我们可以在这些类型上面运行原子操作，例如：

- 追加字符串
- 增加哈希中的值
- 加入一个元素到列表
- 计算集合的交集、并集和差集
- 从有序集合中获取最高排名的元素。

### redis与memcached的区别

redis和memcached相比,的独特之处:
1.redis可以用来做存储(storge), 而memccached是用来做缓存(cache)
  这个特点主要因为其有”持久化”的功能.
2.存储的数据有”结构”,对于memcached来说,存储的数据,只有1种类型--”字符串”,
  而redis则可以存储字符串,链表,哈希结构,集合,有序集合.


## redis安装
### Mac下的安装

直接homebrew安装～

```
brew install redis
```

没啥说的

### Ubuntu下安装

```
$sudo apt-get update
$sudo apt-get install redis-server
```

### 安装完后得到的命令

|命令|解释|
|----|----|
|redis-server|服务器端|
|redis-cli|客户端|
|redis-benchmark|性能测试工具|
|redis-check-aof|检查aof日志的工具|
|redis-check-dump|检查rbd日志的工具|

### 启动与连接

必须先启动redis服务器才能连接redis客户端.....
 
```
$ redis-server 启动服务器
$ redis-cli        连接服务器
```

## 数据类型

### 字符串(String)

字符串是 Redis 最基本的数据类型

>注：字符串值可以存储最大512M的长度。

### 列表(Lists)

Redis 列表仅仅是按照插入顺序排序的字符串列表。可以添加一个元素到 Redis 列表的头部 (左边) 或者尾部 (右边)。

### 集合(Sets)

Redis 集合是没有顺序的字符串集合 (collection)。可以在 O(1) 的时间复杂度添加、删除和测试元素存在与否 (不管集合中有多少元素都是常量时间)。

加入一个元素到集合中并不需要检查元素是否已存在。
Redis支持很多服务器端的命令,可以在很短的时间内和已经存在的集合一起计算并集,交集和差集。

### 哈希／散列(Hashes)

Redis 哈希是字符串字段 (field) 与字符串值之间的映射，所以是表示对象的理想数据类型。
拥有少量字段 (少量指的是大约 100) 的哈希会以占用很少存储空间的方式存储，所以你可以在一个很小的Redis实例里存储数百万的对象。

### 有序集合(Sorted sets)

Redis 有序集合和 Redis 集合类似，是非重复字符串集合 (collection)。不同的是，每一个有序集合的成员都有一个关联的分数 (score)，用于按照分数高低排序。尽管成员是唯一的，但是分数是可以重复的。








