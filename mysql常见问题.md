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
