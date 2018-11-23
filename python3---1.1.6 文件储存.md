# 文件储存


## TXT 文本存储

```
import requests
from pyquery import PyQuery as pq

url = 'https://www.zhihu.com/explore'
headers = {
  'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.162 Safari/537.36'
}
html = requests.get(url,headers=headers).text
doc = pq(html)
items = doc('.explore-tab .feed-item').items()
for item in items:
  question = item.find('h2').text()
  author = item.find('.author-link-line').text()
  file = open('explore.txt','a',encoding='utf-8')
  file.write('\n'.join([question,author]))
  file.write('\n' + '=' * 50 + '\n')
  file.close()
```


## JSON 文件存储

- 读取 json
  - loads(): 将 json 文本字符串转为 json 对象，
  - dumps(): 将 json 对象转为文本字符串
```
import json

str = '''
[
  {
    "name": "jonh",
    "sex": "male"
  }
]
'''

data = json.loads(str)
print(data)
print(type(data))

### 获取属性
data[0]['name']
data[0].get('name') # 推荐使用，如果键名不存在，不会报错，返回 None，另外，get()还可以传入第二个参数（即默认值）
```

-  从json 文本读取内容
```
with open('data.json','r') as file:
  str = file.read()
  data = json.loads(str)
  print(data)
```

- 输出 json
```
with open('data.json','w') as file:
  file.write(json.dumps(str))
```


## csv文件存储
- 写入
  - 可以传入 delimiter 分隔符，如： `csv.writer(csvfile,delimiter=' ')`
```
import csv

with open('data.csv','w') as csvfile:
  writer = csv.writer(csvfile)
  writer.writerow(['id','name','age'])
  writer.writerow(['1001','mike',12])
```
- 字典写入
```
import csv

with open('data.csv','w') as csvfile:
  filenames = ['id','name','age']
  writer = csv.DictWriter(csvfile,filenames=filenames)
  writer.writeheader()
  writer.writerow(['id','name','age'])
  writer.writerow(['1001','mike',12])
```

- 读取 csv
```
## 读取

with open('data.csv','r',encoding='utf-8') as csvfile:
  reader = csv.reader(csvfile)
  for row in reader:
    print(row)
```



## 关系数据库存储--- mysql（已安装pymysql）
- 连接数据库

```
import pymysql

db = pymysql.connect(host='localhost',user='root',password='root',port=3306)
cursor = db.cursor()
cursor.execute('select version()')
data = cursor.fetchone()
print('database version:',data)
cursor.execute('create database spiders default character set utf8')
db.close()
```
- 创建表
```
sql = 'create table if not exists students(id varchar(255) not null,name varchar(255) not null,age int not null,primary key (id))'
cursor.execute(sql)
db.close()
```

- 插入数据

>更改操作必须为一个事务，事务的 4 个属性： ACID(原子性，一致性，隔离性，持久性)  
原子性：比如插入一条数据，要么全部插入，要么都不插入


```

sql = 'insert into students(id,user,age) values(%s, %s ,%s)'

插入、更新、删除这些操作的标准写法：
try:
  cursor.execute(sql)
  db.commit()
except:
  db.rollback()
db.close()
==================

id="20181118"
name="fairy"
age= 27
sql = 'insert into students(id,name,age) values(%s, %s ,%s)'

try:
  cursor.execute(sql,(id, name, age))
  db.commit()
except:
  db.rollback()
db.close()


=====================
如果突然增加字段，上面的写法就不方便了，所以改写一个动态变化的字典


data = {
  'id': '20181119', # id 不能添加重复，不然会插入 fail
  'name': 'fairy123',
  'age': 27
}

table = 'students'
keys = ', '.join(data.keys())
values = ', '.join(['%s'] * len(data))

sql = 'insert into {table}({keys}) values ({values})'.format(table=table, keys=keys, values=values)

try:
  if cursor.execute(sql,tuple(data.values())):
    print('ok')
    db.commit()
except:
  print('fail')
  db.rollback()
db.close()


```

- 更新数据

```
# sql = 'update students set age = %s where name = %s'

# try:
#   if cursor.execute(sql,(25,'fairy')):
#     print('ok')
#     db.commit()
# except:
#   print('fail')
#   db.rollback()
# db.close()




# 重复数据去重

data = {
  'id': '20181119',
  'name': 'fairy123',
  'age': 2999
}

table = 'students'
keys = ', '.join(data.keys())
values = ', '.join(['%s'] * len(data))

sql = 'insert into {table}({keys}) values ({values}) on duplicate key update'.format(table=table, keys=keys, values=values)

update = ', '.join([" {key} = %s".format(key=key) for key in data])

sql += update

try:
  if cursor.execute(sql,tuple(data.values())*2):
    print('ok')
    db.commit()
except:
  print('fail')
  db.rollback()
db.close()
```

- 删除数据

```
# 删除数据

table = 'students'
condition = 'age > 27'

sql = 'delete from {table} where {condition}'.format(table=table,condition=condition)

try:
  cursor.execute(sql)
  print('ok')
  db.commit()
except:
  print('fail')
  db.rollback()
db.close()

```

- 查询数据

```
# 查询数据

sql = 'select * from students where age > 20'

try:
  cursor.execute(sql)
  print('ok',cursor.rowcount)

  one = cursor.fetchone()
  print('one',one)

  all = cursor.fetchall()
  print('all',all,type(all))
  # db.commit()  # 不需要 commit() 方法
except:
  print('fail')
  db.rollback()
db.close()

```


## 非关系数据库存储--- MongoDB(已安装pymongo)

非关系数据库细分：
- 键值存储数据库： redis， Voldmort oracle BDB
- 列存储数据库： Cassandra,HBase, Riak
- 文档型数据库：代表有 CouchDB, MongoDB
- 图形数据库：Neo4j,InfoGrid, Infinite Graph


### 1. 插入

```
# mongodb 数据库存储
# 
# 链接数据库

import pymongo

client = pymongo.MongoClient(host='localhost',port=27017)
# client = MongoClient('mongodb://localhost:27017/')

# 指定数据库
db = client.test
# db= client['test']

# 指定集合
collection = db.students
# collection = db['students']

# 插入数据
student = {
  'id': '2018111801',
  'name': 'johns',
  'age': 23,
  'gender': 'male'
}

# insert is deprecated. Use insert_one or insert_many instead.
result = collection.insert_one(student)
print(result)

```


### 2.查询

```

## 查询

# result = collection.find_one({'name':'john'})
result = collection.find({'name':'john'})

# 年龄大于 20 
result = collection.find({'age':{'$gt':20}})

# 正则匹配,查询名字以 M 开头的数据
result = collection.find({'name':{'$regex': '^M.*'}})

# 计数
result = collection.find({'name':'john'}).count()

# 排序 sort
result = collection.find().sort('name',pymongo.ASCENDING)

# 偏移  skip() 偏移几个位置
result = collection.find().sort('name',pymongo.ASCENDING).skip(2)

# 还可以用 limit() 指定要取的结果个数
result = collection.find().sort('name',pymongo.ASCENDING).skip(2).limit(2)

print(type(result))
print(result)

```


### 3.更新
```
# 更新

condition = {'name': 'johns'}
student = collection.find_one(condition)

student['age'] = 42

# Use replace_one, update_one or update_many instead.
# result = collection.update_one(condition,student)

# 使用 $set 更新
result = collection.update_one(condition,{'$set': student})
print(result)

```



### 4.删除
```
# 删除
#  remove is deprecated. Use delete_one or delete_many instead.
result = collection.remove({'name': 'john'})
print(result)
```



## redis 存储(已安装redis 和 redis-py)

### 查看密码

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


### 遇到的问题

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


### 使用 ConnectionPool 连接
```

# 使用 ConnectionPool 连接
# from redis import StrictRedis, ConnectionPool

# pool = ConnectionPool(host='localhost', port=6379, db=0, password='')
# redis = StrictRedis(connection_pool=pool)
```



## RedisDump

- redis-dump: 用于导出数据
- redis-load: 用于导入数据

```
redis-dump -u localhost:6379
```


### 参考
- [Redis-study](https://github.com/fairyly/Redis-study)
- [Redis 教程](http://www.runoob.com/redis/redis-tutorial.html)



## 参考
- [【python3 网络爬虫开发实战】]()
