# scrapy 爬虫框架

>需要安装几个依赖后再安装 scrapy， 具体参考 [scrapy 爬虫框架安装](https://github.com/fairyly/python/blob/master/python3%20%E7%88%AC%E8%99%AB%E5%BC%80%E5%8F%91%E5%AE%9E%E6%88%98%EF%BC%88book%EF%BC%89.md#7%E7%88%AC%E8%99%AB%E6%A1%86%E6%9E%B6)

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

- 1.创建项目，输入 shell 命令
```
  scrapy startproject scrapyDemo
```

- 2.创建文件，在项目目录 scrapyDemo/spiders下新建文件  `book_spide.py`
```
# scrapyDemo/spiders/book_spide.py

import scrapy

class BooksSpider(scrapy.Spider):
  # 每个爬虫的唯一标识
  name = "books"

  # 定义爬虫的起始点，可以有多个，这里只有一个
  start_urls = ['http://books.toscrape.com/']
  def start_requests(self):
    urls = [
        'http://books.toscrape.com/'
    ]
    for url in urls:
        yield scrapy.Request(url=url, callback=self.parse)

  #
  def parse(self,response):
    # 提取数据
    # 每一本书的信息在 <article class="product_pod"></article>中
    # 使用 css() 方法找到这样的元素，依次迭代
    for book in response.css('article.product_pod'):
      # 书名信息在的元素 h3 > a 的 title 
      name = book.xpath('./h3/a/@title').extract_first()

      # 书价信息所在元素 <p class="price_color">£33.34</p>
      price = book.css('p.price_color::text').extract_first()

      yield {
        'name': name,
        'price': price,
      }
    # 提取链接
    # 下一页链接在 的元素  <li class="next"><a href="catalogue/page-2.html">next</a></li>
    
    next_url = response.css('ul.page li.next a::attr(href)').extract_first()
    if next_url:
      # 如果找到下一页的 url,得到绝对路径，构造新的 request 对象
      next_url = response.urljoin(next_url)
      yield scrapy.Request(next_url,callback=self.parse) 
```

- 3. 执行爬虫，保存爬到的数据到 book.csv
```
  scrapy crawl books -o books.csv
```
  - 遇到的问题: 找不到 `pywin32` 模块，又去 [GitHub](https://github.com/mhammond/pywin32/releases) 下载安装

- 查看 爬到的数据,输入 shell 命令
```
  sed -n '2,$p' books.csv | cat -n
```


## 参考
- [Github](https://github.com/scrapy/scrapy/)
- [Docs](https://doc.scrapy.org/en/latest/intro/tutorial.html)
- [scrapy 中文文档](http://www.scrapyd.cn/doc/181.html)
- [ScrapyProject](https://github.com/cuanboy/ScrapyProject)
- [scrapyTest](https://github.com/cuanboy/scrapyTest)
