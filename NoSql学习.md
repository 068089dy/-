### Redis

设置键值对
```
127.0.0.1:6379> set foo qwe
OK
127.0.0.1:6379> get foo
"qwe"
```
订阅和发布
```
redis-cli
# 订阅redischat频道
127.0.0.1:6379> SUBSCRIBE redischat
# 重新打开一个终端，在redischat频道发布hello消息
127.0.0.1:6379> PUBLISH redischat "hello"
```
