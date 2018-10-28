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



## 参考
- [python3 网络爬虫开发实战]()
