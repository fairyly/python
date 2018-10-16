# python中安装包出现Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None))

也需要换源

>pip install requests -i http://pypi.douban.com/simple --trusted-host pypi.douban.com


如果想配置成默认的源，方法如下：

需要创建或修改配置文件（一般都是创建），

linux的文件在~/.pip/pip.conf，

windows在%HOMEPATH%\pip\pip.ini），

修改内容为：

```
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
```
 
这样在使用pip来安装时，会默认调用该镜像。



## 参考
- [Retry(total=4, connect=None, read=None, redirect=None, status=None)](https://blog.csdn.net/qq_25964837/article/details/80295041)
