<!--yml
category: 社会工程
date: 2022-11-10 10:29:05
-->

# 如何使用PhoneInfoga开源引擎搜集手机号信息？-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=6289](https://www.iculture.cc/sg/pig=6289)

## 杂谈

最近一直在强调国外的Osint框架，Osint是Open-source intelligence的缩写，译为**开源情报**；

说人话就是可以在互联网上通过**搜索引擎搜索到的内容**。

## 视频教程

## 部署方法

```
docker pull sundowndev/phoneinfoga:latest
```

*   通过8080端口映射，您可以访问本地的8080端口进入网页

```
docker run -it -p 8080:8080 sundowndev/phoneinfoga  serve -p 8080
```

<figure class="wp-block-image size-large">![图片[1]-如何使用PhoneInfoga开源引擎搜集手机号信息？-FancyPig's blog](img/caff32ab20b3df2083a488fd7d52dd24.png)</figure>

然后就可以通过手机号寻找到一些开源的信息，例如定位

<figure class="wp-block-image size-large">![图片[2]-如何使用PhoneInfoga开源引擎搜集手机号信息？-FancyPig's blog](img/ae669a0f14598649ee63157c567a4237.png)</figure>

当然，最强大的是后面还内置了很多Google hacking的语句，你可以直接点击然后通过搜索引擎进行搜索

<figure class="wp-block-image size-large is-resized">![图片[3]-如何使用PhoneInfoga开源引擎搜集手机号信息？-FancyPig's blog](img/bc45e42a09d01a6d55ce83af86188b0a.png)</figure>

如果使用**PhoneInfoga**引擎，想要收集国内的手机号，记得要+86

当然，只能确定手机号的大致情况，例如运营商

<figure class="wp-block-image size-large">![图片[4]-如何使用PhoneInfoga开源引擎搜集手机号信息？-FancyPig's blog](img/e90a8c2368e6592209def6e8984dd887.png)</figure>

## 其他方式

当然，上述的教程仅针对于国外或者专业人士，下面说的方式可能更加亲民一些。