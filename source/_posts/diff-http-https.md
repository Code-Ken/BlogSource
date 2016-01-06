title: HTTP和HTTPS的区别
date: 2016-01-05 16:55:38
categories: Tcp/Ip
tags: [http,https]
---

<center>本文主要讲解http和https的关系与区别</center>
<!--more-->

## 注意⚠️
- https协议需要到ca申请证书，一般免费证书很少，需要交费。
- http是超文本传输协议，信息是明文传输，https 则是具有安全性的ssl加密传输协议
- http和https使用的是完全不同的连接方式用的端口也不一样,前者是80,后者是443

## HTTP的缺点

HTTP主要有这些不足：
- 通信使用明文,内容可能被窃听

![](/diff-1.png)

- 不验证通信方身份,因此有可能遭遇伪装

![](/diff-2.png)

- 无法验证报文的完整性,所有有可能已篡改

![](/diff-3.png)

## HTTP + 加密 + 认证 + 完整性保护 = HTTPS

![](/diff-4.png)


### HTTPS是身披SSL外壳的HTTP
通常情况下HTTP是直接和TCP层进行通信的。当使用SSL(安全套阶字)时,则演变成HTTP先和SSL通信,SSL再和TCP通信的了。

![](/diff-5.png)

### 加密技术
讲解SSL前,科普一下加密方法,SSL采用的是一种叫做`公开密钥加密`的加密处理方式
#### 对称加密
加密和解密用的一个密钥的方式称为对称加密,也叫做`共享密钥加密`

![](/diff-6.png)


对称加密在发送加密信息时也需要将密钥发送给对方,但这样可以被攻击者截取,就不安全啦～

![](/diff-7.png)


#### 非对称加密
非对称加密又称作`公开密钥加密`,它很好的解决了对称加密密钥被截取的问题。
非对称加密采用一对非对称的密钥,一把叫做私有密钥,一把叫做共有密钥。
使用非对称加密,发送密文一方使用对方的共有密钥进行加密处理,对方收到加密信息后,再使用自己的私有密钥进行解密。

![](/diff-8.png)

#### HTTPS采用混合加密机制
HTTPS采用对称加密和非对称加密所混合的加密机制。
若密钥能安全交换,那么有可能仅考虑非对称加密。
但是非对称加密与对称加密相比,处理速度相对较慢。
![](/diff-9.png)


#### 公开密钥的认证
使用数字证书认证机构和其颁布的公开密钥证书进行认证。即让第三方独立机构进行验证。

![](/diff-10.png)
![](/diff-11.png)

私有密钥是保存在服务器端的～
注意⚠️：认证是要钱的！！！


### HTTPS安全通信机制

![](/diff-12.png)

下图是完整的HTTPS的通信过程

![](/diff-13.png)


### 为什么HTTPS不是那么普及
1.加密通信与纯文本通信相比,消耗更多的CPU和内存资源

2.购买证书是要钱的！

3.少许对客户端有要求的情况下,会要求客户端也必须有一个证书.
- 这里客户端证书,其实就类似表示个人信息的时候,除了用户名/密码, 还有一个CA 认证过的身份. 应为个人证书一般来说上别人无法模拟的,所有这样能够更深的确认自己的身份
- 目前少数个人银行的专业版是这种做法,具体证书可能是拿U盘作为一个备份的载体

### HTTPS 一定是繁琐的
1.本来简单的http协议,一个get一个response. 由于https 要还密钥和确认加密算法的需要.单握手就需要6/7 个往返,任何应用中,过多的round trip 肯定影响性能.
2.接下来才是具体的http协议,每一次响应或者请求, 都要求客户端和服务端对会话的内容做加密/解密,尽管对称加密/解密效率比较高,可是仍然要消耗过多的CPU,为此有专门的SSL 芯片. 如果CPU 信能比较低的话,肯定会降低性能,从而不能serve 更多的请求,加密后数据量的影响. 所以,才会出现那么多的安全认证提示







































## 参考文档
1.《图解HTTP》,【日】上野宣 








