<!--yml
category: 社会工程
date: 2022-11-10 10:28:12
-->

# 如何使用OSINT工具调查邮箱账户-FancyPig's blog

> 来源：[https://www.iculture.cc/knowledge/pig=19243](https://www.iculture.cc/knowledge/pig=19243)

## 杂谈

我们之前介绍过OSINT工具，通过开源情报、公网中暴露的信息进行调查、溯源，本文依旧是分享OSINT工具的使用方法，我们尝试通过该工具查看我们的邮箱账户是否在公网中泄露！

## 相关阅读

## Mosint

### 准备工作

需要先给自己的Linux系统安装`git`命令

#### CentOS

如果你是CentOS环境，您可以输入下面的命令

```
sudo yum install git
```

#### Debian/Ubuntu

```
sudo apt install git
```

### Mosint工具的使用

#### 下载git仓库

```
git clone https://github.com/alpkeskin/mosint.git
```

<figure class="wp-block-image size-full">![图片[1]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/bc04a71834ca2a4fcaae223e0ba8728f.png)</figure>

#### 安装依赖

我们输入下面的命令进入mosint目录

```
cd mosint
```

<figure class="wp-block-image size-full">![图片[2]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/92299b8eb672812fc8cc2504179de6ac.png)</figure>

安装依赖

```
pip3 install -r requirements.txt
```

<figure class="wp-block-image size-full">![图片[3]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/a4fe73654fecde7a36b470d2c0ee5fa1.png)</figure>

### 工具集成开源引擎

<figure class="wp-block-table">

您需要去相关网站注册账户，并获取APIKEY，然后填入`key.json`文件

```
{
  "BreachDirectory.org API Key": "",
  "Hunter.io API Key": "",
  "EmailRep.io API Key": "",
  "Intelx.io API Key": ""
}
```

<figure class="wp-block-image size-full">![图片[4]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/d7b4b297d0bd6a34b34ff27e48843936.png)</figure>

EmailRep申请是需要24小时之内才能获取到APIKEY的，我这里还没有拿到，故先填入其他的三个！

<figure class="wp-block-image size-full">![图片[5]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/dac560d9dd4bca443b6babb18209562a.png)</figure>

首先我们进入[hunter.io](https://www.iculture.cc/?golink=aHR0cHM6Ly9odW50ZXIuaW8v)

在右上角点击Dashboard

<figure class="wp-block-image size-large">![图片[6]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/43b6433f65c9d47af91f59cdef9ed2e6.png)</figure>

选择China，输入自己的手机号进行注册

之后你可以看到右侧有一个API

<figure class="wp-block-image size-full">![图片[7]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/8fc25cb7f01e58b67ac1f7138af0fb5e.png)</figure>

点击复制即可

<figure class="wp-block-image size-large">![图片[8]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/522c17d09f20172b8c26ad2220b603d0.png)</figure>

首先需要注册账户

[https://intelx.io/signup](https://www.iculture.cc/?golink=aHR0cHM6Ly9pbnRlbHguaW8vc2lnbnVw)

填写邮箱账户、要注册的密码等信息，点击Sign up

<figure class="wp-block-image size-large">![图片[12]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/96f9264e6dee85c8d6c04cf5f38cca3c.png)</figure>

完成注册，需要去邮箱验证

<figure class="wp-block-image size-large">![图片[13]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/28670ee61ebdb104b7878ae9ca424c9d.png)</figure>

点击验证链接

<figure class="wp-block-image size-full">![图片[14]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/5b400ec35f39f58fe5a32bdd07ef9d70.png)</figure>

完成验证

<figure class="wp-block-image size-full">![图片[15]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/f603e994f694318a94090967eaafbc04.png)</figure>

输入邮箱账户和密码，点击Sign in登录

<figure class="wp-block-image size-large">![图片[16]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/f30e3933c116183852e2d718a252988c.png)</figure>

点击右上角Account

<figure class="wp-block-image size-large">![图片[17]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/1382f0cdd596498b8259fe0727f087b5.png)</figure>

点击Developer，然后就可以看到我们的APIKEY了

<figure class="wp-block-image size-large">![图片[18]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/1f7ff77e7cf615f2b1dccae9d0b782f5.png)</figure>

### 邮箱账户收集

比方说我们想查询下我的QQ邮箱`663962@qq.com`

#### 查询全部资料

```
go run main.go -e 663962@qq.com -all
```

#### 查询是否有数据泄露

密码泄露会进行脱敏

```
go run main.go -e 663962@qq.com -leaks
```

这里可以看到之前我的密码用过**6745******😂

<figure class="wp-block-image size-full">![图片[23]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/a9cb151bc83309994a24ebab4f61c886.png)</figure>

其实我们之前还介绍了很多类似的工具在社区

《[【小技巧】教您查询自己的账户密码是否泄露过](https://www.iculture.cc/forum-post/10643.html)》

### 查询社交平台注册情况

```
go run main.go -e 663962@qq.com -social
```

<figure class="wp-block-image size-full">![图片[24]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/0fd142e15075a70b31b035ebf8078a23.png)</figure>

### 查询关联域名

```
go run main.go -e 663962@qq.com -domain
```

<figure class="wp-block-image size-full">![图片[25]-如何使用OSINT工具调查邮箱账户-FancyPig's blog](img/3855f979eb529dcbb64255911ee86d78.png)</figure>

## 更多工具

针对企业我们还可以使用一些收集的平台

</figure>