<!--yml
category: 社会工程
date: 2022-11-10 10:30:06
-->

# 【小技巧分享】如何通过微博图片进行社工Po主-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=740](https://www.iculture.cc/sg/pig=740)

猪头在审核完这篇文章之后，部署了一个简单的微博图片查Po主的工具

# 原理

刚刚在某篇博客上看到这个，通过微博图片链接可以直接找到发图的人

我实验了一下，还可以用，我微博去首页随机找了一张图，链接是这个

```
https://wx1.sinaimg.cn/mw690/548495d9gy1gpyj7gk9htj20u03wi1if.jpg
```

我们提取这个文件名的前八位`548495d9`从16进制转化为10进制，得到的结果是`1417975257`

<figure class="wp-block-image size-large">![图片[1]-【小技巧分享】如何通过微博图片进行社工Po主-FancyPig's blog](img/f975a00627e5a70bff8d71684fc3896d.png)</figure>

然后我再把这个作为uid拿回微博测试，完全正确，就是这个uid为`1417975257`的人发的

## 互动话题

大家来看看这张图片出自谁的微博

<figure class="wp-block-image size-large">![图片[2]-【小技巧分享】如何通过微博图片进行社工Po主-FancyPig's blog](img/241752d47b4f13fc492524af005bef4e.png)</figure>

## 在线图片反查微博地址工具

### 使用方法

### 源码下载

## 其他工具分享

### 在线进制转换工具

### 浏览器扩展插件

如果你很懒，可以考虑使用这些插件