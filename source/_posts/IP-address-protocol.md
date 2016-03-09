title: IP地址与IP协议
date: 2016-03-09 11:27:25
categories: 网络协议
tags: [网络协议,IP]
---

<center>主要介绍IP地址的知识与IP协议的知识</center>

<!--more-->

​
## IP地址
### 什么是IP地址
在TCP/IP协议族的IP层使用的用来标志连接在因特网上的每一个设备的标识符。

- 32位地址IP地址由32bit构成，
- 每8bit一组，共占用4个字节
- IP地址由两部分组成，网络位和主机位

### IP地址表示方法

- 二进制记法  
    10000000 00001011 00000011 00011111
- 点分十进制(最常用的表示方法)
    128.11.3.31
- 十六进制记法
    0X810B0BEF

### IP地址分类

每一类地址都由两个固定长度的字段组成：
- 一个字段是网络号 net-id，它标志主机（或路由器）所连接到的网络
- 一个字段则是主机号 host-id，它标志该主机（或路由器）

IP 地址 ::= { <网络号>, <主机号>}   

![](1.png)

IP地址分为五类：

- A类地址，net-id占1字节，host-id占3字节
- B类地址，net-id占2字节，host-id占2字节    
- C类地址，net-id占3字节，host-id占1字节
- D类地址作为多播地址
- E类地址保留为今后使用

#### 还回地址
第一个字节等于127的IP地址用作还回地址。用于网络软件测试以及本机进程之间通信的特殊地址。 
点分十进制表示为127.0.0.0，只用作目的地址。

### IP地址使用范围

![](2.png)

### 子网和超网

子网是为了将一个IP地址对内划分成多个IP地址，通过子网掩码
超网是为了将多个IP地址对外合并成一个IP地址，通过超网掩码

## IP协议

### IP协议特点

- 无连接性:  跨越多个异构物理网 通用性
- 不可靠:  可靠性问题交由高层协议解决
- 数据报交付协议

### IP数据报传输

数据报传输时是需要分片和合并的，为什么要进行分割呢，主要原因是每个数据链路的MTU(最大传输单元)不尽相同，数据传输时数据报的最大不能超过MTU，故需要分片，有时还可能进行多重分片

![](3.png)


数据报的重组操作，由于传输时每片数据报传输的路径不一定相同，故重组只能在目的主机上进行，有重组时限的限制，若丢失了分片，则无法重组

### IP分组格式

- 数据报：首部 + 数据
- 首部：各字段的含义、作用、取值（长度、TTL、协议、片标志、片偏移、校验和）
- 封装：直接封装在数据帧中

### IP分组处理

TTL、校验和计算、分片和重组、选项处理

