# scrapy爬虫框架

## 启动

1.通过命令行方式

````python
scrapy crawl spiderName
````

2.通过在项目目录下写启动脚本

````python
from scrapy import cmdline
cmdline.execute("scrapy crawl test".split())
````

3.通过API

在你写的parse的文件下或者新建一个文件写入下面代码

````python
rom scrapy.crawler import CrawlerProcess

from testSpider.spiders.test import TestSpider

if __name__ ==  '__main__':
    process = CrawlerProcess(
        {'User_Agent' : 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36'}
    )
    process.crawl(TestSpider)
    process.start()
````

4.一个进程中启动多个爬虫

```python
import scrapy 
from scrapy.crawler import CrawlerProcess
process = CrawlerProcess()
process.crawl(spoder1)
process.crawl(spider2)
process.start()
```

==extract_first()==等同于extract()[0]并且当找不到元素时返回`None`