# python3---1.2.2pyspider 框架使用

>python 3.7 好像不太支持，支持 python 3.6


## install

```
pyspider
PhantomJS
MongoDB
PyMongo
```

- windows 10 下安装时候报错

```

pip install pyspider
Command "python setup.py egg_info" failed with error code 10 in C:\Users\fairy\AppData\Local\Temp\pip-install-4fo556qe\pycurl\
```
>这是 pycurl 安装错误，需要安装 pycurl

`https://www.lfd.uci.edu/~gohlke/pythonlibs/#pycurl`

进入下载对应 wheel 文件版本



- 运行报错

```
Traceback (most recent call last):
  File "g:\program files\python37\lib\runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "g:\program files\python37\lib\runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "G:\Program Files\Python37\Scripts\pyspider.exe\__main__.py", line 5, in <module>
  File "g:\program files\python37\lib\site-packages\pyspider\run.py", line 231
    async=True, get_object=False, no_input=False):
        ^
SyntaxError: invalid syntax
```



## 参考
- [Python3.7 dont'work- issues:841](https://github.com/binux/pyspider/issues/841)