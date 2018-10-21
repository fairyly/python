# Python 安装第三方库

安装工具包easy_install和pip

```
pip install 安装库的名
```


- 安装依赖:
```
pip install -r requirements.txt
```


## From Source (Universal)

- For any missing library, the source is usually available at https://pypi.python.org/pypi/. You can download requests here: https://pypi.python.org/pypi/requests

- On mac osx and windows, after downloading the source zip, uncompress it and from the termiminal/cmd run `python setup.py install` from the uncompressed dir.



## 安装库的方式（python3 为例）

- 1.pip 安装
>pip3 install 库名

- 2.wheel 安装
>先安装 wheel，pip3 install wheel,  
然后到 `PyPI` 上下载对应的 wheel 文件，downloads 页面，下载 .whl 到本地  
命令行进入 wheel 目录，利用 pip 安装  
pip3 install 名字.whl


- 3.源码安装
>git 下载源代码，  
到目录内
python3 setup.py install

- 验证安装（命令行）
>python3
import requests // 如果没报什么错误，库就安装成功了


## 参考
- [pypi](https://pypi.org/)
