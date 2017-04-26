---

layout: post
title: "python流量嗅探学习笔记(一)"
category: "python"
keywords: ["python", "网络"]
description: "python 网络"
tags: ["python", "网络"]
date: 2016-10-02

---

## python原始套接字流量嗅探步骤：

### 1.创建原始套接字，绑定在公开接口上：

windows下，我们可以嗅探所有协议的数据包，所以win下绑定接口的方法是:

```
socket_protocol = socket.IPPROTO_IP
```

而在linux下，只能嗅探到ICMP数据：

```
socket_protocol = socket.IPPROTO_ICMP
```

绑定好后，创建套接字：

```
sniffer = socket.socket(socket.AF_INET,socket.SOCK_RAW,socket_protocol)
```

可以看出来，这个套接字与一般的套接字有所不同，以前创建时，代码是这样的：

```
#tcp
socket.socket(socket.AF_INET,socket.SOCK_STREAM)
#UDP
socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
```

socket对象中的参数AF_INET表示使用ipv4协议，SOCK_STREAM表示使用基于流的tcp协议，SOCK_DGRAM表示使用不连续不可靠的数据包连接（UDP），其中有两个参数:socket.SOCK_RAW和socket_protocol,与之前有所不同，后者的作用是一眼就能看出来(绑定接口)，SOCK_RAW表示提供原始网络协议存取。

### 2.设置捕获的数据包中包含ip头

```
sniffer.setsockopt(socket.IPPROTO_IP,socket.IPPROTO_HDRINCL,1)
```

### 3.读取单个数据包

```
print sniffer.recvfrom(65535)
```
