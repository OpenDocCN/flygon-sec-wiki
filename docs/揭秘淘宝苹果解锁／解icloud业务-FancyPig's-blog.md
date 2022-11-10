<!--yml
category: 社会工程
date: 2022-11-10 10:30:16
-->

# 揭秘淘宝苹果解锁/解icloud业务-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=698](https://www.iculture.cc/sg/pig=698)

## 杂谈

前一阵子有朋友问我如果捡到了带锁的苹果手机，应该怎么办？首先，就是想办法找到丢失的人，坏给失主。开着手机等失主打电话，大部分情况都能还回去，然后接收一张好人卡。当然，如果你真的很需要用它，但又不想让它变成板砖，可以考虑通过解锁/解icloud的方式或者就暴力一点换个主板。

这里偶然看到有人在淘宝上卖类似的业务，就很好奇的去问了问

<figure class="wp-block-image size-large">![图片[1]-揭秘淘宝苹果解锁/解icloud业务-FancyPig's blog](img/cb30efc7c7d5a5a561d7e9c3905d75d1.png)</figure>

## 淘宝开锁流程

商家一般会发一张图让你确认你手机目前的状况：密码输入次数过多锁了？还是可以打开需要改icloud账户？还是重装了系统需要验证icloud账户？还是正常的开屏界面可以输入密码进行解锁？

然后他还会让你提供IMEI串码去查看你手机目前的系统以及定位是否开启，不同情况可能有不同的处理策略和风险。

基本上商家会承诺你大概7-15天之内尝试帮你开锁，大致的价格会依据你手机的锁定状况来判断

这里就以手机在锁屏的状态为例

<figure class="wp-block-image size-large">![图片[2]-揭秘淘宝苹果解锁/解icloud业务-FancyPig's blog](img/f5e5068034454134746e0691c2cdb67c.png)</figure>

很多人一开始会尝试去搜索有没有方法能开屏锁，在google里可以搜一下apple screen unlocker，会给你提供不同的软件

<figure class="wp-block-image size-large">![图片[3]-揭秘淘宝苹果解锁/解icloud业务-FancyPig's blog](img/bcf2bb87d24df80b1d66bed0642ca3ae.png)</figure>

但实际上，它的操作流程就跟ITOOLS这些软件重装系统一样，他只是重新装了系统，重装完系统，苹果手机还是会要求你输入苹果icloud账户来进行激活，那么问题来了？商家是怎么进行苹果icloud账户激活的呢？

### 绕过ICLOUD

第一种就是越狱，然后绕过ICLOUD，这种方法很多小白都会上当受骗，使用这种绕过ICLOUD的方法可能会产生的后果

*   不能通过APPSTORE下载正版应用
*   手机不能打电话，手机成了ITOUCH游戏机了
*   手机不能联网，只能连WIFI游戏机用了
*   手机不能关机，关机再开机就成砖了
*   手机不能初始化、升级，点了也变成砖

### 卡贴机

很多人会尝试在电话卡背面贴上一张卡，通过某种手段来实现可以在有锁的情况下进行电话、网络通讯，但是缺点是很不稳定

### 钓鱼社工大法

最后一种办法可以说是绝大部分卖家都是通过这种方式，他们会向你承诺打不开全额退款，这种方式是毫无成本的，当然可以全额退款了。

这里给大家详细讲解一下具体的操作流程，在重装完系统后开机的时候一般我们可以看到激活icloud的账户邮箱，淘宝商家会给这个邮箱发一封钓鱼的邮件

<figure class="wp-block-image size-large">![图片[4]-揭秘淘宝苹果解锁/解icloud业务-FancyPig's blog](img/49e9c56bb7ff4a00375231b78e451603.png)</figure>

然后利用失主着急的心理，一般失主都会打开钓鱼链接，然后输入自己的账户名和密码，然后淘宝商家通过失主输入的账户名和密码帮助用户进行解锁，最终白嫖100%收益。

## 钓鱼体验

这里给大家送个小福利，猪头也仿照苹果的登陆界面写了个页面，可以进行页面钓鱼，可以体验一下

[https://www.iculture.cc/demo_test/icloud/](https://www.iculture.cc/demo_test/icloud/)

## 查看密码

## 下载源码