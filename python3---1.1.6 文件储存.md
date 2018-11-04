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

```
sql = 'insert into students(id,user,age) values(%s, %s ,%s)'

try:
  cursor.execute(sql)
  db.commit()
except:
  db.rollback()
db.close()
```

- 更新数据

- 删除数据

- 查询数据



## 非关系数据库存储--- MongoDB(已安装pymongo)

非关系数据库细分：
- 键值存储数据库： redis， Voldmort oracle BDB
- 列存储数据库： Cassandra,HBase, Riak
- 文档型数据库：代表有 CouchDB, MongoDB
- 图形数据库：Neo4j,InfoGrid, Infinite Graph



## redis 存储(已安装redis 和 redis-py)



## 参考
- 
