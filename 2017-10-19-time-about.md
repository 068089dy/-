---
---
### c语言里的时间
```
#include<stdio.h>
#include<time.h>

int main(){
  int seconds = time((time_t*)NULL);
  printf("%d\n", seconds);
  return 0;
}
```
```
[dy@dy_arch c]$ gcc demo.c
[dy@dy_arch c]$ ./a.out
1508320486
```
这个数字表示从1970年一月一日0时到现在过了多少秒

### python里的时间
```
>>> import time
>>> time.time()
1508317372.64079
```
和上面一样，这个数字也是表示从1970年一月一日0时到现在过了多少秒

### SQL中的时间
|   类型    |大小   |                    范围                  |  
|----------|-------|-----------------------------------------|  
|datetime  | 8bytes|1000-01-01 00:00:00 ~ 9999-12-31 23:59:59|  
|timestamp | 4bytes|1970-01-01 00:00:01 ~ 2038               |  
|date      | 3bytes| 1000-01-01 ~ 9999-12-31                 |  
|year      | 1bytes| 1901 ~ 2155                             |  
```
MariaDB [(none)]> create database testbase;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> use testbase;
Database changed
MariaDB [testbase]> create table testtable( testdate datetime, testtime timestamp );
Query OK, 0 rows affected (0.31 sec)

MariaDB [testbase]> insert into testtable(testdate,testtime)
    -> values('2017-10-19 19:13:10','2017-10-19 19:13:10');
Query OK, 1 row affected (0.07 sec)

MariaDB [testbase]> select * from testtable;
+---------------------+---------------------+
| testdate            | testtime            |
+---------------------+---------------------+
| 2017-10-19 19:13:10 | 2017-10-19 19:13:10 |
+---------------------+---------------------+
1 row in set (0.01 sec)
```
