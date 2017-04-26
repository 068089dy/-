---
layout: post
title:  "中间人攻防(一)"
category: "linux"
keywords: ["linux", "网络"]
description: "中间人攻防"
tags: ["linux", "网络"]
---
![img](https://github.com/068089dy/068089dy.github.io/blob/master/media/img/bg.jpg?raw=true)
中间人攻击是一种常见的计算机攻击手段，其原理是攻击者主机冒充网关对被攻击者的接收数据动手脚，下面我们来讲解一下如何进行中间人攻击，并且如何有效的应对这种攻击。
# 1.攻击
我们首先要保证攻击机和被攻击机在同一局域网下，还要用到一款软件‘ettercap’，它可谓是局域网攻击神器，ubuntu/debian下利用apt直接进行下载
```
sudo apt-get install ettercap
```
（用阿里云的源可以直接下载）,或者在[github](https://github.com/Ettercap/ettercap)上下载源码。
安装完成之后，打开  
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/ettercap1.png)
然后，
sniff->unified sniffing，
选择网口，我的是wlp3s0。
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/ettercap2.png)
然后，
host->scan for host
扫描局域网内主机
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/ettercap3.png)
然后，
sniff->host list
列出所有主机
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/ettercap4.png)
然后，选择插件
plugins->manage the plugin
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/etter5.png)
然后双击dns_spoff(dns欺骗)
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/etter6.png)
然后返回host list，将网关（192.168.1.1）添加进target2,将要攻击的主机添加进target1。
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/etter7.png)
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/etter8.png)
然后，mitm->arp poisoning(arp污染)
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/etter10.png)
然后start->start sniffing（开始嗅探）
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/etter9.png)
这样我们已经成功的冒充了网关，也就是说可以做一些坏坏的事情了：）  
例如，查看他（她）浏览的图片：）  
然而我们还需要一个图片捕获工具（driftnet）  
ubuntu下，可以
{% highlight bash%}
sudo apt-get install driftnet
```
直接下，还是阿里云的源哦：）  
然后在终端输入
```
sudo driftnet -i wlp3s0
```
我攻击了我的手机，这是我手机在访问b站时的画面
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/bili2.jpg)
这是我的电脑截取到的
![](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E9%98%B21/bili1.png)
是不是因吹斯丁：）
