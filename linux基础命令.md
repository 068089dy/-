### 系统信息
```
# 内核版本
more /proc/version
# 系统版本
more /etc/issue
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
内存
```
free
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
df
```
### 主机信息
### 常用操作
1.环境变量
export
env
whereis
who
2.软链接，硬链接(ln)
### 软件管理
apt
pacman
yaourt
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
rpm
### 编辑
vi
vim
nano
gedit
### 压缩解压
### 查找
find
grep
whereis
### 网络
ip
ifconfig
iptables
### 多用户，组及权限
useradd
usermod
userdel
passwd
### 文件管理及权限
chown
chmod
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
```
service
