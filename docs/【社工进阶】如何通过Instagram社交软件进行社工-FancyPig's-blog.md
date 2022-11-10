<!--yml
category: 社会工程
date: 2022-11-10 10:28:20
-->

# 【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog

> 来源：[https://www.iculture.cc/sg/pig=17294](https://www.iculture.cc/sg/pig=17294)

## 杂谈

之前我们讲了很多基于**OSINT(开源情报)**框架下的内容，例如

今天我们将分享一个OSINT的工具，用于完成Instagram的社工

（如果上不了[Instagram](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cuaW5zdGFncmFtLmNvbS8=)，记得先学习一下[科学上网](https://iculture.cc/vpn)）

当然，你也可以直接租一台[vultr](http://iculture.cc/vultr)的海外服务器

## 工具安装

### 工具下载

从git上clone然后进入Osintgram目录

```
git clone https://github.com/Datalux/Osintgram.git
cd Osintgram
```

当然你也可以直接通过下面链接直接下载

### 安装依赖

```
pip3 install -r requirements.txt
```

<figure class="wp-block-image size-full">![图片[1]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/c630763abbb81e3bb6e5b85ec1919138.png)</figure>

### 配置账户

我们这里需要配置登录的Instagram的账户，在`Osintgram/config`目录下的`credentials.ini`中

```
[Credentials]
username =
password =
```

*   username = 填写账户
*   password = 填写密码

<figure class="wp-block-image size-full">![图片[2]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/2cb7d048a012f18ec97be97f593a1633.png)

<figcaption>账户密码填写示范</figcaption>

</figure>

## 工具使用

我们如果想搜索某个用户名，这里我们以`bymargotrobbie`为例

<figure class="wp-block-image size-large">![图片[3]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/7560d4b8308f1bb76549144da1dfa33a.png)</figure>

可以使用下面的命令

```
python3 main.py bymargotrobbie
```

<figure class="wp-block-image size-full">![图片[4]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/f66912f0c641e8fccf757a2d6c90f3b8.png)</figure>

### 获取目标用户地理位置

```
addrs
```

通过`addrs`命令可以从历史的推送中找到定位相关的内容，这样就可以社工到用户的地理位置！

<figure class="wp-block-image size-full">![图片[5]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/0e9a8ef11105d4a9c619e2b745fe2473.png)</figure>

### 获取TA关注的用户

我们想获取TA关注的人

<figure class="wp-block-image size-large">![图片[6]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/1f2b24f8f2aece211aa8cb1551b92b16.png)

<figcaption>目标用户关注的人</figcaption>

</figure>

可以输入下面的命令

```
followings
```

<figure class="wp-block-image size-full">![图片[7]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/51cf8a7d3354995ebc8ab5db27f3a3a9.png)</figure>

### 获取评论过TA推送的用户

```
wcommented
```

### 获取TA关注的用户邮箱地址

我们可以看到她关注了15个用户，我们通过下面命令可以快速获取她关注的人的邮箱账户信息

```
fwingsemail
```

这里我们可以看到成功找到了其中一位用户的邮箱地址

<figure class="wp-block-image size-full">![图片[8]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/2d2d006278c61861d8bd6280743b0552.png)</figure>

### 获取TA关注的用户手机号

同理，我们还可以尝试获取目标用户其关注用户的手机号

```
fwingsnumber
```

<figure class="wp-block-image size-full">![图片[9]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/5d568992fdee37426c86c565bedc52a0.png)</figure>

### 下载目标用户全部照片

```
photos
```

<figure class="wp-block-image size-full">![图片[10]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/1b02302947d3f31813e06f86be31e7c4.png)</figure>

我们这里显示下载了209张图片

<figure class="wp-block-image size-full">![图片[11]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/d1b76dfc7a402b2d65b289f586231feb.png)</figure>

下载后会存到`output`文件夹中

<figure class="wp-block-image size-large">![图片[12]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/6c5c31e955f8d5edc8bb095631d77eb5.png)

<figcaption>output文件夹</figcaption>

</figure>

下载的图片均来自Instagram的用户相册

<figure class="wp-block-image size-large">![图片[13]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/7eda8f21992209310377e8b40c5bc0b5.png)

<figcaption>Instagram相册</figcaption>

</figure>

### 导出功能

上面是在社工中常见的方式，当然，我们除了获取TA关注的人的资料，还可以获取TA粉丝的资料，但是一般明星的粉丝都会有很多，因此我们这时往往还需要配合导出文件的功能！

譬如，我们想获取TA的粉丝邮箱地址资料

```
fwersemail
```

但是这里我们会发现TA的粉丝太多了，邮箱都上万个了，肯定一次没法在终端完全展示

<figure class="wp-block-image size-full">![图片[14]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/6384b764a67b0b7047138eed4b8795a9.png)</figure>

那么这时，我们可以先输入下面的命令，然后在输入我们想导出的具体命令

*   FILE=y 导出txt格式
*   JSON=y 导出json格式

这里我们尝试导出TXT文件，我们先输入

```
FILE=y
```

<figure class="wp-block-image size-full">![图片[15]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/6b92f16bb141c53b48fb715c00e0deca.png)</figure>

然后在输入

```
fwersemail
```

<figure class="wp-block-image size-full">![图片[16]-【社工进阶】如何通过Instagram社交软件进行社工-FancyPig's blog](img/9020c6ee78f5f2ce0864a3b11f2270c6.png)</figure>

最终就可以在`output`中找到导出结果了