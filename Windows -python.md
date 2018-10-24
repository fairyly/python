# Windows -python

- website: https://www.python.org/
- download: https://www.python.org/downloads/
- window download: https://www.python.org/downloads/windows/


## 多版本安装

- 安装后配置环境变量
如安装在 `G:\Python27`,环境变量配置

```
G:\Python27
G:\Python27\Scripts
```

- 安装 python 3

```
G:\Program Files\Python37
G:\Program Files\Python37\Scripts
```

- 设置别名

>把安装的 `python.exe` 复制一份，命名为 `python2.exe` / `python3.exe`

删除 scripts 下的 删除自带 `pip.exe和pip27/pip37.exe `,只保留 `pip2.exe/pip3.exe`


- 安装库的时候 如果 直接用

```
pip3 install requests

会报错，需要指定版本

python2 -m pip install xxx

python3 -m pip install requests
```

- 运行 py 文件也是指定版本
```
python3  name.py
```
