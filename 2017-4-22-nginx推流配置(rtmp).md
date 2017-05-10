---
layout: post
title:  "nginx推流"
category: "note"
keywords: ["nginx", "rtmp","推流"]
description: "nginx推流"
tags: ["nginx", "rtmp","推流"]
---
### [copy](http://cxuef.github.io/linux/%E3%80%90%E7%BD%AE%E9%A1%B6%E3%80%91%E6%90%AD%E5%BB%BAnginx-rtmp%E7%9B%B4%E6%92%AD%E6%9C%8D%E5%8A%A1%E5%99%A8%EF%BC%8Cffmpeg%E6%A8%A1%E6%8B%9F%E6%8E%A8%E6%B5%81/)

## 1.下载nginx和nginx-rtmp-module模块 并 解压
```
[nginx下载](http://nginx.org/en/download.html)
dy@dy-X455LJ:~/github/nginx-demo$ git clone https://github.com/arut/nginx-rtmp-module.git
```
## 2.配置nginx
```
//配置说明
dy@dy-X455LJ:~/github/nginx-demo/nginx-1.11.13$ ./configure --prefix=/usr/local/nginx --add-module=/home/dy/github/nginx-demo/nginx-rtmp-module --with-http_ssl_module --with-debug
//--prefix：安装到哪个文件夹
//--add-module:添加模块
```
## 3.编译安装
```
dy@dy-X455LJ:~/github/nginx-demo/nginx-1.11.13$ make
dy@dy-X455LJ:~/github/nginx-demo/nginx-1.11.13$ make install
```
检查/usr/local/nginx/(刚刚--prefix后的文件夹)下的文件，应该有四个文件夹，分别是conf,sbin,html,logs。
再检查sbin下是否有nginx文件。
然后更改文件夹conf下的nginx.conf文件，添加如下内容：
```
rtmp {
   server {
       listen 1935;
       chunk_size 4096;
       application myapp {
           live on;
       }
       application hls {
           live on;
           hls on;
           hls_path /tmp/hls;
       }
   }
}
```
保存之后把conf下的文件全部移动到/usr/local/nginx/下，然后使配置生效:
```
dy@dy-X455LJ:/usr/local/nginx/sbin$ sudo ./nginx -c nginx.conf
```
启动nginx
```
dy@dy-X455LJ:/usr/local/nginx/sbin$ sudo ./nginx
```
## 4.测试
先在浏览器输入localhost，看是否能成功进入nginx的欢迎页面。
然后安装ffmpeg
```
sudo apt install ffmpeg
```
推流
```
//ffmpeg -re -i a.flv -f flv rtmp://192.168.1.170/myapp/test1     //貌似不行
ffmpeg -re -i a.mp4 -vprofile baseline -vcodec copy -acodec copy -strict -2 -f flv rtmp://192.168.1.170/myapp/test2      
```
播放
```
ffplay rtmp://192.168.1.170/myapp/test1
```
