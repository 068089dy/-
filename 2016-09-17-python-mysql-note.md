---
layout: post
title:  "python-mysql笔记"
category: "python"
keywords: ["python", "mysql"]
description: "python mysql"
tags: ["python", "mysql"]
---
## 查看版本1

```
import MySQLdb as mdb
import sys

con = None

try:

    con = mdb.connect('localhost', 'user','password', 'testdb');
    #一旦连接成为，我们将会得到一个cursor（游标）对象。这个cursor对象用来遍历结果集中的记录。
    cur = con.cursor()
    #我们通过调用该curor对象的execute()方法来执行SQL语句。
    cur.execute("SELECT VERSION()")
    #我们开始获取数据，由于我们只取回一个记录，因此我们调用fetchone()方法。
    data = cur.fetchone()

    print "Database version : %s " % data

except mdb.Error, e:

    print "Error %d: %s" % (e.args[0],e.args[1])
    sys.exit(1)

finally:

    if con:
        con.close()
```

## 查看版本2

```
import _mysql
import sys

con = None

try:

  con = _mysql.connect('localhost', 'user',
      'password', 'testdb')

  con.query("SELECT VERSION()")
  result = con.use_result()

  print "MySQL version: %s" % \
    esult.fetch_row()[0]

except _mysql.Error, e:

  print "Error %d: %s" % (e.args[0], e.args[1])
    sys.exit(1)

finally:

  if con:
    con.close()
```


## 插入数据


```
import MySQLdb as mdb
import sys

con = mdb.connect('localhost','user','password','testdb');

with con:

    cur = con.cursor()
    cur.execute("CREATE TABLE IF NOT EXISTS \
          Writers(Id INT PRIMARY KEY AUTO_INCREMENT, Name VARCHAR(25))")
    cur.execute("INSERT INTO Writers(Name) VALUES('Jack London')")
    cur.execute("INSERT INTO Writers(Name) VALUES('Honore de Balzac')")
    cur.execute("INSERT INTO Writers(Name) VALUES('Lion Feuchtwanger')")
    cur.execute("INSERT INTO Writers(Name) VALUES('Emile Zola')")
    cur.execute("INSERT INTO Writers(Name) VALUES('Truman Capote')")

```


## 遍历数据

```
import MySQLdb as mdb
import sys

con = mdb.connect('localhost', 'user', 'password', 'testdb');

with con:

    cur = con.cursor()
    cur.execute("SELECT name FROM Writers")

    numrows = int(cur.rowcount)
    for i in range(numrows):
        row = cur.fetchone()
        print row
```


## 删除数据


```
import MySQLdb as mdb
import sys

try:
    conn = mdb.connect('localhost', 'user',
        'password', 'testdb');

    cursor = conn.cursor()

    cursor.execute("DELETE FROM Writers WHERE Name = 'Jack London'")
    cursor.execute("DELETE FROM Writers WHERE Id = 4")
    cursor.execute("DELETE FROM Writers WHERE Id = 3")

    conn.commit()

except mdb.Error, e:

    conn.rollback()
    print "Error %d: %s" % (e.args[0],e.args[1])
    sys.exit(1)

cursor.close()
conn.close()
```
