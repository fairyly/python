# redis

## 查看密码

- client 中输入
```
127.0.0.1:6379> CONFIG get requirepass
1) "requirepass"
2) ""

默认情况下 requirepass 参数是空的，这就意味着你无需通过密码验证就可以连接到 redis 服务。
```
## 设置密码

```
你可以通过以下命令来修改该参数：

127.0.0.1:6379> CONFIG set requirepass "yourpassword"
```


- 设置密码后，客户端连接 redis 服务就需要密码验证，否则无法执行命令。


AUTH 命令基本语法格式如下：
```
127.0.0.1:6379> AUTH password

如：

127.0.0.1:6379> AUTH "yourpassword"
OK
```


## 遇到的问题

>创建了一个 `redis.py` 文件,
连接 redis 
```
# redis 储存

from redis import StrictRedis

# 连接 redis
redis = StrictRedis()

redis.set('name','test')
print(redis.get('name'))


运行一直提示 `ImportError: cannot import name 'StrictRedis' from 'redis'`

后来查到 文件名不能克引入库名相同，就改了文件名就可以了
```
- [error-module-object-has-no-attribute](https://stackoverflow.com/questions/15649222/redis-py-attributeerror-module-object-has-no-attribute)


## 使用 ConnectionPool 连接
```

# 使用 ConnectionPool 连接
# from redis import StrictRedis, ConnectionPool

# pool = ConnectionPool(host='localhost', port=6379, db=0, password='')
# redis = StrictRedis(connection_pool=pool)
```



## 参考
- [Redis-study](https://github.com/fairyly/Redis-study)
- [Redis 教程](http://www.runoob.com/redis/redis-tutorial.html)