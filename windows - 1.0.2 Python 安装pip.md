# windows--- 1.0.2 Python 安装pip.md


要安装第三方库,可以用 pip 安装

## 在 windows 上安装 pip 

- 1.To install pip, securely download `get-pip.py`. [1]:
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```
2.Then run the following:
```
python get-pip.py
```

如果出现 ` Retrying (Retry(total=0, connect=None, read=None, redirect=None, status=None))`

就要换源

## 如果想配置成默认的源，方法如下：

需要创建或修改配置文件（一般都是创建），

linux的文件在~/.pip/pip.conf，

windows在%HOMEPATH%\pip\pip.ini，

>在运行中输入 `%HOMEPATH%` 找到目录,新建 `pip` 目录, 新建 `pip.ini` 文件

修改内容为：
```
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
```

这样在使用pip来安装时，会默认调用该镜像。

- 参考: [windows issue-1.1.2](https://github.com/fairyly/python/blob/master/windows%20issue-%201.1.2%20python%E4%B8%AD%E5%AE%89%E8%A3%85%E5%8C%85%E5%87%BA%E7%8E%B0Retrying%20(Retry(total%3D4%2C%20connect%3DNone%2C%20read%3DNone%2C%20redirect%3DNone%2C%20status%3DNone)).md)


## 参考
- [pip---Installation](https://pip.pypa.io/en/stable/installing/)
