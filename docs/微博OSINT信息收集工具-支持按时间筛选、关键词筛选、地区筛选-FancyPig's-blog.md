<!--yml
category: 社会工程
date: 2022-11-10 10:28:09
-->

# 微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog

> 来源：[https://www.iculture.cc/knowledge/pig=24574](https://www.iculture.cc/knowledge/pig=24574)

## 杂谈

之前我们分享了很多期国外的社交平台[OSINT工具](https://iculture.cc/osint)教程

本期我们将针对国内的社交平台，分享关于微博的OSINT工具

## 效果演示

普通人的搜索方式，我们直接在**微博网页版**中搜索关键词

<figure class="wp-block-image size-full">![图片[1]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/54ec706ecbccfffdacc1219d0dafe56e.png)</figure>

但是程序员可以通过脚本进行快速获取

<figure class="wp-block-image size-large">![图片[2]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/6eac13c8fad0e39c2b2f156beca4c18a.png)</figure>

<figure class="wp-block-image size-full">![图片[3]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/16d69188883bbd1b72ad2faacbdb9025.png)

<figcaption>点击放大查看动图</figcaption>

</figure>

最终可以生成CSV文件，方便分析

<figure class="wp-block-image size-large">![图片[4]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/d3f3e7fb6eeeec76f9bbd2f3691dcec0.png)</figure>

## 功能介绍

针对微博内容进行个性化检索，导出

*   支持关键词（单个**关键词**，同时包含**多个关键词**）
*   支持时间筛选（**初始时间**、**结束时间**）
*   支持微博来源筛选（**原创微博**、**热门微博**、**关注人微博**、**认证用户微博**、**媒体微博**、**观点微博**）
*   支持微博类型筛选（**全部微博**、**包含图片**的微博、**包含视频**的微博、**包含音乐**的微博、**包含短链接**的微博）
*   支持地区筛选（精确到省或直辖市，可以同时包含多个地区）
*   支持自定义访问频率

同时可以导出多种格式的结果

*   **csv文件**（默认）
*   **MySQL数据库**（可选）
*   **MongoDB数据库**（可选）

对于**图片**或者**视频**附件也可以选择性进行下载

## 图文教程

### 下载工具

以下步骤默认您已经下载好项目并解压缩了！

### 安装Scrapy

```
pip install scrapy
```

### 安装依赖

```
pip install -r requirements.txt
```

### 搜索配置

详见weibo-search/weibo/**settings.py**

其中有详细的注释

```
# -*- coding: utf-8 -*-

BOT_NAME = 'weibo'
SPIDER_MODULES = ['weibo.spiders']
NEWSPIDER_MODULE = 'weibo.spiders'
COOKIES_ENABLED = False
TELNETCONSOLE_ENABLED = False
LOG_LEVEL = 'ERROR'
# 访问完一个页面再访问下一个时需要等待的时间，默认为10秒
DOWNLOAD_DELAY = 10
DEFAULT_REQUEST_HEADERS = {
    'Accept':
    'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8,en-US;q=0.7',
    'cookie': 'your cookie'
}
ITEM_PIPELINES = {
    'weibo.pipelines.DuplicatesPipeline': 300,
    'weibo.pipelines.CsvPipeline': 301,
    # 'weibo.pipelines.MysqlPipeline': 302,
    # 'weibo.pipelines.MongoPipeline': 303,
    # 'weibo.pipelines.MyImagesPipeline': 304,
    # 'weibo.pipelines.MyVideoPipeline': 305
}
# 要搜索的关键词列表，可写多个, 值可以是由关键词或话题组成的列表，也可以是包含关键词的txt文件路径，
# 如'keyword_list.txt'，txt文件中每个关键词占一行
KEYWORD_LIST = ['迪丽热巴']  # 或者 KEYWORD_LIST = 'keyword_list.txt'
# 要搜索的微博类型，0代表搜索全部微博，1代表搜索全部原创微博，2代表热门微博，3代表关注人微博，4代表认证用户微博，5代表媒体微博，6代表观点微博
WEIBO_TYPE = 1
# 筛选结果微博中必需包含的内容，0代表不筛选，获取全部微博，1代表搜索包含图片的微博，2代表包含视频的微博，3代表包含音乐的微博，4代表包含短链接的微博
CONTAIN_TYPE = 0
# 筛选微博的发布地区，精确到省或直辖市，值不应包含“省”或“市”等字，如想筛选北京市的微博请用“北京”而不是“北京市”，想要筛选安徽省的微博请用“安徽”而不是“安徽省”，可以写多个地区，
# 具体支持的地名见region.py文件，注意只支持省或直辖市的名字，省下面的市名及直辖市下面的区县名不支持，不筛选请用“全部”
REGION = ['全部']
# 搜索的起始日期，为yyyy-mm-dd形式，搜索结果包含该日期
START_DATE = '2022-01-01'
# 搜索的终止日期，为yyyy-mm-dd形式，搜索结果包含该日期
END_DATE = '2022-10-11'
# 进一步细分搜索的阈值，若结果页数大于等于该值，则认为结果没有完全展示，细分搜索条件重新搜索以获取更多微博。数值越大速度越快，也越有可能漏掉微博；数值越小速度越慢，获取的微博就越多。
# 建议数值大小设置在40到50之间。
FURTHER_THRESHOLD = 46
# 图片文件存储路径
IMAGES_STORE = './'
# 视频文件存储路径
FILES_STORE = './'
# 配置MongoDB数据库
# MONGO_URI = 'localhost'
# 配置MySQL数据库，以下为默认配置，可以根据实际情况更改，程序会自动生成一个名为weibo的数据库，如果想换其它名字请更改MYSQL_DATABASE值
# MYSQL_HOST = 'localhost'
# MYSQL_PORT = 3306
# MYSQL_USER = 'root'
# MYSQL_PASSWORD = '123456'
# MYSQL_DATABASE = 'weibo'
```

### 如何获取Cookie？填到哪里？

打开[微博](https://www.iculture.cc/?golink=aHR0cHM6Ly93ZWliby5jb20vc2V0L2luZGV4)，F12打开**开发者工具**，刷新页面，找到**index**，复制右侧的Cookie内容即可

<figure class="wp-block-image size-large">![图片[5]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/f681ffba35535fc70927e415c0b8ec1d.png)</figure>

然后填入我们刚才提到的**settings.py**文件中

```
DEFAULT_REQUEST_HEADERS = {
    'Accept':
    'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8,en-US;q=0.7',
    'cookie': 'Cookie复制到这里'
}
```

在第15行 **Cookie复制到这里** 的位置填入你的cookie

```
'cookie': 'Cookie复制到这里'
```

### 运行脚本

```
scrapy crawl search -s JOBDIR=crawls/search
```

## 实战分享

这里又回到了开头，如果我们想获取关于**西北工业大学NSA事件**相关的报道，我们可以对**settings.py**进行以下配置

代码第27行，我们首先要确定我们要索引的关键词

```
KEYWORD_LIST = ['西北工业大学NSA,西工大NSA'] 
```

第29行，我们要确定微博来源，我们选择**原创微博**

其他参数您也可以参考

> 0代表搜索全部微博，1代表搜索全部原创微博，2代表热门微博，3代表关注人微博，4代表认证用户微博，5代表媒体微博，6代表观点微博

```
WEIBO_TYPE = 1
```

第31行，我们不进行筛选

> 筛选结果微博中必需包含的内容，0代表不筛选，获取全部微博，1代表搜索包含图片的微博，2代表包含视频的微博，3代表包含音乐的微博，4代表包含短链接的微博

```
CONTAIN_TYPE = 0
```

第34行，我们这里看全部的，当然您也可以在设置时，看某个地区的

> 筛选微博的发布地区，精确到省或直辖市，值不应包含“省”或“市”等字，如想筛选北京市的微博请用“北京”而不是“北京市”，想要筛选安徽省的微博请用“安徽”而不是“安徽省”，可以写多个地区，详见**region.py**

```
REGION = ['全部']
```

第36行，选择微博的起始时间

> 搜索的起始日期，为yyyy-mm-dd形式，搜索结果包含该日期

```
START_DATE = '2022-09-01'
```

第38行，选择微博的结束时间

> 搜索的终止日期，为yyyy-mm-dd形式，搜索结果包含该日期

```
END_DATE = '2022-10-11'
```

然后就可以开始运行了！

```
scrapy crawl search -s JOBDIR=crawls/search
```

完整的settings.py代码我也放在下方

```
# -*- coding: utf-8 -*-

BOT_NAME = 'weibo'
SPIDER_MODULES = ['weibo.spiders']
NEWSPIDER_MODULE = 'weibo.spiders'
COOKIES_ENABLED = False
TELNETCONSOLE_ENABLED = False
LOG_LEVEL = 'ERROR'
# 访问完一个页面再访问下一个时需要等待的时间，默认为10秒
DOWNLOAD_DELAY = 10
DEFAULT_REQUEST_HEADERS = {
    'Accept':
    'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8,en-US;q=0.7',
    'cookie': '填你自己的Cookie哦'
}
ITEM_PIPELINES = {
    'weibo.pipelines.DuplicatesPipeline': 300,
    'weibo.pipelines.CsvPipeline': 301,
    # 'weibo.pipelines.MysqlPipeline': 302,
    # 'weibo.pipelines.MongoPipeline': 303,
    # 'weibo.pipelines.MyImagesPipeline': 304,
    # 'weibo.pipelines.MyVideoPipeline': 305
}
# 要搜索的关键词列表，可写多个, 值可以是由关键词或话题组成的列表，也可以是包含关键词的txt文件路径，
# 如'keyword_list.txt'，txt文件中每个关键词占一行
KEYWORD_LIST = ['西北工业大学NSA,西工大NSA']  # 或者 KEYWORD_LIST = 'keyword_list.txt'
# 要搜索的微博类型，0代表搜索全部微博，1代表搜索全部原创微博，2代表热门微博，3代表关注人微博，4代表认证用户微博，5代表媒体微博，6代表观点微博
WEIBO_TYPE = 1
# 筛选结果微博中必需包含的内容，0代表不筛选，获取全部微博，1代表搜索包含图片的微博，2代表包含视频的微博，3代表包含音乐的微博，4代表包含短链接的微博
CONTAIN_TYPE = 0
# 筛选微博的发布地区，精确到省或直辖市，值不应包含“省”或“市”等字，如想筛选北京市的微博请用“北京”而不是“北京市”，想要筛选安徽省的微博请用“安徽”而不是“安徽省”，可以写多个地区，
# 具体支持的地名见region.py文件，注意只支持省或直辖市的名字，省下面的市名及直辖市下面的区县名不支持，不筛选请用“全部”
REGION = ['全部']
# 搜索的起始日期，为yyyy-mm-dd形式，搜索结果包含该日期
START_DATE = '2022-09-01'
# 搜索的终止日期，为yyyy-mm-dd形式，搜索结果包含该日期
END_DATE = '2022-10-11'
# 进一步细分搜索的阈值，若结果页数大于等于该值，则认为结果没有完全展示，细分搜索条件重新搜索以获取更多微博。数值越大速度越快，也越有可能漏掉微博；数值越小速度越慢，获取的微博就越多。
# 建议数值大小设置在40到50之间。
FURTHER_THRESHOLD = 46
# 图片文件存储路径
IMAGES_STORE = './images'
# 视频文件存储路径
FILES_STORE = './videos'
# 配置MongoDB数据库
# MONGO_URI = 'localhost'
# 配置MySQL数据库，以下为默认配置，可以根据实际情况更改，程序会自动生成一个名为weibo的数据库，如果想换其它名字请更改MYSQL_DATABASE值
# MYSQL_HOST = 'localhost'
# MYSQL_PORT = 3306
# MYSQL_USER = 'root'
# MYSQL_PASSWORD = '123456'
# MYSQL_DATABASE = 'weibo' 
```

运行完成后，我们可以看到这里多了个**结果文件**

<figure class="wp-block-image size-full">![图片[6]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/086498b0ce68c86f813cc75dc75069f3.png)</figure>

最终得到相关的excel文件

<figure class="wp-block-image size-full">![图片[7]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/b7796228822f644f606c90dd42721786.png)</figure>

打开之后，我们可以根据转发数等参数进行筛选，找到用户阅读比较多的文章，进一步分析

<figure class="wp-block-image size-large">![图片[8]-微博OSINT信息收集工具 支持按时间筛选、关键词筛选、地区筛选-FancyPig's blog](img/9c7ee2230089cc09a8323c8c79b16b51.png)</figure>