#### 下载镜像
到[这里](http://manjaro-arm.org/downloads.php#rpi2)下载好镜像（树梅派2和3是同一个镜像）
#### 解压并写入到内存卡中
```
sudo dd if=Manjaro-ARM-minimal-rpi2-18.06.img of=/dev/sdb
```
#### ssh登录
如果没有显示器，就比较麻烦了，要先用ssh登录
1.先用一根网线把树梅派和电脑连起来
2.电脑上安装dnsmasq（因为电脑要充当网关为树梅派分配ip）
```
sudo pacman -S dnsmasq
```
3.编辑dnsmasq的配置文件/etc/dnsmasq.conf
```
interface = enp2s0                                    #网口
bind-interfaces
dhcp-range=192.168.1.1,192.168.1.3,255.255.255.0,24h  #dhcp分配ip地址范围192.168.1.1-3
dhcp-option=option:router,192.168.1.1                 #路由
```
4.重启dnsmasq服务
```
sudo systemctl restart dnsmasq
```
5.为网口设置ip
```
sudo ip link set up dev enp2s0
sudo ip addr add 192.168.1.1/24 broadcast 192.168.1.255 dev enp2s0 
```
6.查看网口邻居
```
ip neigh show dev enp2s0
# 192.168.1.2  INCOMPLETE
```
树梅派的ip就是192.168.1.2
7.ssh登录
```
```
