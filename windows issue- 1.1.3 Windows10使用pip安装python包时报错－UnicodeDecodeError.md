# Windows10使用pip安装python包时报错－UnicodeDecodeError: 'ascii' codec c
 先换源了还是报错，
 看到 V2EX 中的
 >用以下两种方法试试
1.pip install Scrapy --upgrade --cache-dir=临时目录路径
2.python 目录 Python27\Lib\site-packages 建一个文件 sitecustomize.py 
内容写：
```
import sys 
sys.setdefaultencoding('gb2312')
```
 - 我设置的, 安装没问题了，不过出现乱码
 ```
import sys 
sys.setdefaultencoding('utf-8')
```
## 参考
- [UnicodeDecodeError: 'ascii' codec c](http://www.cnblogs.com/songzhenhua/p/9312723.html)
- [](https://www.v2ex.com/t/301350)
