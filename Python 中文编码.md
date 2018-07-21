# Python 中文编码

Python 文件中如果未指定编码，在执行过程会出现报错：

Python中默认的编码格式是 ASCII 格式，在没修改编码格式时无法正确打印汉字，所以在读取中文时会报错。

解决方法为只要在文件开头加入 # -*- coding: UTF-8 -*- 或者 #coding=utf-8 就行了


## demo

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
print "你好，世界";
```


我文件中添加了可是输出的不是代码中中文

## Python设置默认编码为UTF-8






## 问题
- https://blog.ernest.me/post/python-setdefaultencoding-unicode-bytes
