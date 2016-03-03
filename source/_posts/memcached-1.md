title: memcached第二章——基本操作
date: 2016-03-03 18:04:22
categories: memcached
tags: [memcached,PHP]
---

<center>介绍memcached的具体操作指令</center>

<!--more-->



## 保存数据

语法: 

```
add key flag expire length 
set key flag expire length 

//key 给值起一个独特的名字
//flag 标志,要求为一个正整数
//expire 有效期
//length 缓存的长度(字节为单位) 
```


flag 的意义:
memcached 基本文本协议,传输的东西,理解成字符串来存储.
想:让你存一个 php 对象,和一个 php 数组,怎么办?答:序列化成字符串,往出取的时候,自然还要反序列化成 对象/数组/json 格式等等.这时候, flag 的意义就体现出来了.
比如, 1 就是字符串, 2 反转成数组 3,反序列化对象.....
expire 的意义:
设置缓存的有效期,有 3 种格式
1:设置秒数, 从设定开始数,第 n 秒后失效.
2:时间戳, 到指定的时间戳后失效.
比如在团购网站,缓存的某团到中午 12:00 失效. add key 0 1379209999 6 
3: 设为 0. 不自动失效. 

>1:编译 memcached 时,指定一个最长常量,默认是 30 天.所以,即使设为 0,30 天后也会失效.
2:可能等不到 30 天,就会被新数据挤出去.

## 替换数据

语法

```
replace key flag expire length
```


|操作名|解释|
|:----:|----|
|add|仅当存储空间中不存在键相同的数据时才保存|
|replace|仅当存储空间中存在键相同的数据时才保存|
|set|与add和replace不同,无论何时都保存|
 

## 删除数据


```
delete key [time seconds]

```


删除指定的 key.
如加可选参数time,则指删除key,并在删除key后的time秒内,不允许get,add,replace 操作此 key. 

## 获取数据

```
get key
```

返回 key 的值 

## 增减操作


```
incr/decr key num
```
 

增和减是原子操作,但未设置初始值时,不会自动赋成 0。因此,应当进行错误检查,必要时加入初始化操作 






## 统计操作


```
stats 


stat pid 2296   //进程号
stat uptime 4237 //持续运行时间
stat time 1370054990   //服务器所在主机当前系统的时间，单位是秒。
stat version 1.2.6 //版本号
stat pointer_size 32 //服务器所在主机操作系统的指针大小，一般为32或64.
stat curr_items 4 //当前存储的键个数,不包括目前已经从缓存中删除的对象
stat total_items 13 //表示从memcached服务启动到当前时间，系统存储过的所有对象的数量，包括目前已经从缓存中删除的对象。
stat bytes 236 //表示系统存储缓存对象所使用的存储空间，单位为字节。
stat curr_connections 3  //表示当前系统打开的连接数
stat total_connections 4  //表示从memcached服务启动到当前时间，系统打开过的连接的总数。stat connection_structures 4   //
stat cmd_get 20
stat cmd_set 16
stat get_hits 13 //表示获取数据成功的次数。
stat get_misses 7 //表示获取数据失败的次数。
stat evictions 0 //为了给新的数据项目释放空间，从缓存移除的缓存对象的数目。比如超过缓存大小时根据LRU算法移除的对象，以及过期的对象
stat bytes_read 764 //memcached服务器从网络读取的总的字节数。
stat bytes_written 618  //memcached服务器发送到网络的总的字节数。
stat limit_maxbytes 67108864 //memcached服务缓存允许使用的最大字节数。这里为67108864字节，也就是是64M.与我们启动memcached服务设置的大小一致。
stat threads 1
end 
```

## 清空操作

```
flush_all 
```
清空所有的存储对象 
