<!--yml
category: 社会工程
date: 2022-11-10 10:30:24
-->

# 王者荣耀S23赛季礼包仿真钓鱼-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=315](https://www.iculture.cc/sg/pig=315)

# 王者荣耀S23赛季礼包仿真钓鱼

## 杂谈

很多朋友最近钓鱼钓上瘾了，感觉还没玩够，跟我说有没有可以拿到账户密码的，我想了想，就搞了个王者的礼包领取的仿真页面，后面有空再搞个内测申请的界面吧，大家想尝试社工密码的建议自己申请一个跟王者荣耀官网类似的域名，这样仿真效果更佳。

## 声明

请不要拿下面的页面去钓鱼，仅用于仿真钓鱼社工教学

## 介绍

下面是仿真页面，共分为两个入口

![image.png](img/ea67114ae80ad6b2e999f6af488419bf.png "image.png")
两个入口点击之后均会跳转到登录页
![image.png](img/970073faba747c80268299d9c529bfa1.png "image.png")
然后输入完账户和密码，我们可以在公共的页面获取密码，由于时间仓促，所有社工收集的密码都可以在这里查看
![image.png](img/fd99a34d33f3a220ce881a7a9ac5a9dc.png "image.png")
因此不建议你们直接使用，建议自己部署。

## 体验

评论获取体验链接

### 仿真页面入口

S23赛季最新活动仿真页面入口
[https://www.iculture.cc/demo/pvpqq/](https://www.iculture.cc/demo/pvpqq/)

老活动：阿古朵礼包仿真页面入口
[https://www.iculture.cc/demo/pvpqq/index_demo.php](https://www.iculture.cc/demo/pvpqq/index_demo.php)

### 查看密码页面

## 部署说明

### 创建并导入数据库

创建好数据库，将`导入数据库.sql`导入到mysql数据库中

### 配置数据库文件

需要修改根目录下和admin目录下的`text.php`文件
![image.png](img/741db749b59b7915d612bf4b4c78171c.png "image.png")
![image.png](img/829af3c8405ef6c96f212f86645da40c.png "image.png")
配置好数据库名和密码