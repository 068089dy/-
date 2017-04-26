---
layout: post
title: "python流量嗅探学习笔记(二)"
category: "python"
keywords: ["python", "网络"]
description: "python 网络"
tags: ["python", "网络"]
date: 2016-10-03
---


首先创建一个结构体类，我们要用到struct这个模块：

```
import struct
```

然后创建结构体类

```
class IP(Structure):
  _fields_ = [

  ]
```

然后根据ipv4的结构（如图）
![img](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/2016-10-3-python-sniffer-note2/ipv4.jpg)
我们创建结构体：

```
    ("ihl",          c_ubyte,4),                 #4位 头长途
    ("version",      c_ubyte,4),                 #4位 版本号
    ("tos",          c_ubyte),                   #8位 服务类型
    ("len",          c_ushort),                  #16位 ip数据包总长
    ("id",           c_ushort),                  #3位 标识符
    ("offset",       c_ushort),                  #13位 片偏移
    ("ttl",          c_ubyte),                   #8位 生存时间
    ("protocol_num", c_ubyte),                   #8位 协议类型
    ("sum",          c_ushort),                  #16位 校验和
    ("src",          c_uint),                    #32位 源ip
    ("dst",          c_uint),                    #32位 目标ip
```

然后就要把我们捕获到的数据放入结构体中：
捕获的方法和[上次]()一样:

```
host = "127.0.0.1"
socket_protocol = socket.IPPROTO_ICMP
sniffer = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket_protocol)
sniffer.bind((host, 0))
sniffer.setsockopt(socket.IPPROTO_IP, socket.IP_HDRINCL, 1)
```

传入结构体类中：

```
raw_buffer = sniffer.recvfrom(65535)[0]
ip_header = IP(raw_buffer[0:20])      
```

因为__init__是在类实例对象创建之后调用的方法，而类IP在创建时就需要传入数据，所以我们要用到另一个特殊的方法————__new__,方法__new__是创建类实例的方法：

```
def __new__(self, socket_buffer=None):
  return self.from_buffer_copy(socket_buffer)
```

这样就在创建类时传入了数据，然后我们就可以像使用类中变量一样使用结构体中的变量了：
```
version = ip_header.version
print "ip version is",version
```

关于源ip和目标ip的转化：
```
src_address = socket.inet_ntoa(struct.pack("<I",ip_header.src))
dst_address = socket.inet_ntoa(struct.pack("<I",ip_header.dst))
```
struct中的pack方法是把字节流ip_header.src按照前面"<I"的格式转化为字符串，然后再用socket中的inet_ntoa方法转化为常见的格式，[关于struct的详细使用方法](http://www.cnblogs.com/gala/archive/2011/09/22/2184801.html)。

然后，为了方便以后粘贴复制，先把源码贴着：
```
# /usr/bin/python
# coding:utf-8

import socket
import struct
from ctypes import *

class IP(Structure):
    _fields_ = [
        ("ihl",          c_ubyte,4),                 #4位 头长途
        ("version",      c_ubyte,4),                 #4位 版本号
        ("tos",          c_ubyte),                   #8位 服务类型
        ("len",          c_ushort),                  #16位 ip数据包总长
        ("id",           c_ushort),                  #3位 标识符
        ("offset",       c_ushort),                  #13位 片偏移
        ("ttl",          c_ubyte),                   #8位 生存时间
        ("protocol_num", c_ubyte),                   #8位 协议类型
        ("sum",          c_ushort),                  #16位 校验和
        ("src",          c_uint),                    #32位 源ip
        ("dst",          c_uint),                    #32位 目标ip
    ]

    def __init__(self, socket_buffer=None):

        self.protocol_map={1:"ICMP",6:"TCP",17:"UDP"}
        self.src_address = socket.inet_ntoa(struct.pack("<I",self.src))
        self.dst_address = socket.inet_ntoa(struct.pack("<I",self.dst))
        try:
            self.protocol = self.protocol_map[self.protocol_num]
        except:
            self.protocol = str(self.protocol_num)

    def __new__(self, socket_buffer=None):
        return self.from_buffer_copy(socket_buffer)


host = "127.0.0.1"
socket_protocol = socket.IPPROTO_ICMP
sniffer = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket_protocol)
sniffer.bind((host, 0))
sniffer.setsockopt(socket.IPPROTO_IP, socket.IP_HDRINCL, 1)

while True:
    raw_buffer = sniffer.recvfrom(65535)[0]
    ip_header = IP(raw_buffer[0:20])
    print "%s:%s->%s"%(ip_header.protocol,ip_header.src_address,ip_header.dst_address)

```
