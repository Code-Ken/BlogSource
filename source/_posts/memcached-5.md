title: memcached第六章——经典问题
date: 2016-03-04 09:30:00
categories: memcached
tags: ［memcached,PHP］
---

<center>memcached一些经典问题的解决方案</center>

<!--more-->



## 缓存雪崩现象

缓存雪崩一般是由某个缓存节点失效,导致其他节点的缓存命中率下降, 缓存中缺失的数据 去数据库查询.短时间内,造成数据库服务器崩溃.
重启 DB,短期又被压跨,但缓存数据也多一些.
DB 反复多次启动多次,缓存重建完毕,DB 才稳定运行. 

### 解决方案

- 把缓存设置为随机 3 到 9 小时的生命周期,这样不同时失效,把工作分担到各个时间点上去. 
- 或者将失效时间设置为凌晨


## 缓存的无底洞现象 multiget-hole 

该问题由 facebook 的工作人员提出的, facebook 在 2010 年左右,memcached 节点就已经达3000 个.缓存数千G内容.
他们发现了一个问题---memcached 连接频率,效率下降了,于是加 memcached 节点,添加了后,发现因为连接频率导致的问题,仍然存在,并没有好转,称之为”无底洞现象”.
[原文](http://highscalability.com/blog/2009/10/26/facebooks-memcached-multiget-hole-more-machines- more-capacit.html)


### multiget-hole 问题分析

以用户为例: user-133-age, user-133-name,user-133-height .....N 个 key,当服务器增多,133 号用户的信息,也被散落在更多的节点,所以,同样是访问个人主页,得到相同的个人信息, 节点越多,要连接的节点也越多.对于 memcached 的连接数,并没有随着节点的增多,而降低. 于是问题出现.

### multiget-hole 解决方案:

把某一组 key,按其共同前缀,来分布:

比如 user-133-age, user-133-name,user-133-height 这 3 个 key,在用分布式算法求其节点时,应该以 ‘user-133’来计算,而不是以 user-133-age/name/height 来 计算.

这样,3 个关于个人信息的 key,都落在同 1 个节点上,访问个人主页时,只需要连接 1 个节点.

问题解决:)

## 数据永久被踢现象

网上有人反馈为"memcached 数据丢失",明明设为永久有效,却莫名其妙的丢失了.
其实,这要从 2 个方面来找原因:

即前面介绍的 惰性删除,与 LRU 最近最少使用记录删除.

分析：
1:如果 slab 里的很多 chunk,已经过期,但过期后没有被 get 过, 系统不知他们已经过期. 
2:永久数据很久没 get 了,不活跃,如果新增 item,则永久数据被踢了.
3: 当然,如果那些非永久数据被 get,也会被标识为 expire,从而不会再踢掉永久数据 

### 解决方案

永久数据和非永久数据分开放 


### 参考文献

- Mecached权威指南，燕十八著
- memcached 全面剖析,长野雅广、前坂徹著 
- PHP manual
