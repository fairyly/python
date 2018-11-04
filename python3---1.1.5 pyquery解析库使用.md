# pyquery解析库使用

## 初始化
```
from pyquery import PyQuery as pq


html = """
<html>
<head>
  <title>test</title>
  <body>
    <div>
      <ul>
        <li>first</li>
        <li>second</li>
      </ul>
    </div>
  </body>
</head>
</html>

"""

#### 字符串初始化

# doc = pq(html)
# print(doc('li'))


### URL初始化

# doc = pq(url='https://cuiqingcai.com') #首先会请求 url,然后得到 HTML 内容完成初始化
# print(doc('title'))

# h和下面代码功能相同

# from pyquery import PyQuery as pq
# import requests

# doc = pq(requests.get('https://cuiqingcai.com').text)
# print(doc('title'))


### 文件初始化
# doc = pq(filename='deml.html')
# print(doc('li'))



```


## css 选择器

```
from pyquery import PyQuery as pq


html = """
<html>
<head>
  <title>test</title>
  <body>
    <div class="contain">
      <ul class="list>
        <li>first</li>
        <li>second</li>
      </ul>
    </div>
  </body>
</head>
</html>

"""

## css 选择器
doc = pq(html)
print(doc('.contain .list li'))
print(type(doc('.contain .list li')))
```

## 查找节点

查询函数和 jquery中函数相同

- find()
- children()
- parents()
- siblings()


## 遍历

```
list = doc('li').items
for li in list:
  print(li,type(li))

```
## 获取信息

- 获取属性
  - attr()
  ```
    a = doc('.item a')
    print(a.attr('href'))
  ```

- 获取文本
  - text(): 只返回纯文字内容，多个节点，返回所有节点内部纯文本
  ```
    a = doc('.item a')
    print(a.text())
  ```
  - HTML(): 返回内部节点所有的 html 文本,多个节点返回第一个节点内部的 html 文本，
  ```
    a = doc('.item a')
    print(a.html())
  ```

- 节点操作
  - addClass() 和 removeClass()
  - attr()
  - html()
  - text()
  - remove()
  - append()
  - empty()
  - prepend()

- 伪类选择器
  ```
  doc('li:nth-child(2)')
  doc('li:last-child')
  doc('li:first-child')
  ```

## 参考
- [GitHub - pyquery](https://github.com/gawel/pyquery)
