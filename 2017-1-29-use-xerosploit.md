---
layout: post
title: "使用xerosploit"
category: "note"
keywords: ["note", "内网安全","xerospolit","渗透工具"]
description: "使用xerosploit"
tags: ["project", "内网安全","xerospolit","渗透工具"]
---

Xerosploit是一个渗透测试工具包，通过调用bettercap与nmap，可以进行中间人攻击。它具有各种模块，可以实现高效的攻击，并允许执行拒绝服务攻击和端口扫描。[原文](https://zhuanlan.zhihu.com/p/25031766?utm_source=org.telegram.messenger&utm_medium=social)

## 第一步.安装nmap
```
sudo apt install nmap
```
## 第二步.安装bettercap [原文](http://bbs.ichunqiu.com/thread-6915-1-1.html)
1.安装ruby
```
sudo apt-get install ruby
```
2.成功安装之后，我们来将ruby的源更换成淘宝提供的源
```
gem sources --remove https://rubygems.org/ //删除原本的ruby镜像
gem sources -a https://ruby.taobao.org/ //添加taobao的源gem sources -l //查看当前的源 如图
gem install foo //更新一下
```
3.下载安装bettercap
```
git clone https://github.com/evilsocket/bettercap  //从github上下载bettercap
cd bettercap  //进入 bettcercap文件夹
gem build bettercap.gemspec //将berrercap打包成一个gem文件
sudo gem install berrercap*.gem  //root权限安装bettercap
```
安装失败 [解决](http://stackoverflow.com/questions/22544754/failed-to-build-gem-native-extension-installing-compass)

```

apt-get install ruby-dev

```

又报错 [解决](www.2cto.com/article/201507/425092.html)

```
sudo apt-get install libpcap-dev
sudo bettercap -h //root权限启动bettercap,并且-h查看帮助文档
```

## 第三步.安装xerosploit
```
git clone LionSec/xerosploit
cd xerosploit/
./install.py
```
## 第四步.使用xerosploit[视频教程](https://www.youtube.com/watch?v=AEjMEz_ugVM)
