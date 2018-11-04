# pyquery解析库使用


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

# doc = pq(html)
# print(doc('li'))

doc = pq(url='https://cuiqingcai.com') #首先会请求 url,然后得到 HTML 内容完成初始化
print(doc('title'))

# h和下面代码功能相同

# from pyquery import PyQuery as pq
# import requests

# doc = pq(requests.get('https://cuiqingcai.com').text)
# print(doc('title'))
```



## 参考
- [GitHub - pyquery](https://github.com/gawel/pyquery)
