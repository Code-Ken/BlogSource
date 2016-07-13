title: JSONP跨域问题的解决方法
date: 2016-07-13 22:41:19
categories: HTTP
tags: [HTTP,跨域,Ajax,JSON,JSONP]
---

<center>JSONP介绍与应用</center>
<!--more-->

## Ajax技术
Asynchronous JavaScript and XML (Ajax) 是Web2.0的关键技术。
Ajax允许在不干扰Web应用程序的显示和行为的情况下在后台进行数据检索。使用XMLHttpRequest函数获取数据，它是一种API,允许客户端JavaScript 通过 HTTP 连接到远程服务器。

不过，由于受到浏览器的限制，该方法`不允许跨域通信`。如果尝试从不同的域请求数据，会出现安全错误。

## 啥是跨域

   所有的浏览器都遵守同源策略，这个策略能够保证一个源的动态脚本不能读取或操作其他源的http响应和cookie，这就使浏览器隔离了来自不同源的内容，防止它们互相操作。所谓同源是指`协议、域名和端口`都一致的情况。
简单的来说，出于安全方面的考虑，页面中的JavaScript无法访问其他服务器上的数据，即“同源策略”。而跨域就是通过某些手段来绕过同源策略限制，实现不同服务器之间通信的效果。

举例说明：

![](1.png)


## 怎么解决跨域问题

 - 第三方网站开启HTTP的Access-Control-Allow-Origin参数
   
只有当目标页面的response中，包含了 Access-Control-Allow-Origin 这个header，并且它的值里有我们自己的域名时，浏览器才允许我们拿到它页面的数据进行下一步处理。如：

```
Access-Control-Allow-Origin: http://run.jsbin.io
```

如果它的值设为 * ，则表示谁都可以用：

```
Access-Control-Allow-Origin: *
```

`但是谁又喜欢被不认识的人上呢？`
   
 - 将网站自身服务器做中转服务器请求跨域网站，绕过跨域问题

![](2.png)

⚠️注意：实际请求中B Server实际是返回了数据的只是浏览器不让使用而已～

浏览器不让使用:
![](3.png)

用Charles监控的数据
![](4.png)
![](5.png)

- 使用JSONP解决跨域问题

## 什么是JSONP

- 别人发现，Web页面上调用js文件时则不受是否跨域的影响（不仅如此，别人还发现凡是拥有"src"这个属性的标签都拥有跨域的能力，比如script、img、iframe）
- 于是可以判断，当前阶段如果想通过纯web端（ActiveX控件、服务端代理、属于未来的HTML5之Websocket等方式不算）跨域访问数据就只有一种可能，那就是在远程服务器上设法把数据装进js格式的文件里，供客户端调用和进一步处理；
- 恰巧我们已经知道有一种叫做JSON的纯字符数据格式可以简洁的描述复杂数据，更妙的是JSON还被js原生支持，所以在客户端几乎可以随心所欲的处理这种格式的数据；
- 这样子解决方案就呼之欲出了，web客户端通过与调用脚本一模一样的方式，来调用跨域服务器上动态生成的js格式文件（一般以JSON为后缀），显而易见，服务器之所以要动态生成JSON文件，目的就在于把客户端需要的数据装入进去。
- 客户端在对JSON文件调用成功之后，也就获得了自己所需的数据，剩下的就是按照自己需求进行处理和展现了，这种获取远程数据的方式看起来非常像AJAX，但其实并不一样。
- 千言万语汇成一句话:JSONP是一段参数是json格式(大多数情况)的JS代码

## JSONP的具体实现

- 静态调取js文件方式实现

```
#php代码
exit('demo({height: 170, weight: 90})');

#html代码
<script type="text/javascript">
    function demo(data) {
        alert("身高: " + data.height + ", 体重: " + data.weight);
    }
</script>	
<script type="text/javascript" src="http://localhost:1111"></script>

```

- 动态调取js文件方式实现

```
#php代码
exit('demo({height: 170, weight: 90})');

#html代码
<script type="text/javascript">
    function demo(data) {
        alert("身高: " + data.height + ", 体重: " + data.weight);
    }

    var url = "localhost:1111"; 
    var script = document.createElement('script');
    script.setAttribute('src', url);
</script>	
```

- 动态获取callback函数


```
#上面的callback函数都是Sever写死的,我们可以用url传参方式动态获取回调函数名称
#php代码
exit($_GET['callback'].'({height: 170, weight: 90})');

#html代码
<script type="text/javascript">
    function demo(data) {
        alert("身高: " + data.height + ", 体重: " + data.weight);
    }

    var url = "localhost:1111"; 
    var script = document.createElement('script');
    script.setAttribute('src', url+'?callback=demo');
</script>
```

- 使用JQuery处理JSONP

```
#php代码
exit($_GET['callback'].'({height: 170, weight: 90})');

#html代码
<script src="http://libs.baidu.com/jquery/1.9.0/jquery.js"></script>
<script type="text/javascript">
	var url = 'localhost:1111';
	$.ajax ({
		url: url,
		dataType: 'jsonp',
		success: function (data) {
		alert("身高: " + data.height + ", 体重: " + data.weight);
	},
	});
</script>
```

$.ajax函数判断dataType为jsonp时会自动加入callback函数,并在success中自动调取:


![](6.png)

![](7.png)


## JSONP服务器错误的处理

当我们用ajax请求jsonp时,如果如果服务器端的错误我们是不能通过ajax原生的error属性来捕获到错误的:
>error
A function to be called if the request fails (...) Note: This handler is not called for cross-domain script and cross-domain JSONP requests. This is an Ajax Event.


## JSONP POST问题
JsonP only works with type: GET
>script”: Evaluates the response as JavaScript and returns it as plain text. Disables caching by appending a query string parameter, “_=[TIMESTAMP]”, to the URL unless the cache option is set to true. Note: This will turn POSTs into GETs for remote-domain requests.

即使你写成POST方式,ajax也会给你转成get方式,如果想向第三方网站提交数据,最好还是先提交到自己的服务器上，通过自己的服务器中转到第三方服务器


## 参考资料

- [使用 JSONP 实现跨域通信，第 1 部分: 结合 JSONP 和 jQuery 快速构建强大的 mashup](http://www.ibm.com/developerworks/cn/web/wa-aj-jsonp1/index.html)
- [JSONP与POST方式请求](http://www.xiaoxiaozi.com/2011/11/25/2239/)
- [Stack Overflow:JSONP request error handling](http://stackoverflow.com/questions/19035557/jsonp-request-error-handling)
- [利用JSONP解决AJAX跨域问题的原理与jQuery解决方案](http://www.open-open.com/lib/view/open1344558130468.html)
- [【原创】说说JSON和JSONP，也许你会豁然开朗，含jQuery用例](http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html)
