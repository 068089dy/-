### 1.忘记密码？
### 2.忘记root密码？
__注：此操作进行时任何人都能免密登录数据库，操作前做好安全措施。__
* 修改配置文件
在[mysqld]的段中加上一句：`skip-grant-tables`
* 重启mysql
```
/etc/init.d/mysqld restart  ( service mysqld restart )
```
* 免密登录并修改密码
```
mysql -uroot
mysql> USE mysql ;
mysql> UPDATE user SET Password = password ( 'new-password' ) WHERE User = 'root' ;
mysql> flush privileges ;
mysql> quit
```
* 再重启mysql
```
/etc/init.d/mysqld restart  ( service mysqld restart )
```
### 3.centos7安装mariadb
https://www.jianshu.com/p/61e9cbd1b675
### 4.查看所有用户
```
select user host, password from mysql.user;  #root下
```
### 5.配置文件路径查看
```
whereis mysql
```
### 6.查看数据文件存储路径
```
show global variables like "%datadir%";
```
### 7.在docker中启动mysql失败时查看日志
```
docker logs 容器 
```
### 8.
