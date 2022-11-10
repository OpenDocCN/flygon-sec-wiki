<!--yml
category: 社会工程
date: 2022-11-10 10:35:06
-->

# 如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog

> 来源：[https://www.iculture.cc/cybersecurity/pig=2618](https://www.iculture.cc/cybersecurity/pig=2618)

<figure class="wp-block-image size-full">![图片[1]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/b9d2e714beb2777bcb5bdc04d70352a1.png)</figure>

## 简介

本教程提供了多种思路以及实用的工具及教程，帮助大家隐藏自己的真实IP地址，更好的保护大家的隐私。针对不同的应用场景，本文主要提供了3种思路

*   使用VPN代理（通用全部场景）
*   使用Tor浏览器（仅限于浏览器冲浪）
*   使用Ngrok隧道转发（仅限于hvv中部分场景）

## 使用VPN代理

该方法适用于全部场景，可以使用VPN进行代理，要使用`全局模式`才可以，使用方法具体操作参考下面的文章

我们为大家准备了适用各种类型的**若干免费节点**，评论即可获取

## 使用Tor浏览器

针对**浏览网页**这个特殊场景，我们可以使用`Tor浏览器`来避免溯源到真实IP地址。**此教程无需使用VPN代理。**

### 相关资料

之前我们有讲过使用暗网时会用到Tor浏览器，相关资料如下

### 快捷入口

### Windows配置教程

安装完成后打开Tor浏览器，选择`Tor Network Settings`

<figure class="wp-block-image size-full">![图片[2]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/ddc30d57f472f453b4748ea0bfcd8318.png)</figure>

这里选择`meek-azure`

<figure class="wp-block-image size-full">![图片[3]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/9296270e6a2710ba2cf8b03e0bf63398.png)</figure>

然后等待再等待，这里已经连上了。

<figure class="wp-block-image size-full">![图片[4]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/081643ce46585f851a8029a29f04c25a.png)</figure>

这里我们尝试访问[FancyPig官网](https://www.iculture.cc)，可以看到节点从本地跳转到了希腊的节点，然后又跳转到了荷兰的节点，最终访问我们的网站

<figure class="wp-block-image size-full">![图片[5]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/94b86b639d0f8fc3f732f684d35b5332.png)</figure>

而且每一次链路都是不一样的，这样很大程度上保护了我们的隐私。

<figure class="wp-block-image size-full">![图片[6]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/c6ac7d304a960f1fcc5764875b27a069.png)</figure>

## 使用Ngrok隧道转发

该方法一般配合`Cobalt Strike`一起使用，可以一定程度上隐藏C2真实IP

### 快捷入口

### 使用教程

在`隧道管理`里选择`开通隧道`，找到免费的`美国Ngrok免费服务器`

<figure class="wp-block-image size-large">![图片[7]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/a77aa75ec4bc3eaa318de3635cce33ff.png)</figure>

选择`tcp`隧道

<figure class="wp-block-image size-large">![图片[8]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/f4052907fe79fb7455d8bae142af1481.png)</figure>

选择`客户端下载`

<figure class="wp-block-image size-large">![图片[9]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/52c59bc200cecf42583188deb33e5a37.png)</figure>

我这里是[windows](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cubmdyb2suY2Mvc3Vubnkvd2luZG93c19hbWQ2NC56aXA/dj0yLjE=)的版本，下载后解压缩，运行`Sunny-Ngrok启动工具.bat`

<figure class="wp-block-image size-full">![图片[10]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/542268d576a1f30b78f52bf8fb617478.png)</figure>

启动后，需要输入客户端id

<figure class="wp-block-image size-full">![图片[11]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/70005ce846f795b41a8185d021431a75.png)</figure>

<figure class="wp-block-image size-large">![图片[12]-如何隐藏自己的真实IP地址 防止被溯源/恶意钓鱼-FancyPig's blog](img/315b1edad77f2a12472f4ada631a7840.png)</figure>