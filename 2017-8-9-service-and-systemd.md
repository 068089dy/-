---
## 自建服务
---
### 1.service(以uwsgi为例)
创建脚本
```
sudo vim /etc/init.d/uwsgi
```
写入
```
#!/bin/bash
# chkconfig: - 85 15  #脚本开头要写上 # chkconfig: - 85 15 ，不然无法添加为系统服务
uwsgi=/usr/local/python3/bin/uwsgi  #uwsgi的可执行文件
uwsgi_conf=/home/dy/my_uwsgi.ini

case $1 in
    start)
        echo -n "Starting uWsgi"
        nohup $uwsgi $uwsgi_conf &
        echo " done"
    ;;

    stop)
        echo -n "Stopping uWsgi"
        killall -9 uwsgi
        echo " done"
    ;;

    restart)
        $0 stop
        $0 start
    ;;

    show)
        ps -ef|grep uwsgi|grep -v grep
    ;;

    *)
        echo -n "Usage: $0 {start|restart|stop|show}"
    ;;

esac
```
添加可执行属性
```
sudo chmod +x /etc/init.d/uwsgi
```
添加为系统服务
```
sudo chkconfig --add uwsgi
sudo chkconfig uwsgi on
```
启动 uWSGI 服务
```
sudo service uwsgi start
```

### 2.systemd
新建一个rc-local.service
```
nano /usr/lib/systemd/system/rc-local.service     #：软件包安装的单元
```
或
```
nano /etc/systemd/system/rc-local.service     #：系统管理员安装的单元
```
一般用软件包安装的单元,系统管理员安装的单元一般都是网络，桌面显示这些。
写入以下内容
```
[Unit]
Description=/etc/rc.local Compatibility
ConditionPathExists=/etc/rc.local
[Service]
Type=forking
ExecStart=/etc/rc.local
TimeoutSec=0
StandardOutput=tty
RemainAfterExit=yes
SysVStartPriority=99
[Install]
WantedBy=multi-user.target
```
```
systemctl enable rc-local.service
```
然后:
```
nano /etc/rc.local
```
写入:
```
#!/bin/bash
sslocal -c ss.conf
```
权限设置
```
sudo chmod +x /etc/rc.local
```
