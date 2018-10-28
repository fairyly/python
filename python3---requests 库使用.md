# python3---requests 库使用


## get 请求

```
import requests

response = requests.get('https://www.python.org')
print(response.status_code)
print(response.text)
print(response.cookie)
```

- 带参数

```
r1 = requests.get(url='http://dict.baidu.com/s', params={'wd': 'python'})      # 带参数的get请求
```

- 响应的内容
```
r.encoding                       #获取当前的编码
r.encoding = 'utf-8'             #设置编码
r.text                           #以encoding解析返回内容。字符串方式的响应体，会自动根据响应头部的字符编码进行解码。
r.content                        #以字节形式（二进制）返回。字节方式的响应体，会自动为你解码 gzip 和 deflate 压缩。

r.headers                        #以字典对象存储服务器响应头，但是这个字典比较特殊，字典键不区分大小写，若键不存在则返回None

r.status_code                     #响应状态码
r.raw                             #返回原始响应体，也就是 urllib 的 response 对象，使用 r.raw.read()   
r.ok                              # 查看r.ok的布尔值便可以知道是否登陆成功
 #*特殊方法*#
r.json()                         #Requests中内置的JSON解码器，以json形式返回,前提返回的内容确保是json格式的，不然解析出错会抛异常
r.raise_for_status()             #失败请求(非200响应)抛出异常
```

- 定制头和cookie信息

```
import requests
import json
 
data = {'some': 'data'}
headers = {'content-type': 'application/json',
           'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:22.0) Gecko/20100101 Firefox/22.0'}
 
r = requests.post('https://api.github.com/some/endpoint', data=data, headers=headers)
print(r.text)
```

- 发送你的cookies到服务器，可以使用 cookies 参数
```
import requests
 
url = 'http://httpbin.org/cookies'
cookies = {'testCookies_1': 'Hello_Python3', 'testCookies_2': 'Hello_Requests'}
# 在Cookie Version 0中规定空格、方括号、圆括号、等于号、逗号、双引号、斜杠、问号、@，冒号，分号等特殊符号都不能作为Cookie的内容。
r = requests.get(url, cookies=cookies)
print(r.json())
```

- 代理

```
proxies = {'http':'ip1','https':'ip2' }
requests.get('url',proxies=proxies)
```

- 上传文件

```
import requests
 
url = 'http://127.0.0.1:8080/upload'
files = {'file': open('/home/rxf/test.jpg', 'rb')}
#files = {'file': ('report.jpg', open('/home/lyb/sjzl.mpg', 'rb'))}     #显式的设置文件名
 
r = requests.post(url, files=files)
print(r.text)
```


-  身份验证

```
import requests
from requests.auth import HTTPBasicAuth
 
r = requests.get('https://httpbin.org/hidden-basic-auth/user/passwd', auth=HTTPBasicAuth('user', 'passwd'))
# r = requests.get('https://httpbin.org/hidden-basic-auth/user/passwd', auth=('user', 'passwd'))    # 简写
print(r.json())
```


## requests模块抓取网页源码并保存到文件示例

这是一个基本的文件保存操作，但这里有几个值得注意的问题：

1.安装requests包，命令行输入pip install requests即可自动安装。很多人推荐使用requests，自带的urllib.request也可以抓取网页源码

2.open方法encoding参数设为utf-8，否则保存的文件会出现乱码。

3.如果直接在cmd中输出抓取的内容，会提示各种编码错误，所以保存到文件查看。

4.with open方法是更好的写法，可以自动操作完毕后释放资源。

```
#! /urs/bin/python3
import requests

'''requests模块抓取网页源码并保存到文件示例'''
html = requests.get("http://www.baidu.com")
with open('test.txt', 'w', encoding='utf-8') as f:
    f.write(html.text)
    
'''读取一个txt文件，每次读取一行，并保存到另一个txt文件中的示例'''
ff = open('testt.txt', 'w', encoding='utf-8')
with open('test.txt', encoding="utf-8") as f:
    for line in f:
        ff.write(line)
        ff.close()

因为在命令行中打印每次读取一行的数据，中文会出现编码错误，所以每次读取一行并保存到另一个文件，这样来测试读取是否正常。
（注意open的时候制定encoding编码方式）
```


## requests模拟登陆GitHub

```
import requests
 2 from bs4 import BeautifulSoup
 3 
 4 
 5 def login_github():
 6     """
 7     通过requests模块模拟浏览器登陆GitHub
 8     :return: 
 9     """
10     # 获取csrf_token
11     r1 = requests.get('https://github.com/login')   # 获得get请求的对象
12     s1 = BeautifulSoup(r1.text, 'html.parser')      # 使用bs4解析HTML对象
13     token = s1.find('input', attrs={'name': 'authenticity_token'}).get('value')     # 获取登陆授权码，即csrf_token
14     get_cookies = r1.cookies.get_dict()     # 获取get请求的cookies，post请求时必须携带
15     
16     # 发送post登陆请求
17     '''
18     post登陆参数
19     commit    Sign+in
20     utf8    ✓
21     authenticity_token    E961jQMIyC9NPwL54YPj70gv2hbXWJ…fTUd+e4lT5RAizKbfzQo4eRHsfg==
22     login    JackUpDown（用户名）
23     password    **********（密码）
24     '''
25     r2 = requests.post(
26         'https://github.com/session',
27         data={
28             'commit': 'Sign+in',
29             'utf8': '✓',
30             'authenticity_token': token,
31             'login': 'JackUpDown',
32             'password': '**********'
33         },
34         cookies=get_cookies     # 携带get请求的cookies
35                        )
36     login_cookies = r2.cookies.get_dict()   # 获得登陆成功的cookies，携带此cookies就可以访问任意GitHub页面
37 
38     # 携带post cookies跳转任意页面
39     r3 = requests.get('https://github.com/settings/emails', cookies=login_cookies)
40     print(r3.text)
```

## 抓取猫眼电影排行：
- [完整 code](https://github.com/Python3WebSpider/MaoYan/blob/master/spider.py)

```
import requests
 
def get_one_page(url):
    headers ={
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.170 Safari/537.36',
        }
    response = requests.get(url,headers = headers)
    if response.status_code ==200:
        return response.text
    return None
 
def main():
    url = 'http://maoyan.com/board/4'
    html = get_one_page(url)
    print(html)
    
main()
============================

# 正则提取

import requests
import re
 
def get_one_page(url):
    headers ={
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.170 Safari/537.36',
        }
    response = requests.get(url,headers = headers)
    if response.status_code ==200:
        return response.text
    return None

def parse_one_page(html):
    pattern = re.compile('<dd>.*?board-index.*?>(\d+)</i>.*?data-src="(.*?)".*?name"><a'
                        + '.*?>(.*?)</a>.*?star">(.*?)</p>.*?releasetime">(.*?)</p>'
                        + '.*?integer">(.*?)</i>.*?fraction">(.*?)</i>.*?</dd>', re.S)
    items = re.findall(pattern, html)
    # print(items)
    for item in items:
      yield {
        'index':item[0],
        'image':item[1],
        'title':item[2],
        'actor':item[3].strip()[3:],
        'time':item[4].strip()[5:],
        'score':item[5] + item[6]
      }
 
def main():
    url = 'http://maoyan.com/board/4'
    html = get_one_page(url)
    # print(html)
    for item in parse_one_page(html):
      print(item)
    
main()


================================

# 写入文件
import json # 需要引入 json 库

def write_to_file(content):
    with open('result.txt', 'a', encoding='utf-8') as f:
        f.write(json.dumps(content, ensure_ascii=False) + '\n')

```

## 参考
- [python-requests 快速上手](http://cn.python-requests.org/zh_CN/latest/user/quickstart.html)
- [python3_requests模块详解](https://www.cnblogs.com/ranxf/p/7808537.html)
