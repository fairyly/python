# python import win32api 报ImportError: DLL load failed,找不到指定的模块

##  `scrapy crawl books` 执行爬虫代码的时候出现 

- 执行 `scrapy bench` 也是报这个错,找不到指定的模块
- `import win32api`  也是报这个错,找不到指定的模块


- 将 安装目录了文件 `/Lib/site-packages/pywin32_system32/*` 拷贝至 `C:/Windows/System32`

还是会出错

- GitHub 下载 pywin32 安装
  - [pywin32-releases](https://github.com/mhammond/pywin32/releases)


## 参考
- [python import win32api 报ImportError: DLL load failed: 找不到指定的模块](https://blog.csdn.net/yl20175514/article/details/82981087)
