title: Mysql缓存技术
date: 2016-03-24 22:47:29
categories: Mysql
tags: [Mysql,缓存]
---

<center>本文主要介绍mysql缓存技术</center>

<!--more-->


## Mysql缓存特征
- MysqlL查询缓存机制是MySQL数据库中的重要机制之一
- 缓存sql文本及查询结果，如果运行相同的sql，服务器直接从缓存中取到结果，而不需要再去解析和执行sql
- Mysql缓存适用于那些不常变化的表
- 缓存的结果是通过sessions共享的，所以一个client查询的缓存结果，另一个client也可以使用
- 缓存不会返回过时的数据

## 哪些情况下Mysql缓存不起作用
- Mysql缓存在分库分表环境下是不起作用的
- 执行sql里有触发器,自定义函数,或一些变化的函数和事件时,mysql缓存也是不起作用的
- 如果表变化了，那么这个表的所有mysql缓存就都会失效,变化操作包括更新,插入删除
- mysql缓存同样也可以在事务里使用


- 查询语句是一个外部查询的子查询时,Mysql缓存是不起作用的

```
mysql> SHOW STATUS LIKE 'Qcache_hits';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Qcache_hits   | 178   |
+---------------+-------+
1 row in set (0.02 sec)

mysql> SELECT * FROM adminlog WHERE adminlog_id = ANY (SELECT ent_area_id FROM ent);
#数据隐藏
74 rows in set (0.02 sec)

mysql> SHOW STATUS LIKE 'Qcache_hits';                           
| Variable_name | Value |
+---------------+-------+
| Qcache_hits   | 178   |
+---------------+-------+
1 row in set (0.01 sec)

mysql> SELECT * FROM adminlog WHERE adminlog_id = ANY (SELECT ent_area_id FROM ent);
#数据隐藏
3 rows in set (0.09 sec)

mysql> SHOW STATUS LIKE 'Qcache_hits';                      
| Variable_name | Value |
+---------------+-------+
| Qcache_hits   | 179   |
+---------------+-------+
1 row in set (0.01 sec)

mysql> SELECT ent_area_id FROM ent; 
#数据隐藏
74 rows in set (0.02 sec)

mysql> SHOW STATUS LIKE 'Qcache_hits';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Qcache_hits   | 179   |
+---------------+-------+
1 row in set (0.02 sec)

```

⚠️注意:
>查询必须是完全相同的(逐字节相同)才能够被认为是相同的。
另外，同样的查询字符串由于其它原因可能认为是不同的。
使用不同的数据库、不同的协议版本或者不同默认字符集的查询被认为是不同的查询并且分别进行缓存。

下面sql查询缓存认为是不同的

```
    SELECT * FROM tbl_name  
    Select * from tbl_name  
```

## Mysql缓存相关参数

```
mysql> show variables like '%query_cache%';
+------------------------------+---------+
| Variable_name                | Value   |
+------------------------------+---------+
| have_query_cache             | YES     |      --查询缓存是否可用
| query_cache_limit            | 1048576 |      --可缓存具体查询结果的最大值
| query_cache_min_res_unit     | 4096    |      --查询缓存分配的最小块的大小(字节)
| query_cache_size             | 599040  |      --查询缓存的大小
| query_cache_type             | ON      |      --是否支持查询缓存
| query_cache_wlock_invalidate | OFF     |      --控制当有写锁加在表上的时候，是否先让该表相关的 Query Cache失效
+------------------------------+---------+
6 rows in set (0.02 sec)
```
- `query_cache_min_res_unit`
当查询进行的时候，Mysql把查询结果保存在qurey cache中，但如果要保存的结果比较大，超过query_cache_min_res_unit的值 ，这时候mysql将一边检索结果，一边进行保存结果，所以，有时候并不是把所有结果全部得到后再进行一次性保存，而是每次分配一块query_cache_min_res_unit大小的内存空间保存结果集，使用完后，接着再分配一个这样的块，如果还不不够，接着再分配一个块，依此类推，也就是说，有可能在一次查询中，mysql要进行多次内存分配的操作。
适当的调节query_cache_min_res_unit可以优化内存
如果你的查询结果都是一些small result,默认的query_cache_min_res_unit可能会造成大量的内存碎片
如果你的查询结果都是一些larger resule，你可以适当的把query_cache_min_res_unit调大

- `query_cache_type`
如果query_cache_type的值是0或者OFF，不会读取缓存
如果query_cache_type的值是1或者ON会读取缓存除非sql语句中注明“SELECT SQL_NO_CACHE”
如果query_cache_type的值是2或者DEMAND只有在sql语句中注明SELECT SQL_CACHE才会读取缓存

- `query_cache_size`
官方建议缓存大小控制在几十兆，不要设置成几百兆，如果设置的太小也是不可以的,设置小于40Kb会有警告。

## 开启缓存的方式

```
mysql> set global query_cache_size = 600000; --设置缓存内存大小
mysql> set global query_cache_type = ON;     --开启查询缓存
```

⚠️注意
>set global时需要有SUPER权限

如果在开启Mysql缓存状态下想针对某个表不让它进行缓存查询，此时可以这样写

```
select SQL_NO_CACHE * from table_name; 
```

## 查询Mysql缓存状态

```
mysql> SHOW STATUS LIKE 'Qcache%';
+-------------------------+--------+
| Variable_name           | Value  |
+-------------------------+--------+
| Qcache_free_blocks      | 1      | ----在查询缓存中的闲置块
| Qcache_free_memory      | 382704 | ----剩余缓存的大小
| Qcache_hits             | 198    | ----缓存命中次数
| Qcache_inserts          | 131    | ----缓存被插入的次数
| Qcache_lowmem_prunes    | 0      | ----由于内存低而被删除掉的缓存条数
| Qcache_not_cached       | 169    | ----没有被缓存的条数
| Qcache_queries_in_cache | 128    | ----缓存中有多少条查询语句
| Qcache_total_blocks     | 281    | ----总块数
+-------------------------+--------+
8 rows in set (0.00 sec)
```


## 参考文档
- [MysqlDocument](http://dev.mysql.com/doc/refman/5.6/en/)




