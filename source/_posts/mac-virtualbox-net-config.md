title: Mac使用Virtualbox安装Centos的网络配置
date: 2016-03-30 10:26:32
categories: Linux
tags: [Linux]
---

<center>真是坑！！！</center>

<!--more-->


## virtualbox中设置

![](1.png)

## Mac网络设置

![](2.png)

## 对centos的一些修改

```
vi /etc/resolv.conf
    #这个要对应上mac网络上的IP地址
    nameserver 192.168.0.8

vi /etc/sysconfig/network
    NETWORKING=yes

vi /etc/sysconfig/network-scripts/ifcfg-eth0 
    #这个文件只保留这些就可以
    DEVICE=eth0
    ONBOOT=yes
    MM_CONTROLLED=no
    BOOTPROTO=dhcp

vi /etc/yum/pluginconf.d/fastestmirror.conf
    enabled=0

chkconfig iptables off
/etc/init.d/network restart
```
