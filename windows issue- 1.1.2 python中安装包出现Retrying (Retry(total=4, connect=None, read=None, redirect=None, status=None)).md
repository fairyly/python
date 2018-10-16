# python中安装包出现Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None))

也需要换源

搜集了一些国内的pip源，如下：

阿里云 http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 

豆瓣(douban) http://pypi.douban.com/simple/ 

清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

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
