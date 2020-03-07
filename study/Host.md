## Host

host 是HTTP 1.1协议中新增的一个请求头，主要用来实现虚拟主机技术

虚拟主机（virtual hosting）即共享主机（shared web hosting），可以利用虚拟技术把一台完整的服务器分成若干个主机，因此可以在单一主机上运行多个网站或者服务

当服务器接收到来自浏览器的请求时，会根据请求头中的host字段来决定访问哪个站点。

比如一个服务器的ip地址为 120.32.64.288，有三个www.baidu.com, www.google.com, www.bing.com的域名解析到这三个网站上。当我们在浏览器中请求通过www.baidu.com这个网址去访问，DNS解析出了120.32.64.288这个ip地址。这个ip地址上有三个网站，那么服务器就是根据请求头中的host字段来选择使用www.baidu.com这个域名的网站程序对请求做出响应。

如果在Http 1.1中没有指定host，会返回404



TODO: 以后理解一下这个

https://blog.csdn.net/zj6257/article/details/83012282