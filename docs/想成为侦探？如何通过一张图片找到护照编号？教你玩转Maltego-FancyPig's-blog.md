<!--yml
category: 社会工程
date: 2022-11-10 10:29:01
-->

# 想成为侦探？如何通过一张图片找到护照编号？教你玩转Maltego-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=6891](https://www.iculture.cc/sg/pig=6891)

## 声明

以下内容仅为技术科普，使用内容均来自OSINT（开源情报），不涉及敏感数据。

本文从未隐讳任何个人、群体、公司。非文学作品，请勿用于阅读理解的练习。

## 简介

**OSINT**(Open-source Intelligence)开源情报是任何调查人员或白帽黑客的必备工具。

今天，我们将只从一张**未知主题的照片**开始，并将 **OSINT 工具**串起来，最后我们将在美国制裁名单上找到它们。

## 视频介绍

### 关键时间节点

0:00 倒计时
0:48 介绍
9:05 从一个图像开始
13:25 Pimeyes面部识别搜索
19:00 TinEye图像搜索
21:17 Maltego
25:07 从实体和变换开始
29:20 Aleph搜索
36:00 寻找护照
40:29 寻找外交文件
42:21 结束感想

### 视频教程

评论获取视频完整版

## 工具汇总

以下部分功能如果无法直接访问，请您科学上网

### 在线工具

#### Google浏览器插件 Who stole my pictures

[https://chrome.google.com/webstore/detail/who-stole-my-pictures/mcdbnfhkikiofkkicppioekloflmaibd/related?hl=zh-CN](https://chrome.google.com/webstore/detail/who-stole-my-pictures/mcdbnfhkikiofkkicppioekloflmaibd/related?hl=zh-CN)

<figure class="wp-block-image size-large">![图片[1]-想成为侦探？如何通过一张图片找到护照编号？教你玩转Maltego-FancyPig's blog](img/2c336ab77ededa6d9c06b9a8fcf2178e.png)</figure>

您只需要找到一张图片，然后右键，可以选择图片搜索引擎进行快速搜索，匹配出相似的图片（详细使用方法评论获取）

Pimeyes主要是针对面部识别的，之前可以白嫖免费的，目前收费了，后面我们会出破解使用的解决方案，敬请期待

#### pimeyes

<figure class="wp-block-image size-large">![图片[2]-想成为侦探？如何通过一张图片找到护照编号？教你玩转Maltego-FancyPig's blog](img/8d5be56ae25ad011ffde1700aa25a284.png)</figure>

由于pimeyes中每日免费10次的体验已经被开发方移除，因此视频讲解中的破解使用上述工具方法暂已失效，等待后续补充方法。不过u1s1，功能是真心强大，可以搜索到**博客**、**社区论坛**、**官网**，甚至还可以搜索到**犯罪在逃等通缉**记录，属实有些强大、同时也令人毛骨悚然！

下面图片中提取的面部为了保护个人隐私，已经打码。

<figure class="wp-block-image size-large">![图片[3]-想成为侦探？如何通过一张图片找到护照编号？教你玩转Maltego-FancyPig's blog](img/462c98dc16ea023c7bcd5012dc509d20.png)</figure>

### Tineye

[https://tineye.com/](https://www.iculture.cc/?golink=aHR0cHM6Ly90aW5leWUuY29tLw==)

<figure class="wp-block-image size-large">![图片[4]-想成为侦探？如何通过一张图片找到护照编号？教你玩转Maltego-FancyPig's blog](img/507f4bcf55ea04356d053210812ac87d.png)</figure>

### 本地工具

#### Maltego

Maltego社区版在Kali linux里自带，如果不会安装kali linux可以参考之前的文章