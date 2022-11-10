<!--yml
category: 社会工程
date: 2022-11-10 10:29:35
-->

# 【视频教学】如何远程控制任何安卓设备？-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=2373](https://www.iculture.cc/sg/pig=2373)

## 声明

本教程仅用于远程设备管理的教学，请勿用于非法用途

## 前言

之前出过一篇远程控制的文章，但仅针对于会员用户

今天出一篇大家都可以看的，并且还配了视频教学，记得点赞哦！

## 视频教学

本视频来自Youtube大佬`Loi Liang Yang`

视频中使用的Kali Linux安方法，可以参考

## 项目功能

**原标题：一个基于云端的远程安卓管理套件**

可以通过云端管理以下资料

*   手机短信
*   通话记录、通话时长
*   手机系统的文件
*   手机安装的程序
*   GPS定位
*   浏览器页面访问记录

## 项目下载

评论获取项目下载地址

## 项目部署

使用`git clone`本项目

```
git clone https://github.com/D3VL/L3MON
```

<figure class="wp-block-image size-large">![图片[1]-【视频教学】如何远程控制任何安卓设备？-FancyPig's blog](img/d1b79991c024b0d0c21e8d401139085a.png)</figure>

需要安装`pm2`依赖

```
npm install pm2 -g
npm install 
```

然后启动本项目

```
pm2 start index.js
pm2 startup
```

配置`maindb.json`

```
{
	"admin":{
		"username":"admin",
		"password":"25d55ad283aa400af464c76d713c07ad",
		"loginToken":"",
		"logs":[],
		"iplog":[]

},	
	"clients":[]

}
```

里面的密码需要自行生成小写的MD5格式的填入，如果不会生成请参考。

## 项目的使用

在浏览器里通过`http://[你的IP地址]:22533`访问

<figure class="wp-block-image size-large">![图片[2]-【视频教学】如何远程控制任何安卓设备？-FancyPig's blog](img/195a923ffaeaf07de0ec81e94bf9659d.png)</figure>

之后进行打包，然后把打包完成的apk文件发给用户就行了，至于怎么社工就是你的事情了。

<figure class="wp-block-image size-large">![图片[3]-【视频教学】如何远程控制任何安卓设备？-FancyPig's blog](img/111f6438786cb790c3848a8c888a5548.png)</figure>

之后你就可以看到定位、通话记录和时长、短信、用户的文件夹、用户安装了哪些应用、访问了哪些网页

<figure class="wp-block-image size-large">![图片[4]-【视频教学】如何远程控制任何安卓设备？-FancyPig's blog](img/d436793ea58a1fd85bbc3a6e6e41b4fe.png)</figure>

<figure class="wp-block-image size-large">![图片[5]-【视频教学】如何远程控制任何安卓设备？-FancyPig's blog](img/e7a8301818be0c7df098a2dd241eff54.png)</figure>