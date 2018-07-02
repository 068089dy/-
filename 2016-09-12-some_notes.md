---
layout: post
title: "一些杂记"
category: "note"
keywords: ["note", "网络"]
description: "杂记"
tags: ["note", "网络"]
---

## 封ip

封禁单个 IP
```
iptables -I INPUT -s 192.168.1.100 -j DROP
```
封禁一个 IP 段
```
iptables -I INPUT -s 192.168.1.100/116 -j DROP
```

## 一些简单的git命令

```
Global setup:  
 Set up git  
  git config --global user.name "Your Name"  
  git config --global user.email your.email@gmail.com  

Next steps:  
  mkdir gitbook  
  cd gitbook  
  git init  
  touch README  
  git add README  
  git commit -m 'first commit'  
  git remote add origin git@github.com:vogella/gitbook.git  
  git push -u origin master  

Existing Git Repo?  
  cd existing_git_repo  
  git remote add origin git@github.com:vogella/gitbook.git  
  git push -u origin master  
```

## 修改ip地址

```
ifconfig eth0 192.168.0.20 netmask 255.255.255.0
```

## 安装FLUXION时

出现Dhcpd...........not install时，安装这个：
```
sudo apt-get install isc-dhcp-server
```
Bully...........Not installed
Mdk3............Not installed

## 开启apache服务

```
sudo /etc/init.d/apache2 start  
```

## ubuntu禁用报告
```
sudo gedit /etc/default/apport
```
打开后把enable=1改为0;

## 查看ubuntu版本
```
lsb_release -a
uname -a
```

## ubuntu安装flash
chrome
```
sudo apt-get install pepperflashplugin-nonfree
sudo update-pepperflashplugin-nonfree --install
```
firefox
```
sudo apt install flashplugin-installer
```

## 更改gnome桌面主题

将主题复制到usr/share/themes
启动gnome--tweak-tool

## apt
```
apt-cache dumpavail //列出全部
          search
          show
apt-get autoremove //自动删除没用的软件依赖
```


## 免流教程

http://www.gaojinan.com/vps-openvpn-china-telecom-unicom-mobile-mianliu-ml.html#comments


## Linux SSH远程文件/目录传输命令scp

3、将本地文件上传到服务器上
```
scp -P 2222 /home/lnmp0.4.tar.gz root@www.vpser.net:/root/lnmp0.4.tar.gz  #传输文件
scp -r openfire/ root@www.vpser.net:/root/    #传输文件夹
```
上端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。 /home/lnmp0.4.tar.gz表示本地上准备上传文件的路径和文件名。root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4.tar.gz 表示保存在远程服务器上目录和文件名。

## java后台运行
```
nohup java abc &
```
## 各种控件属性

listview
android:fastScrollEnabled="true"


## myeclipse安装

1.在/opt目录下，修改myeclipse的权限  
```
sudo chmod 777 myeclipse-2015-stable-3.0-offline-installer-linux.run  
```
2.在当前路径下执行以下代码即可自动安装   
```
./myeclipse-2015-stable-3.0-offline-installer-linux.run  
```

## ubuntu下安装kde出错

下列软件包有未满足的依赖关系：
 kde-telepathy-minimal : 依赖: kde-config-telepathy-accounts (>= 15.04.0) 但是它将不会被安装

解决办法：先用 sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade 更新所有软件包。

如还有问题可以直接强制覆盖，但 telepathy 账户可能无法正常工作：
```
apt-get -o Dpkg::Options::="--force-overwrite" -f install
```

## 查看linux内核版本

```
cat /proc/version
```

## archlinux的一些特殊软件包

```
pacman -S archlinuxcn-keyring  #Arch Linux 中文社区仓库
pacman -S broadcom-wl-dkms  #broadcom网卡驱动
pacman -S linux-headers #编译内核头文件
pacman -S net-tools dnsutils inetutils iproute2 #网络相关工具(eg.ifconfig)
```

## python-scapy安装(pip无法安装时)
1、下载
从http://www.secdev.org/projects/scapy/下载release版本，这里我下载的是（executable zip）
2、安装
LINUX平台：
将下载的压缩文件进行解压，转到解压后的目录，然后运行安装。具体步骤如下：
```
$cd scapy-2.X (解压后的目录)
$sudo python setup.py install
```

## python抓取网页乱码问题
研究得知源网页为GBK（gb2312）编码，而python打印为utf8编码，所以需要做一下编码转换
html = resp.read()
html = unicode(html,'GBK').encode('UTF-8')
unicode函数即把GBK编码的网页转换为unicode，再用encode编码成UTF-8输出即可

## linux环境变量的配置

修改环境变量
# 法1（以java为例）：
```
vim ~/.bashrc
```
添加：

```
export JAVA_HOME=/usr/lib/jvm/java-7-sun
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
保存退出，输入以下命令使之立即生效：

```
source ~/.bashrc
```

为了将我们安装的JDK设置为默认JDK版本，还要进行如下工作。
执行代码:
```
  sudo update-alternatives --install /usr/bin/java java /home/ding/bin/jdk1.8.0_77/bin/java 300
  sudo update-alternatives --install /usr/bin/javac javac /home/ding/bin/jdk1.8.0_77/bin/javac 300
```

# 法2（以android为例）：

```
echo 'export ANDROID_HOME="/home/ding/bin/android-sdk-linux"' >> ~/.bashrc

echo 'export JAVA_CMD="/usr/lib/jvm/java-7-openjdk-amd64/bin/java"' >> ~/.bashrc
```

## 关于android的一些命令

列出设备：
```
adb devices
lsusb
```
检查是否安装了SDK版本包：
```
$ android list targets
```
查看AVD列表：
```
$ android list avd
```
创建一个Android项目：
```
$ android create project -n TestAndroidProj -t 'android-15' -p ~/android_proj -k com.example -a TestProjActivity

-n：项目名(TestAndroidProj);
-t：android SDK版本号(android-15);
-p：Android项目的路径;
-k：Java的包名;
-a：初始的Activity。
```
安装应用：
```
adb install -r <name>.apk
```
执行Shell命令：
```
adb shell <command>
```
模拟按键：
```
adb shell input keyevent <value>
```
截屏：
```
adb shell screencap <path>.png
```
录像：
```
adb shell screenrecord <path>.mp4
```

## mysql相关
```
//登陆
mysql> mysql -uusername -ppassword
//列出所有user(root用户下)
mysql> SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
//创建用户
//1.本地用户
mysql> create user 'username'@'localhost'identified by 'password';
//2.远程用户
create user 'username'@'%'identified by 'password';
mysql远程登陆时需要修改配置文件/etc/mysql/my.cnf
bind-address            = 127.0.0.1
bind-address            = 0.0.0.0
//给用户权限
mysql> grant all on database.* to "username"@"%";
//建数据库
mysql> create database rssdb;
//列出数据库
mysql> show database;
//使用数据库
mysql> use rssdb;
//建表
//插入
//列出
//删除
//查找
//导出
mysql -u username -p testdatabase > file.sql //导出数据库testdatabase的数据和表
mysql -u username -p testdatabase -d > file.sql //导出数据库testdatabase的表
//导入
use database;
source file.sql;
```

## openfire相关
```
# 下载安装openfire
wget -c http://download.igniterealtime.org/openfire/openfire_3_8_2.tar.gz
tar -xzvf openfire_3_8_2.tar.gz
mv openfire /usr/local/openfire
# 启动openfire
#/usr/local/openfire/bin/openfire
Usage: /usr/local/openfire/bin/openfire {start|stop|status}
/usr/local/openfire/bin/openfire start
# 配置openfire数据库
mysql>create database openfire;
mysql>source /usr/local/openfire/resources/database/openfire_mysql.sql
mysql> grant all on openfire.* to admin@"%" identified by 'admin'
```

## vscode改字体
文件->首选项->设置：  
在右侧的窗口里的打括号内添加一行：
```
"editor.fontFamily": "Hack（字体）",
```

## UEFI BIOS启动设置
### 1.bios启动
UEFI固件兼容bios启动：  
boot（引导/启动）->Lunch CSM（兼容支持模块）->Enabled  #兼容bios  
secure（安全）->secure boot control（安全引导）->Disabled
### 2.uefi启动
boot（引导/启动）->Lunch CSM（兼容支持模块）->Disabled  #不兼容bios  
secure（安全）->secure boot control（安全引导）->Enabled

## nasm简单使用
运行在操作系统上：

运行在裸机上：


## EDK2环境搭建（archlinux）
[原文](https://zhuanlan.zhihu.com/p/25941817)
依赖：
__gcc__
__make__
__g++__
__libcrossguid(ubuntu下包名叫uuid-dev)__
首先从EdkII官方github地址[tianocore/edk2](http://link.zhihu.com/?target=https%3A//github.com/tianocore/edk2)下载source tree。
```
make -C BaseTools
source ./edksetup.sh
```

```
nano Conf/target.txt
```
修改内容为：
ACTIVE_PLATFORM        = ShellPkg/ShellPkg.dsc
TARGET_ARCH            = IA32 X64
TOOL_CHAIN_TAG         = GCC48
```
build
```

## archlinux网易云音乐踩坑
安装
```
yaourt -S netease-cloud-music
```
启动不了
```
sudo nano /usr/share/applications/netease-cloud-music.desktop
```
修改：  
Exec=netease-cloud-music %U --no-sandbox  
白屏  
卸载noto字体

## 夏娜讲堂
__gcc__:c编译工具
```
#优化参数"O2","-o"后面跟生成的可执行文件
gcc -O2 hello.c -o hello-O2
#对c文件进行汇编而不链接,生成hello.o文件
gcc -c hello.c
#"-I"后跟include文件夹
#"-m32"表示按32位方式编译
#"-nostdinc"不包含C语言的标准库里的头文件(没有"stdio.h","string.h"......)
#"-fno-builtin"求gcc不主动使用自己的内建函数，除非显式声明。
#"-fno-stack-protector"不使用栈保护等检测
#"-ggdb和-gstabs+"添加相关的调试信息，调试对后期的排错很重要。
```

__md5sum__：md5校验和
```
#md5哈希校验和，用于查看两个文件是否相同
md5sum hello-O0 hello-O2
```

__file__:文件属性查看
```
#产看文件属性类型
file 文件
```

__hexdump__:16进制查看器
```
#以16进制方式显示文件，less是逐行显示,"-C"参数会翻译显示
hexdump hello-O2 | less
```

__as__：汇编
```
#汇编
as hello.s
```

__nasm__:汇编
```
#"-f"后跟文件格式（elf,ef64）
#"-g"生成调试信息
eg:
nasm -f elf -g hello.s
```

__ld__:链接工具
```
#"-T"后面跟链接器脚本
#"-m"后是我们要生成的文件格式（elf64,elf_i386）
#"-nostdlib"不链接C语言的标准库
#"-o"生成二进制文件
eg:
ld -T scripts/kernel.ld -m elf_i386 -nostdlib hello.o hello1.o hello2.o -o hello
```
链接脚本的书写（）

__objdump__：反汇编
```
#反汇编
objdump -d/-S hello-O2
#查看符号表
objdump -sysms hello-O2
```

__objcopy__:二进制文件操作
```
#
objcopy --sym hello-O2
#将文件转化位纯二进制文件（去掉elf_section头...）
objcopy -O binary bootsect.o
```

__makefile__
```
目标文件：生成目标文件所需的文件
      -@命令
假目标：目标文件
.PHONY=假目标
```
“-”表示错误继续执行，“@”表示不生成调试信息

## linux下命令行走ss代理
__下载proxychain__
```
sudo pacman -S proxychain-ng
```
__配置文件__(默认是/etc/proxychains.conf)
```
strict_chain
proxy_dns 
remote_dns_subnet 224
tcp_read_time_out 15000
tcp_connect_time_out 8000
localnet 127.0.0.0/255.0.0.0
quiet_mode

[ProxyList]
socks5  127.0.0.1 1080
```
__启动__（在命令前加上proxychain4即可）

```
proxychain4 curl https://www.google.com
proxychain4 ./setup.sh
```

## linux查看wifi密码
所有的wifi信息都在/etc/NetworkManager/system-connections/目录下，root打开看看就明白了。


## virtualbox虚拟机主机互传文件
环境：
虚拟机：win10
宿主机：debian9
---
1.安装增强功能
设备----安装增强功能----点击右下角跳出的东东----一路next安装。（重启）
2.共享文件夹
先在宿主机创建一个文件夹“static”，
设置----共享文件夹----将“static”添加固定分配----勾选自动挂载和固定分配。

## 以默认root权限启动应用
以网易云音乐为例
# sudo visudo
添加下面这行
dy ALL=ALLALL = NOPASSWD: /usr/bin/netease-cloud-music
然后修好applications-desktop文件
# sudo vim /usr/share/applications/netease-cloud-music.desktop
修改Exec=netease-cloud-music %U这行为Exec=sudo netease-cloud-music %U


### 端口映射（frp）
```
wget https://github.com/fatedier/frp/releases/download/v0.16.0/frp_0.16.0_linux_amd64.tar.gz
tar -zxvf frp_0.16.0_linux_amd64.tar.gz
frp_0.16.0_linux_amd64
ls
# 如下
# LICENSE  frpc  frpc.ini  frpc_full.ini	frps  frps.ini	frps_full.ini
```
#### 服务端
修改frps.ini
```
[common]
bind_port = 9000
vhost_http_port = 80
auth_token = frp
```
启动
```
./frps -c frps.ini
```
#### 客户端
修改frpc.ini
```
[common]
server_addr = 23.106.133.142
server_port = 9000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

[www]
type = http
custom_domains = 66.dingdinghaha.com.cn
local_port = 8000
auth_token = frp
```
启动
```
./frpc -c frpc.ini
```
