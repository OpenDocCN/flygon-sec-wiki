<!--yml
category: 社会工程
date: 2022-11-10 10:35:26
-->

# 【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog

> 来源：[https://www.iculture.cc/knowledge/pig=11951](https://www.iculture.cc/knowledge/pig=11951)

## 相关声明

本教程仅用于hvv、红蓝攻防对抗等专业领域，请勿用于非法用途。

## 相关阅读

Wireshark的功能实在强大，可以做很多事情，我们之前提供过很多篇教程

## 杂谈

今天我们延续之前的这篇，给大家补充一下

## 工具下载

## 图文教程

首先，我们打开wireshark，这里我们要注意的是

*   如果你是使用的有线连接的互联网，则使用`以太网`
*   如果你是使用的WIFI连接的互联网，则使用`Wlan`

<figure class="wp-block-image size-large">![图片[1]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/c6cf714620ce5f19f26a41e390ffcc4f.png)</figure>

我们这里属于第二种情况，所以选择WLAN

<figure class="wp-block-image size-large">![图片[2]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/fcb892bc4ddb7f33e0533c62a79a0e7a.png)</figure>

### 方法一：使用CTRL+F字符串查找

按一下ctrl F，选择分组详情、字符串、然后输入代码`020048`（这个是QQ语音对应的特征，其他的后面我们会分享）

<figure class="wp-block-image size-full">![图片[3]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/19a214c7d9f46926e16d19cf649ed155.png)</figure>

点击一下就开始抓包了，这时，我们给另一个猪猪打个电话

<figure class="wp-block-image size-full">![图片[4]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/b7d3d50dc04cbbf1800b4ad8ad023b78.png)</figure>

接通之后

<figure class="wp-block-image size-full">![图片[5]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/db424087f4d8593efd2ef0b4e3c7efbf.png)</figure>

我们点查找就可以看到对方的IP地址了

<figure class="wp-block-image size-large">![图片[6]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/5bdd8819d3efbf6431dbf6c987594aea.png)</figure>

当然，除了这种方法，还可以使用下面的方法

### 方法二：在过滤器中填写代码回车查找

在过滤器中填写`udp[8:3]==02:00:48`进行过滤，比第一种方法更加直观

<figure class="wp-block-image size-large">![图片[7]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/873b19a9c3200c09140a32d9a8ad8fa4.png)</figure>

### 为什么是020048？

那这里肯定会有人问了为什么是`020048`，QQ语音通话使用的是UDP协议直连，简单来说就是语音通话的双方直接连接，不通过其他服务器，020048是**QQ UDP**协议**72字节**的报文头

<figure class="wp-block-image size-large">![图片[8]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/e573ad8779fc1be08643ce0b7da65690.png)</figure>

同时也是为什么可以使用`udp[8:3]==02:00:48`进行过滤的原因，UDP前面包括**8字节UDP头**，**后面就是数据**，但是wireshark并没有提供**udp.data**这种直接的过滤方式，故我们使用**偏移**来实现此过滤，也就只能靠udp[8:x]这样偏移来获取（注：8是固定的八个字节），QQ前面的报文头是不会变化的，所以说我们可以通过之前说的两种方式来找到包含带有真实IP的包。

## 微信语音获取IP特征过程

如果我们不知道特征，那么怎么去尝试寻找特征呢？其实很简单，电脑连上WIFI，给另一个微信（**已知IP**）打个电话，然后开着wireshark，在过滤器上可以输入我们已知的IP地址（如果你不知道自己的IP地址，可以在百度上直接输入`本机ip`就可以快速获取了，我们这里已经获取好了，在过滤器中输入）

<figure class="wp-block-image size-full">![图片[9]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/349f5a99abd269f1be55e39e1a114baa.png)</figure>

然后，我们展开详情，看看Data里有没有同样的特征

<figure class="wp-block-image size-large">![图片[10]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/77adede694e58bf54d869041e4c6d0b4.png)</figure>

我们在这里发现，微信与QQ不同，他的报文头是**随机的值**，通过找规律发现**前两位都是a3**，根据之前说的原理，可以使用**udp[8:1]==a3**进行过滤。

### 利用data.len进行过滤

这个抓包的并没有上面的第一种方法准确（并不缺少数据，但是过滤后的**无关IP较多**），此方法参考了**台湾中央警察大学**三位研究员2020年9月在公开会议上分享的一篇论文，其中提到了根据**Length**、**Time to live**、**Flags**三个维度来来判断**嫌疑人真实IP**的技巧，根据这篇论文提供的思路，最后得出的命令为**data.len >= 120 and data.len <= 150**，也就是筛选Length长度为120到150区间的包

<figure class="wp-block-image size-large">![图片[11]-【Wireshark教程】如何通过语音获取对方IP 支持钉钉/QQ/WX-FancyPig's blog](img/18c63cca45a0caeadcc1baf2676d727b.png)</figure>

## 其他客户端的特征有吗？

微信、钉钉这些语音的特征有吗？

答案是有的，我们这里直接分享其特征：

*   钉钉：udp[8:4]==00:01:00:4c
*   QQ：udp[8:3]==02:00:48
*   微信：udp[8:1]==a3

## 参考原文