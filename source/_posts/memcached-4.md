title: memcached第五章——分布式算法
date: 2016-03-04 09:26:53
categories: memcached
tags: [memcached,PHP]
---

<center>memcached两种分布式算法介绍</center>

<!--more-->

## 取模算法

就是“根据服务器台数的余数进行分散”。求得键的整 数哈希值,再除以服务器台数,根据其余数来选择服务器。 

余数计算的方法简单,数据的分散性也相当优秀,但也有其缺点。
那就是当添加或移除服务器时, 缓存重组的代价相当巨大。添加服务器后,余数就会产生巨变,这样就无法获取与保存时相同的服务器,从而影响缓存的命中率。

## 一致性哈希算法

### Consistent Hashing 的简单说明
​
Consistent Hashing 如下所示:首先求出 memcached 服务器(节点)的哈希值,并将其配置到 0~232的圆(continuum)上。然后用同样的方法求出存储数据的键的哈希值,并映射到圆上。然后从数 据映射到的位置开始顺时针查找,将数据保存到找到的第一个服务器上。如果超过 232 仍然找不到服务器,就会保存到第一台 memcached 服务器上。 


![](1.png)

从上图的状态中添加一台 memcached 服务器。余数分布式算法由于保存键的服务器会发生巨大变化而影响缓存的命中率,但 Consistent Hashing 中,只有在 continuum 上增加服务器的地点逆时针方向 的第一台服务器上的键会受到影响。 

![](2.png)


因此,Consistent Hashing 最大限度地抑制了键的重新分布。而且,有的 Consistent Hashing 的实现方 法还采用了虚拟节点的思想。使用一般的 hash 函数的话,服务器的映射地点的分布非常不均匀。 因此,使用虚拟节点的思想,为每个物理节点(服务器)在 continuum 上分配 100~200 个点。这样就能抑制分布不均匀,最大限度地减小服务器增减时的缓存重新分布。 


## 两种算法命中率问题
当memcahed 结点由 N->N-1 时;
取模算法的命中率降为1/(N-1);
一致性哈希的命中率降为了1-(1/(N-1));
可见一致性哈希算法的优势十分明显。

![](3.png)

<br/>

![](4.png)




## PHP扩展中应用

在PHP客户端，memcached默认的算法是取模算法，需要在客户端切换成一致性哈希算法

```
$mem = new memcached();
//却换成一致性哈希算法
$mem->setOption($mem::OPT_DISTRIBUTION,$mem::DISTRIBUTION_CONSISTENT);
//开启或关闭兼容的libketama类行为。当开启此选项后，元素key的hash算法将会被设置为md5并且分布算法将会 采用带有权重的一致性hash分布。这一点非常有用因为其他基于libketama的客户端（比如python，urby）在同样 的服务端配置下可以透明的访问key。
$mem->setOption($mem::OPT_LIBKETAMA_COMPATIBLE,TRUE);
```