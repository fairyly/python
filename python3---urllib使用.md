# python3---urllib使用

自带的 urllib 库，不用再安装，包含四个模块
- request: 请求模块，用来模拟发送请求，
- error: 异常处理模块，
- parse: 工具模块，提供许多 url 处理方法
- robotparser： 识别网站的 robot.txt 网站，判断哪些网站可以爬，其实用的少

## 1.发送请求

- urllib.request.urlopen(url, data=None, [timeout, ]*, cafile=None, capath=None, cadefault=False, context=None)
  - url:  需要打开的网址

  - data：Post提交的数据

  - timeout：设置网站的访问超时时间

直接用urllib.request模块的urlopen（）获取页面，page的数据格式为bytes类型，需要decode（）解码，转换成str类型。

```
import urllib.request

url = "http://www.baidu.com"
data = urllib.request.urlopen(url).read()
data = data.decode('UTF-8')
print(data)
```

- urlopen返回对象提供方法：

  - read() , readline() ,readlines() , fileno() , close() ：对HTTPResponse类型数据进行操作

  - info()：返回HTTPMessage对象，表示远程服务器返回的头信息

  - getcode()：返回Http状态码。如果是http请求，200请求成功完成;404网址未找到

  - geturl()：返回请求的url

- **Request**：
urllib.request.Request(url, data=None, headers={}, method=None)

使用request（）来包装请求，再通过urlopen（）获取页面。

- 用来包装头部的数据：

  - User-Agent ：这个头部可以携带如下几条信息：浏览器名和版本号、操作系统名和版本号、默认语言

  - Referer：可以用来防止盗链，有一些网站图片显示来源http://***.com，就是检查Referer来鉴定的

  - Connection：表示连接状态，记录Session的状态。

```
from urllib import request,parse

url = 'http://httpbin.org/post'
headers = {
   'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) '
                  'Chrome/45.0.2454.85 Safari/537.36 115Browser/6.0.3',
   'Referer': 'http://httpbin.org',
   'Connection': 'keep-alive'
}
dict = {
  'name': 'grame'
}
data = bytes(parse.urlencode(dict),encoding='utf8')
req = request.Request(url=url, data=data, headers=headers,method='POST')
page = request.urlopen(req).read()
page = page.decode('utf-8')
print(page)

==========================
返回：
{
  "args": {},
  "data": "",
  "files": {},
  "form": {
    "name": "grame"
  },
  "headers": {
    "Accept-Encoding": "identity",
    "Connection": "close",
    "Content-Length": "10",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "httpbin.org",
    "Referer": "http://httpbin.org",
    "User-Agent": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.85 Safari/537.36 115Browser/6.0.3"
  },
  "json": null,
  "origin": "223.93.186.244",
  "url": "http://httpbin.org/post"
}


```
- **高级用法**
  - Handler: 继承这个 baseHandler 类，比较常见的类：
    - ProxyHandler：为请求设置代理
    - HTTPCookieProcessor：处理 HTTP 请求中的 Cookies
    - HTTPDefaultErrorHandler：处理 HTTP 响应错误。
    - HTTPRedirectHandler：处理 HTTP 重定向。
    - HTTPPasswordMgr：用于管理密码，它维护了用户名密码的表。
    - HTTPBasicAuthHandler：用于登录认证，一般和 HTTPPasswordMgr 结合使用。
  - 认证登录
    - HTTPPasswordMgrWithDefaultRealm
    - HTTPBasicAuthHandler
    - build_opener
    ```
    import urllib.request

    url = "http://tieba.baidu.com/"
    user = 'user'
    password = 'password'
    pwdmgr = urllib.request.HTTPPasswordMgrWithDefaultRealm()
    pwdmgr.add_password(None，url ，user ，password)

    auth_handler = urllib.request.HTTPBasicAuthHandler(pwdmgr)
    opener = urllib.request.build_opener(auth_handler)
    response = opener.open(url)
    print(response.read().decode('utf-8'))
    ```
  - 使用代理: ProxyHandler
    ```
    import urllib.request

    url = "http://tieba.baidu.com/"
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'
    }

    proxy_handler = urllib.request.ProxyHandler({
        'http': 'web-proxy.oa.com:8080',
        'https': 'web-proxy.oa.com:8080'
    })
    opener = urllib.request.build_opener(proxy_handler)
    urllib.request.install_opener(opener)

    request = urllib.request.Request(url=url, headers=headers)
    response = urllib.request.urlopen(request)
    print(response.read().decode('utf-8'))
    ```

  - Cookie:
    - http.cookiejar
    - urllib.request
    ```
    import http.cookiejar
    import urllib.request

    url = "http://tieba.baidu.com/"
    fileName = 'cookie.txt'

    cookie = http.cookiejar.CookieJar()
    handler = urllib.request.HTTPCookieProcessor(cookie)
    opener = urllib.request.build_opener(handler)
    response = opener.open(url)

    f = open(fileName,'a')
    for item in cookie:
        f.write(item.name+" = "+item.value+'\n')
    f.close()
    ```


## 2.异常处理
- URLError: URLError 是 urllib.error 异常类的基类, 可以捕获由urllib.request 产生的异常。

它具有一个属性reason，即返回错误的原因

```
import urllib.request
import urllib.error

url = "http://www.google.com"
try:
    response = request.urlopen(url)
except error.URLError as e:
    print(e.reason)
```

- HTTPError: 是 UEKRrror 的子类，专门处理 HTTP 和 HTTPS 请求的错误。它具有三个属性。 
  - 1)code：HTTP 请求返回的状态码。 
  - 2)renson：与父类用法一样，表示返回错误的原因。 
  - 3)headers：HTTP 请求返回的响应头信息。
  
获取 HTTP 异常的示例代码, 输出了错误状态码、错误原因、服务器响应头
```
import urllib.request
import urllib.error

url = "http://www.google.com"
try:
    response = request.urlopen(url)
except error.HTTPError as e:
   print('code: ' + e.code + '\n')
   print('reason: ' + e.reason + '\n')
   print('headers: ' + e.headers + '\n')
```


## 3.解析链接
- 1.urlparse()：url 的识别和分段
```
import urllib.request
import urllib.parse
url = "http://www.baidu.com"
parsed = urllib.parse.urlparse(url)
print(parsed)
#输出：ParseResult(scheme='http', netloc='www.baidu.com', path='', params='', query='', fragment='')
```

- 2.urlunparse()： 实现 url 的构造，参数长度必须为 6 ，
```
data=['http','www.baidu.com','index.html','user','a=6','comment']

print(urlunparse(data))

这样就完成了urlstring的构造
```


- 3.urlsplit()：url 解析，不单独解析 params 这一部分，只返回 5 个结果
```
urlsplict把urlstirng分割成5个部分，其中少了paramters

res=urlsplict('http://www.baidu.com/index.html;user?id=5#comment')

print(res)
```


- 4.urlunsplit()：将链接各个部分组合成完整链接的方法，传入参数也是一个可迭代对象
```

```


- 5.urljoin()：链接的解析，拼合和生成
```
import urllib.parse
url = "http://www.baidu.com"
new_path = urllib.parse.urljoin(url,"index.html")
print(new_path)
#输出：http://www.baidu.com/index.html
```


- 6.urlencode()：将字典类型转化成请求参数
```
import urllib.parse
dic = {'name':'melon','age':18}
data = urllib.parse.urlencode(dic)

print(data)     #age=18&name=melon
```


- 7.parse_qs()：将请求参数转化成字典
```
from urllib.parse import parse_qs

query='wd=query&tn=monline_dg&ie=utf-8'

print(parse_qs(query))

输出结果是：{'tn': ['monline_dg'], 'wd': ['query'], 'ie': ['utf-8']}
```


- 8.parse_qsl()： 将参数转化成元组组成的列表
```
from urllib.pase  import parse_qsl：将参数转换成为元组组成的列表

query='wd=query&tn=monline_dg&ie=utf-8'

print(parse_qsl(query))

输出结果：[('wd', 'query'), ('tn', 'monline_dg'), ('ie', 'utf-8')]
```


- 9.quote()： URL 编码
```
from urllib。parse import quote

keyword='美女'

url='https://www.baidu.com/s?wd='+quote(keyword)

print(url)

输出结果：https://www.baidu.com/s?wd=%E7%BE%8E%E5%A5%B3
```


- 10.unquote():  URL 解码
```
rom urllib.parse import unquote

url='https://www.baidu.com/s?wd=%E7%BE%8E%E5%A5%B3'

print(unquote(url))

输出结果：https://www.baidu.com/s?wd=美女
```







## 参考
- [python3 网络爬虫开发实战]()
- [urllib 模块的使用](https://www.cnblogs.com/Lands-ljk/p/5447127.html)
- [详解 python3 urllib](https://www.jianshu.com/p/2e190438bd9c)
- [python3 urllib 爬虫 Handler 处理器PRoxyHandlr 处理器（代理设置）](https://blog.csdn.net/yangxiaodong88/article/details/80759086)
