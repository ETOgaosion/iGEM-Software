#Scrapy Library

[摘自scrapy库官方文档](https://docs.scrapy.org/en/latest/intro/tutorial.html#our-first-spiderhttps://requests.readthedocs.io/en/master/user/quickstart/)&nbsp;&nbsp;&nbsp;author:高梓源&nbsp;&nbsp;&nbsp;date:1.26

---

[TOC]

##Quick Start

首先安装scrapy包，`pip install scrapy`即可

使用scrapy爬虫适用于大型项目，首先选定项目地址建立目录（即文件夹），windows下`cmd here`，输入

```
scrapy startproject tutorial
```

start一个project，名字是tutorial，建立项目目录，包含以下：

```
tutorial/
    scrapy.cfg            # 部署配置文件

    tutorial/             # py模块，代码位置
        __init__.py

        items.py          # project items definition file

        middlewares.py    # project middlewares file 中间件 连接应用程序与操作系统，实现动态网页、传输数据

        pipelines.py      # project pipelines file 数据传输流水线

        settings.py       # project settings file

        spiders/          # a directory where you'll later put your spiders
            __init__.py
```

###示例一

####第一个爬虫

在`tutorial/tutorial/spiders/`中新建文件`quotes_spider.py`，输入以下代码并保存：

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"

    def start_requests(self):
        urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/',
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse)

    def parse(self, response):
        page = response.url.split("/")[-2]
        filename = f'quotes-{page}.html'
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log(f'Saved file {filename}')
```

首先建立spider子类`scrapy.Spider`，定义以下属性和方法：

-`name`：标识spider，在项目中唯一，不同spider的name不同

-`start_requests`：加入可迭代的spider，一直爬行，注意到**yield**关键字，为迭代器的象征，可看作`return`

>插入迭代器review：

```python
def foo():
    print("starting...")
    while True:
        res = yield 4
        print("res:",res)

g = foo()#函数中有迭代器yeild，不会真正执行，返回一个生成器对象

print(next(g))#调用next方法，foo函数真正从头顺序执行，遇到yield即返回4，不会赋值res

print("*"*20)

print(next(g))#调用next方法，从上一步未完成步开始，由于上步直接返回，因此res赋值None

#由于还在while循环，再进行一次yield，返回4
```


-`parse`：解析查询结果的方法，此处为将内容直接写入文件，参数`response`为响应结果实例。

####运行

在项目顶级目录，即首个`tutorial/`，运行

```
scrapy crawl quotes
```

检查当前`tutrial/`目录中的文件。已经创建了两个新文件：quotes-1.html和quotes-2.html，对应爬取的两个网页。

##提取数据：CSS选择器

在工程根目录`tutorial/`中使用：

```
scrapy shell "http://quotes.toscrape.com/page/1/"
```

而后即可用css选择器与正则表达式查询，返回值为列表，如：

```
>>> response.css('title')
[<Selector xpath='descendant-or-self::title' data='<title>Quotes to Scrape</title>'>]
>>> response.css('title::text').getall()
['Quotes to Scrape']

#加入正则表达式
>>> response.css('title::text').re(r'Quotes.*')
['Quotes to Scrape']
>>> response.css('title::text').re(r'Q\w+')
['Quotes']
>>> response.css('title::text').re(r'(\w+) to (\w+)')
['Quotes', 'Scrape']
```

###结合spyder

```python
import scrapy


class QuotesSpider(scrapy.Spider):
    name = "quotes"
    start_urls = [
        'http://quotes.toscrape.com/page/1/',
        'http://quotes.toscrape.com/page/2/',
    ]#只需要两个url，无需request

    def parse(self, response):
        for quote in response.css('div.quote'):
            yield {
                'text': quote.css('span.text::text').get(),
                'author': quote.css('small.author::text').get(),
                'tags': quote.css('div.tags a.tag::text').getall(),
            }
```

##存储获得数据

依然在工程根目录`tutorial/`，使用：

```
scrapy crawl quotes -O quotes.json
```

生成json文件，即数据库文本，数据结构与python相似

##学习项目

[见于此github]('https://github.com/scrapy/quotesbot')