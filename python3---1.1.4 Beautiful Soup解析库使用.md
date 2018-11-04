# python3---1.1.4 Beautiful Soup解析库使用

## Beautiful Soup
>借助网页结构和属性等特性来解析网页,我们不用再去写复杂的正则表达式，只需要简单的几天语句，就可以完成某个元素的提取

```
from bs4 import BeautifulSoup

# soup = BeautifulSoup('<p>hello</p>','lxml')
# print(soup.p.string)


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

soup = BeautifulSoup(html,'lxml')
print(soup.prettify())
print("输出表签<title>及内部标签或内容:",soup.title)
print(soup.title.string)
print( "输出表签<body>及内部标签或内容:",soup.body)
print(soup.ul)
```



## 方法选择器

- find_all()
- find()


## css 选择器
- select()


## 参考
- 
