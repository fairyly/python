# error: Microsoft Visual C++ 14.0 is required

```
python3 -m pip install tesserocr pillow

 Failed to extract tesseract version from executable: [WinError 2] 系统找不到指定的文件。
  Supporting tesseract v3.04.00
  Building with configs: {'libraries': ['tesseract', 'lept'], 'cython_compile_time_env': {'TESSERACT_VERSION': 197632}}
  G:\Program Files\Python37\lib\distutils\dist.py:274: UserWarning: Unknown distribution option: 'long_description_content_type'
    warnings.warn(msg)
  running bdist_wheel
  running build
  running build_ext
  building 'tesserocr' extension
  error: Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools": http://landinghub.visualstudio.com/visual-cpp-build-tools

  ----------------------------------------
  Failed building wheel for tesserocr
```


## 常规的安装模块包都是通过：

```
pip install xxx
conda install xxx
github下载并解压： 
  wget https://github.com/amueller/word_cloud/archive/master.zip 
  unzip master.zip 
  rm master.zip 
  cd word_cloud-master 
安装依赖包： 
  sudo pip install -r requirements.txt 
安装wordcloud 
  python setup.py install
--------------------- 
作者：W-大泡泡 
原文：https://blog.csdn.net/u011389474/article/details/60764926 
```

最后下载 whl 文件安装,安装可以了

```
>python3 -m pip install tesserocr-2.3.1-cp37-cp37m-win_amd64.whl
```


## 参考
- [python 报错Microsoft Visual C++ 14.0 is required](https://blog.csdn.net/TH_NUM/article/details/77095177)