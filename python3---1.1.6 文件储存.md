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
```
import csv

with open('data.csv','w') as csvfile:
  writer = csv.writer(csvfile)
  writer.writerow(['id','name','age'])
  writer.writerow(['1001','mike',12])
```

## 参考
- 
