<!--yml
category: 社会工程
date: 2022-11-10 10:29:28
-->

# hvv钓鱼中常用的域名混淆方法-FancyPig's blog

> 来源：[https://www.iculture.cc/cybersecurity/pig=3020](https://www.iculture.cc/cybersecurity/pig=3020)

## 介绍

在hvv中，我们常常需要使用社工的手段配合钓鱼链接，达到一些特殊的攻击效果。

## 注册与官方类似的域名

去搜索与官方类似的域名，比方说将字母和数字进行混淆

### 字母、数字之间的替换

比方说微软一款产品叫做`OneNote`，我们可以把字母O换成数字0，来完成混淆

<figure class="wp-block-image size-large">![图片[1]-hvv钓鱼中常用的域名混淆方法-FancyPig's blog](img/e28658c1c26ed8733bf05db565320695.png)</figure>

### 包含产品名称

比方说腾讯的`王者荣耀`官网是[pvp.qq.com](http://pvp.qq.com)

我们可以注册与之类似的域名

<figure class="wp-block-image size-large">![图片[2]-hvv钓鱼中常用的域名混淆方法-FancyPig's blog](img/f732b05a54695de00033d5d1e87fe4e9.png)</figure>

或者我们可以把产品名称和单位结合在一起的方式，人们下意识不会产生反感

<figure class="wp-block-image size-large">![图片[3]-hvv钓鱼中常用的域名混淆方法-FancyPig's blog](img/3dae1f98c248d013469326127cb8237f.png)</figure>

## 使用二级、三级域名

我们还可以通过解析二级、三级域名的方式，把官方的域名包含在我们的域名之前

比方说组合一个这样的域名出来

[qq.com.iculture.cc](http://qq.com.iculture.cc)

我们需要去域名解析后台设置主机记录

<figure class="wp-block-image size-full">![图片[4]-hvv钓鱼中常用的域名混淆方法-FancyPig's blog](img/74e17cbe6155da80ec5f9037a0f57442.png)</figure>

当然，这个还不是最骚的，还可以这样！

[www.qq.com.iculture.cc](http://www.qq.com.iculture.cc)

<figure class="wp-block-image size-full">![图片[5]-hvv钓鱼中常用的域名混淆方法-FancyPig's blog](img/2a58f2a383e9a1972ad83d403bebcc17.png)</figure>

通过这样的方式，一定情况下还可以绕过防火墙的基础判断，同时还达到了混淆，而且不需要花一分钱！