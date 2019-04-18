### 系统信息
```
# linux内核版本
more /proc/version
# 发行版
more /etc/issue

uname -r
```
### 硬件信息
cpu
```
more /proc/cpuinfo | grep "model name"
```
查看CPU位数
```
getconf LONG_BIT
```
内存和交换空间
```
free -h
```
usb设备
```
lsusb
```
pci设备
```
lspci
```
### 磁盘信息
```
fdisk -l
# 所有分区空间
df -h ./
# 当前目录空间
du -h
```
扩展分区
```
fdisk /dev/sdX
  p #列出
  d #删除（分区号）#删除要扩展的分区，这里删除并不会删除分区上的数据
  n #新建（选分区号，和开始结束位）
umount /dev/sdXx
e2fsck -f /dev/sdb2 #检查分区信息
resize2fs -p /dev/sdb2  #调整分区大小

```
### 主机信息
### 常用操作
1.环境变量
```
export
env
whereis
who
```
2.软链接，硬链接(ln)
软链接就是个快捷方式，硬链接可认为是一个文件拥有两个文件名。
ln
```
-s：软链接，不带参数默认是硬链接
-f：强制链接（如果所有重名文件强制删除）
-v：显示信息
```
3.删除
```
rm -rf *.py
  -i：删除时提示
  -f：强制删除，没有提示
  -r：删除文件夹和文件
  -v：显示步骤
```
### 软件管理
apt
```
apt update  //更新软件源
apt upgrade //更新软件包
apt install //
apt remove  //
apt autoremove //自动删除没用的
apt install -f //解决依赖
```
pacman
```
pacman -S 下载安装
pacman -R 删除
pacman -Ss 搜索
pacman -Qs #搜索本地安装包
```
yay，yaourt 同上

yum
```
yum install package //安装package
yum update  //全局更新
yum update package  //更新package
yum upgrade //升级
yum upgrade package //升级package
yum info package  //信息
yum remove package  //删除
yum 
```
dpkg
```
dpkg -i #安装
```
rpm
### 编辑，查看，写入
vim
```
normal模式：刚进入vim，就是normal模式

删当前光标所在的一个字符：x
保存：:w
退出：:q
强制退出：:q!
剪切当前行：dd
粘贴：p

insert模式：normal下按i进入insert模式，再按esc退回到normal模式。
visual模式：normal下按v进入visual模式，可以选择文本
```

nano
```
复制一行：alt+6
剪切一行：ctrl+k
粘贴：ctrl+u

部分复制：先将光标移到要复制部分的开头，然后按住alt+a，开始选中，然后alt+6/ctrl+k

上一页：ctrl+y
下一页：ctrl+v

搜索：Ctrl+w，然后输入要搜索的单词，如果要跳到下一个，则继续按ctrl+w，然后直接回车。

保存：ctrl+o
退出：ctrl+x
```
gedit
cat
echo
### 进程及端口管理
ps
#### netstat
查看占用端口的进程号
```shell
➜  ~ netstat -anp | grep 9999
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 127.0.0.1:9999          0.0.0.0:*               LISTEN      7918/python         
➜  ~ lsof -i:9999
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
python  7918 ding    3u  IPv4 117917      0t0  TCP localhost:distinct (LISTEN)
```
kill
```shell

```

### 压缩解压
tar
```
-c 打包
-x 解包
-t 列出文件
-v 显示过程
-f 指定打包后的文件名
tar -cvf 打包文件名 源文件    # 打包
tar -tvf 文件     #列出归档
tar -zxvf test.tar.gz   #解压
```
zip
```
zip 压缩文件名 源文件   #压缩文件
zip -r 压缩文件名 源文件    #压缩目录
unzip 文件名   #解压
```
7z
### 查找
find
```
# 按名称
find ./ -name 文件名正则
-i：忽略大小写
# 按所有者
find /root -user root
find /root nouser
# 按时间
find hello.txt -mtime +5
mtime：修改文件内容
ctime：改变文件属性
atime：文件访问时间
-5：五天内
+5：五天前
5：5天前当天修改的
# 按大小搜索
find . size 100k
-8k：小于8k
+8M：大于8M
6k：等于6k
```
grep
sed
awk
```
awk '{print $2}'  #匹配第二列
```
whereis

### 网络
ip
ifconfig
iptables
tcpdump
```

```
### 多用户，组及权限
useradd
usermod
userdel
passwd
### 文件管理及权限
chown
chmod
w，r，x三个字母分别表示读，写，执行权限；同时又可以用4，2，1代替。
EG：
```
ll file //查看file权限
-rw-rw-rw- 1 dy dy 0 Dec  7 10:28 file
```
给file增加可执行权限
```
chmod +x file //给file增加可执行权限
```
文件可执行
```
-rwxrwxrwx 1 dy dy 0 Dec  7 10:28 file* //文件可执行
```
改变用户权限为421
```
chmod 421 file  //改变用户权限为421
ll file
-r---w---x 1 dy dy 0 Dec  7 10:28 file*
```
rm
mv
cp
### 远程管理
ssh
scp
ftp
### 服务
systemd
```
systemctl start test.service
systemctl stop test.service
systemctl restart test.service
systemctl cat test.service
systemctl list-unit-files  查看开机启动项 
systemctl enable test #开机启动
systemctl list-units --type=service #查看所有已启动服务
```
 journalctl 
```
journalctl --list-boots #列出引导记录
journalctl -b -1 #显示上次引导记录日志
journalctl --since "2015-01-10" --until "2015-01-11 03:00"    #限定日期查看
  --since的使用：
    1.可以加 today：journalctl -u nginx.service --since today 检查今天某项服务的运行状态：
    2.journalctl --since 09:00 --until "1 hour ago" 获得早9：00到一小时前这段时间内的报告
journalctl -u v2ray #查看指定服务日志

```

service
### 日志
内核日志 dmesg
