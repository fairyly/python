# python3---1.1.3 XPath解析库使用

用正则提取页面的信息还是比较繁琐，不方便，




## XPath : 信息提取

和 lxml 库配合使用


### 常用规则：
```
//  表示从html整个文档全局查找

/   表示从根节点选取

.   表示从当前节点选取

..  表示从上层节点选取

@   表示选取属性
```


```
from lxml import etree

text = '''
<div>
  <ul>
    <li>first</li>
    <li>second</li>
  </ul>
</div>
'''
html = etree.HTML(text)
# res = etree.tostring(html)
# print(res.decode('utf-8'))
# html = etree.parse(text,etree.HTMLParser())
result = html.xpath('//li')
print(result)
```


## 参考
- 
