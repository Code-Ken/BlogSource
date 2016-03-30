title: memcached第四章——过期机制与删除机制
date: 2016-03-04 09:25:18
categories: memcached
tags: [memcached,PHP]
---

<center>memcached过期机制与删除机制的介绍</center>
  
<!--more-->

## 过期机制——惰性过期

上一章介绍过,memcached 不会释放已分配的内存。记录超时后,客户端就无法再看见该记录 (invisible,透明),其存储空间即可重复使用。

Lazy Expiration

`memcached 内部不会监视记录是否过期,而是在 get 时查看记录的时间戳,检查记录是否过期。这种技术被称为惰性过期`

因此,memcached 不会在过期监视上耗费CPU时间

## LRU删除机制

如果以 122byte 大小的chunk 举例, 122的chunk都满了,又有新的值(长度为120)要加入, 要挤掉谁?

memcached 此处用的Least Recently Used(LRU最近最少使用) 删除机制
顾名思义,这是删除“最近最少使用”的记录的机制。因此,当 memcached 的内存空间不足时(无法从 slab class 获取到新的空间 时),就从最近未被使用的记录中搜索,并将其空间分配给新的记录。从缓存的实用角度来看,该模型十分理想。 

原理: 当某个单元被请求时,维护一个计数器,通过计数器来判断最近谁最少被使用.就把谁t出. 

>注: 即使某个 key 是设置的永久有效期,也一样会被踢出来!即--永久数据被踢现象 
