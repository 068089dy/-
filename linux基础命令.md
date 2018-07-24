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
扩展分区
```
fdisk /dev/sdX
  p #列出
  d #删除（分区号）
  n #新建（选分区号，和开始结束位）
umount ／dev/sdXx
e2fsck -f /dev/sdb2 #检查分区信息
resize2fs -p /dev/sdb2  #调整分区大小

```
### 主机信息
### 常用操作
1.环境变量
export
env
whereis
who
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
