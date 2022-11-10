<!--yml
category: 社会工程
date: 2022-11-10 10:28:32
-->

# 如何通过QQ看点获取QQ号-FancyPig's blog

> 来源：[https://www.iculture.cc/knowledge/pig=11549](https://www.iculture.cc/knowledge/pig=11549)

## 杂谈

最近有热心网友在群里提问，能不能在QQ看点上通过主页找到作者的QQ号呢？

之前最原始的base64解密方法已经失效了，不过还是有新的方法，甚至不需要解密😎😎😎

## 相关阅读

## 图文教程

下面的教程我们将给大家提供两种方式，第一种比较原始，第二种比较简易，选择适合你的方式！

首先，我们先找一个QQ看点的视频，emmmm，这个好像是沫子

<figure class="wp-block-image size-full is-resized">![图片[1]-如何通过QQ看点获取QQ号-FancyPig's blog](img/0a8b4e2b7c61254dcbd985eb3b095a76.png)</figure>

打开之后，我们点击沫子的头像进入主页

<figure class="wp-block-image size-full">![图片[2]-如何通过QQ看点获取QQ号-FancyPig's blog](img/2c6810b036ae756e02d019f65df513ae.png)</figure>

点击右上角**···**

<figure class="wp-block-image size-full">![图片[3]-如何通过QQ看点获取QQ号-FancyPig's blog](img/578a28e7ed55c729a1d65abda9a7abf5.png)</figure>

点击复制链接

<figure class="wp-block-image size-full">![图片[4]-如何通过QQ看点获取QQ号-FancyPig's blog](img/76303832346b8f9d4c66aad43ce18990.png)</figure>

复制链接后得到下面的内容

```
https://kandian.qq.com/mqq/vue/main?_wv=10145&_bid=3302&adfrom=qqshare&x5PreFetch=1&sourcefrom=6&secUin=ABGKeNaYmpGAyxwi811feC8EiA&adtag=qqshare&t=1647305394903&viola_share_url=http://viola.qq.com/js/profileV2.js?_rij_violaUrl=1&v_bid=3740&v_tid=6&v_bundleName=profileV2&hideNav=1&v_nav_immer=1&accountId=&secUin=ABGKeNaYmpGAyxwi811feC8EiA&iid=&iid=&sourcefrom=6
```

接下来如何使用这个链接呢？

### 方法一

我们分析提取`secUin=`后面到`&iid`之前的内容

<figure class="wp-block-image size-full">![图片[5]-如何通过QQ看点获取QQ号-FancyPig's blog](img/cb7005c8bd241c0c15e549be611ad9ef.png)</figure>

这里是

```
ABGKeNaYmpGAyxwi811feC8EiA
```

然后使用我们抓包时候找到的接口

```
https://kandian.qq.com/cgi-bin/social/getHomePage?uin=&pageSize=8&pageCookies=&isInQQ=0&tabid=0&version=0&clientType=2&secUin=
```

将`ABGKeNaYmpGAyxwi811feC8EiA`填到上面的`secUin=`后面，则得到

```
https://kandian.qq.com/cgi-bin/social/getHomePage?uin=&pageSize=8&pageCookies=&isInQQ=0&tabid=0&version=0&clientType=2&secUin=ABGKeNaYmpGAyxwi811feC8EiA
```

然后在`third_uin`中就可以看到沫子的QQ号了欸

<figure class="wp-block-image size-full">![图片[6]-如何通过QQ看点获取QQ号-FancyPig's blog](img/084e5b0ccc2af38ae7c1febfa91d096a.png)</figure>

然后我们去查找验证下，发现是正确的欸😊😊😊

<figure class="wp-block-image size-full">![图片[7]-如何通过QQ看点获取QQ号-FancyPig's blog](img/58898aff4d3f2a609682acf733e687f0.png)</figure>

### 方法二

很多热心网友表示方法一有点复杂，那我们来点简单易操作的，我们之前分享的氧化氢工具箱里面其实自带的是有的，当时是在讲IP溯源的时候提到过这个工具的

进入工具箱页面，将链接粘贴在输入框里

<figure class="wp-block-image size-large">![图片[8]-如何通过QQ看点获取QQ号-FancyPig's blog](img/ab930c34f726f04d3d23242e1776bc99.png)</figure>

然后点击查询，就可以看到结果了，这里可以一键添加好友，还是蛮香的

<figure class="wp-block-image size-large">![图片[9]-如何通过QQ看点获取QQ号-FancyPig's blog](img/1d330942b88cb1539edcdf89b492908a.png)</figure>