### 1.mongodb
#### 1.安装
```
apt install mongodb
apt install bash-completion
```
#### 2.启动
mongodb的默认配置文件是/etc/mongodb.conf。

启动mongo服务
```
mkdir -p /data/db #这个是mongodb默认的数据库文件目录，
mongod --port 9999  #在9999端口启动mongodb，不使用这个参数的话默认是
  --dpath:设置数据库文件目录
  -f /etc/mongodb.conf:设置配置文件
  
root@4e8910d56ce2:/# mongod
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] MongoDB starting : pid=29 port=27017 dbpath=/data/db 64-bit host=4e8910d56ce2
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] db version v3.2.11
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] git version: 009580ad490190ba33d1c6253ebd8d91808923e4
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2l  25 May 2017
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] allocator: tcmalloc
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] modules: none
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] build environment:
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten]     distarch: x86_64
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten]     target_arch: x86_64
2018-10-14T02:54:45.900+0000 I CONTROL  [initandlisten] options: {}
2018-10-14T02:54:45.908+0000 I -        [initandlisten] Detected data files in /data/db created by the 'wiredTiger' storage engine, so setting the active storage engine to 'wiredTiger'.
2018-10-14T02:54:45.908+0000 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=3G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2018-10-14T02:54:46.232+0000 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2018-10-14T02:54:46.232+0000 I CONTROL  [initandlisten] 
2018-10-14T02:54:46.233+0000 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/db/diagnostic.data'
2018-10-14T02:54:46.233+0000 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2018-10-14T02:54:46.234+0000 I NETWORK  [initandlisten] waiting for connections on port 27017
```

客户端shell连接
```
root@4e8910d56ce2:/# mongo 
MongoDB shell version: 3.2.11
connecting to: test
Server has startup warnings: 
2018-10-14T02:54:46.232+0000 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2018-10-14T02:54:46.232+0000 I CONTROL  [initandlisten] 
> 
```

#### 3.使用
##### 1.连接
在mongo命令打开的shell下输入
```
mongodb://[username:password@]server-ip:port[database]

```
##### database：数据库
```
# 创建/选中(database必须在插入数据之后才能真正被创建)
> use test_database
# 列出所有database
> show dbs
# 查看当前db
> db
```
##### collections(tables)：集合
```
# 创建(先选中database)
> db.createCollection("col")
```
##### documents：文档(相当于json数据)
```
# 插入
> db.col.insert(json数据)
```




