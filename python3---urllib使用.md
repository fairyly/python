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
  - 使用代理: ProxyHandler

  - Cookie:
   


## 2.异常处理




## 3.解析链接




## 参考
- [python3 网络爬虫开发实战]()
- [urllib 模块的使用](https://www.cnblogs.com/Lands-ljk/p/5447127.html)
- [详解 python3 urllib](https://www.jianshu.com/p/2e190438bd9c)
- [python3 urllib 爬虫 Handler 处理器PRoxyHandlr 处理器（代理设置）](https://blog.csdn.net/yangxiaodong88/article/details/80759086)
