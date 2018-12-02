# RuntimeError: Failed to init API,

```
import tesserocr
from PIL import Image

image = Image.open('ocr.jpg')
result = tesserocr.image_to_text(image)
print(result)


Traceback (most recent call last):
  File "testOcr.py", line 5, in <module>
    result = tesserocr.image_to_text(image)
  File "tesserocr.pyx", line 2407, in tesserocr._tesserocr.image_to_text
RuntimeError: Failed to init API, possibly an invalid tessdata path: H:\Program Files\Python37\
```


## 解决

```
不难看出 tesserocr.py文件没有指定正确的tessdata 路径，

我的python也并非安装在 H:\Program Files\Python37\，

网上看到的方法都是添加D:\Program Files\Tesseract-OCR这个到系统环境变量，依然不成功，

目前没找到如何修改 tesserocr.py 关联的 tessdata path 有效方式，

但是比较简单粗暴的方法是，可以根据提示，直接手工新建 H:\Program Files\Python37\，

并将 Tesseract-OCR 对应的 tessdata文件夹整个拷贝到 H:\Program Files\Python37\即可。

亲测有效。
```




## 参考
- [Python3.6安装使用tesserocr文件时遇到问题](http://www.mamicode.com/info-detail-2353336.html)