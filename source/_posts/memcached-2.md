title: memcached第三章——内存存储机制
date: 2016-03-03 21:49:30
categories: memcached
tags: [memcached,PHP]
---

<center>本文主要讲解memcached的内存存储原理</center>

<!--more-->



## 内存碎片化

如果用c语言直接 malloc,free来向操作系统申请和释放内存时,在不断的申请和释放过程中,形成了一些很小的内存片断,无法再利用
`这种空闲,但无法利用内存的现象,称为内存的碎片化`

## Slab Allocator机制

为了缓解内存的碎片化，memcached用Slab Allocator机制来管理内存.

Slab Allocator 原理: 
将分配的内存分割成各种尺寸的块(chunk),并把尺寸相同的块分成组(chunk的集合),而且,slab allocator 还有重复使用已分配的内存的目的。也就是说,分配到的内存不会释放,而是重复利用。

![](1.png)

## 系统如何选择Chunk

memcached 根据收到的数据的大小, 选择最适合数据大小的chunk组
>警示:
如果有 100byte 的内容要存,但 122 大小的仓库中的 chunk 满了 并不会寻找更大的,如 144 的仓库来存储,
而是把 122 仓库的旧数据踢掉! 详见过期与删除机制 

![](2.png)

## SlabAllocator的缺点

Slab Allocator 解决了当初的内存碎片问题,但新的机制也给 memcached 带来了新的问题。
这个问题就是,由于分配的是特定长度的内存,因此无法有效利用分配的内存。例如,将 100 字节的数据缓存到 128 字节的 chunk 中,剩余的 28 字节就浪费了 :(

![](2.png)


## Grow Factor增长因子调优

memcached 在启动时可以通过­f 选项指定 Growth Factor 因子, 并在某种程度上控制 slab之间的差异. 默认值为 1.25. 但是,在该选项出现之前,这个因子曾经固定为 2,称为”powers of 2”策略。

增长因子是2时chunk的大小分布

```
gaosongdeMacBook-Pro:~ Ken$ memcached -f 2 -vvv
slab class   1: chunk size        96 perslab   10922
slab class   2: chunk size       192 perslab    5461
slab class   3: chunk size       384 perslab    2730
slab class   4: chunk size       768 perslab    1365
slab class   5: chunk size      1536 perslab     682
slab class   6: chunk size      3072 perslab     341
slab class   7: chunk size      6144 perslab     170
slab class   8: chunk size     12288 perslab      85
slab class   9: chunk size     24576 perslab      42
slab class  10: chunk size     49152 perslab      21
slab class  11: chunk size     98304 perslab      10
slab class  12: chunk size    196608 perslab       5
slab class  13: chunk size    393216 perslab       2
slab class  14: chunk size   1048576 perslab       1
```

增长因子是1.25时chunk的大小分布

```
gaosongdeMacBook-Pro:~ Ken$ memcached -f 1.25 -vvv
slab class   1: chunk size        96 perslab   10922
slab class   2: chunk size       120 perslab    8738
slab class   3: chunk size       152 perslab    6898
slab class   4: chunk size       192 perslab    5461
slab class   5: chunk size       240 perslab    4369
slab class   6: chunk size       304 perslab    3449
slab class   7: chunk size       384 perslab    2730
slab class   8: chunk size       480 perslab    2184
slab class   9: chunk size       600 perslab    1747
slab class  10: chunk size       752 perslab    1394
slab class  11: chunk size       944 perslab    1110
slab class  12: chunk size      1184 perslab     885
slab class  13: chunk size      1480 perslab     708
slab class  14: chunk size      1856 perslab     564
slab class  15: chunk size      2320 perslab     451
slab class  16: chunk size      2904 perslab     361
slab class  17: chunk size      3632 perslab     288
slab class  18: chunk size      4544 perslab     230
slab class  19: chunk size      5680 perslab     184
slab class  20: chunk size      7104 perslab     147
slab class  21: chunk size      8880 perslab     118
slab class  22: chunk size     11104 perslab      94
slab class  23: chunk size     13880 perslab      75
slab class  24: chunk size     17352 perslab      60
slab class  25: chunk size     21696 perslab      48
slab class  26: chunk size     27120 perslab      38
slab class  27: chunk size     33904 perslab      30
slab class  28: chunk size     42384 perslab      24
slab class  29: chunk size     52984 perslab      19
slab class  30: chunk size     66232 perslab      15
slab class  31: chunk size     82792 perslab      12
slab class  32: chunk size    103496 perslab      10
slab class  33: chunk size    129376 perslab       8
slab class  34: chunk size    161720 perslab       6
slab class  35: chunk size    202152 perslab       5
slab class  36: chunk size    252696 perslab       4
slab class  37: chunk size    315872 perslab       3
slab class  38: chunk size    394840 perslab       2
slab class  39: chunk size    493552 perslab       2
slab class  40: chunk size    616944 perslab       1
slab class  41: chunk size    771184 perslab       1
slab class  42: chunk size   1048576 perslab       1
```

可见,组间差距比因子为 2 时小得多,更适合缓存几百字节的记录。从上面的输出结果来看,可能 会觉得有些计算误差,这些误差是为了保持字节数的对齐而故意设置的。 

将 memcached 引入产品,或是直接使用默认值进行部署时,最好是重新计算一下数据的预期平均长度,调整 growth factor,以获得最恰当的设置。内存是珍贵的资源,浪费就太可惜了。 










 
