# scrapy 爬虫框架

>需要安装几个依赖后再安装 scrapy， 具体参考 []()

## scrapy 命令

- 全局命令
```
    startproject    创建项目
    settings        获取 scrapy 的设定
    runspider       基于文件运行，也就是说你按照 scrapy 的蜘蛛格式编写了一个 py 文件，`scrapy runspider scrapy_cn.py`
    shell           主要是调试用，里面还有很多细节的命令 `scrapy shell http://www.scrapyd.cn`
    fetch           模拟我们的蜘蛛下载页面，也就是说用这个命令下载的页面就是我们蜘蛛运行时下载的页面，这样的好处就是能准确诊断出，我们的到的 html 结构到底是不是我们所看到的
    view            和 fetch 类似都是查看蜘蛛看到的是否和你看到的一致，便于排错
    version         查看scrapy版本,`scrapy version`
```

- 项目命令 
```
    crawl      运行XX蜘蛛,基于项目运行,直接在项目目录运行命令
    check      检查蜘蛛
    list       显示有多少个蜘蛛
    edit       使用编辑器打开爬虫文件，（Windows 上似乎有问题，Linux 上没有问题）：`scrapy edit f1`
    parse      获取给定的 URL 并使用相应的 spider 分析处理
    genspider  根据蜘蛛模板创建蜘蛛的命令: http://www.scrapyd.cn/jiaocheng/148.html
    deploy     将项目部署到scrapyd服务
    bench     测试本地硬件性能（工作原理：）：scrapy bench （如果遇到问题：解决问题: import win32api ImportError: DLL load failed，到这里查看解决办法。
```

## demo 





## 参考
- [Github](https://github.com/scrapy/scrapy/)
- [Docs](https://doc.scrapy.org/en/latest/intro/tutorial.html)
- [scrapy 中文文档](http://www.scrapyd.cn/doc/181.html)
- [ScrapyProject](https://github.com/cuanboy/ScrapyProject)
- [scrapyTest](https://github.com/cuanboy/scrapyTest)
