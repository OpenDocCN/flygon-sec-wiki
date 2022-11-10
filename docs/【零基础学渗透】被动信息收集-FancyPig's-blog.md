<!--yml
category: 社会工程
date: 2022-11-10 10:34:57
-->

# 【零基础学渗透】被动信息收集-FancyPig's blog

> 来源：[https://www.iculture.cc/cybersecurity/pig=11003](https://www.iculture.cc/cybersecurity/pig=11003)

停更已久的[《零基础学渗透》](https://www.iculture.cc/category/cybersecurity/seclearn)系列，今天开始继续更新了

<figure class="wp-block-image size-full">![图片[1]-【零基础学渗透】被动信息收集-FancyPig's blog](img/8e5026b509c27d896d3311866fbd0e04.png)</figure>

更多网络空间安全学习资料，您也可以在我们的社区里找到

## 相关声明

以下内容仅限于教育目的，请勿用于非法用途。

被动信息收集通常不与目标发生交互，这意味着目标将无法发现我们正在调查TA

## 学习目标

1.明确目标，要**收集什么信息**

2.掌握常用的**信息收集工具**和**第三方服务**

3.归纳总结自己的**信息收集思路**

网友绘制了一张社工思维导图，可以参考

<figure class="wp-block-image size-full">![图片[2]-【零基础学渗透】被动信息收集-FancyPig's blog](img/4e2d9c5ec3533e6328bc5289f1490ef7.png)</figure>

虽然感觉比我之前绘制的要烂的很多，不过也有一定参考价值😂😂😂

首先，我们分享一下信息收集的常见内容

*   1.IP地址段
*   2.域名信息
*   3.邮件地址
*   4.文档图片信息
*   5.公司地址
*   6.公司组织架构
*   7.联系电话
*   8.人员姓名/职务
*   9.目标系统使用的技术架构
*   10.公开的商业信息

信息的用途

*   1.用信息描述目标
*   2.发现
*   3.社会工程学攻击
*   4.物理缺口

## 学习内容

1.信息收集内容、信息用途、信息收集DNS、DNS信息收集-NSLOOKUP

2.DNS信息收集

3.DNS区域传输、DNS字典爆破、DNS注册信息

4.搜索引擎、各种互联网资产搜索引擎（shodan、fofa、zoomeye等）

5.google搜索：实例

6.域名的历史解析记录

**信息收集**是一个比较庞大的工程，而且比较有趣，因此，我们在下面会详细展开说明，并附上在线视频教程，欢迎大家观看学习。

## 在线学习视频

### 被动信息收集

1.利用第三方服务对目标进行被动信息收集防止被发现

请参考下面视频中的第6-13节内容

*   6【第3章 被动信息收集】1-被动信息收集概述
*   7【第3章 被动信息收集】2-DNS域名解析原理
*   8【第3章 被动信息收集】3-DNS信息收集
*   9【第3章 被动信息收集】4-查询网站的域名注册信息和备案信息
*   10【第3章 被动信息收集】5-使用Maltego收集子域名信息
*   11【第3章 被动信息收集】6-使用Shodan暗黑谷歌搜索引擎收集信息
*   12【第3章 被动信息收集】7-Google搜索引擎使用技巧
*   13【第3章 被动信息收集】8-常见最新漏洞公布网站

### 信息收集与漏洞挖掘

以下视频包含

*   搜索引擎的利用
*   信息收集专题概括
*   信息收集与情报挖掘——如何收集骗子信息
*   社交网站的信息挖掘
*   钓鱼网站搭建前的准备
*   网络定位IP的常见方法

3.敏感信息收集

4.信息采集

5.大型目标渗透测试

## 图文资料

### DNS注册信息

常见方式：

*   nslookup/dig
*   dns域名爆破
*   recon-ng(信息收集)

请在终端中尝试`nslookup`、`dig`等命令，详细命令以及各种工具用法，请评论获取

### IP、MAC地址

IP、MAC的基础知识和常见自我保护手段

**IP地址的唯一性：**

网络上所有的设备都必须有一个独一无二的IP地址，就好比是邮件上都必须注明收件人地址，邮递员才能将邮件送到。

同理，每个IP信息包都必须包含有目的设备的IP地址，信息包才可以正确地送到目的地。同一设备不可以拥有多个IP地址，所有使用IP的网络设备至少有一个唯一的IP地址。

**IP地址的格式和分段：**

在计算机二进制中，1个字节 = 8位 = 8bit（比特）

IP地址(IPv4)由32位二进制数组成，分为4段（4个字节）

每一段为8位二进制数（1个字节）

每一段8位二进制，中间使用英文的标点符号“.”隔开

由于二进制数太长，为了便于记忆和识别，把每一段8位二进制数转成十进制，大小为0至255。

IP地址表示为：xxx.xxx.xxx.xxx

210.21.196.6就是一个IP地址的表示。

**IP地址的组成：**

IP地址包括网络地址和主机地址两个部分。

比如：210.21.196.6

其中210.21.196 是网络地址

6 是主机地址

同一网段内（近似理解为同一个机房内）的计算机网络地址相同，主机地址不会同时重复出现（因为主机是唯一的）。通过设置网络地址和主机地址，在互相连接的整个网络中保证每台主机的IP地址不会互相重叠，即IP地址具有了唯一性。

**MAC地址：**

称为物理地址，也叫硬件地址，用来定义网络设备的位置，MAC地址是网卡出厂时设定的，是固定的（但可以通过在设备管理器中或注册表等方式修改（常用于伪装自己的身份，往往一个网络硬件被生产出来，制造商是有MAC地址记录留存的），同一网段内的MAC地址必须唯一）。MAC地址采用十六进制数表示，长度是6个字节（48位），分为前24位和后24位。

**IP隐藏技术：**

SSR、VPN、SSH、S5、Proxifier等代理工具的使用，互联网不是法外之地，7层代理也可查，只不过是查你付出的成本高低而已。

使用方法百度即可，这里不多说了，命要紧。

**加密聊天工具：**

signal、小飞机等不需要手机号注册的匿名聊天APP，需要注册的也有绕过方式，代购手机卡、网络手机号等。

使用方法百度即可，这里不多说了，命要紧。

**指纹清理、PC使用习惯、自我保护：**

常见软件都会搜集你的MAC地址，将某台上网设备和你本人的实名信息绑定，所以，工作和私人要完全分开，避免留下相关指纹信息。

linux下通过macchanger命令可更改mac地址：

具体百度一看就知道了，再写入kali的开机自启，每次开机的mac就都不同了。

浏览器的选择，规避国产浏览器和外国浏览器的国内版，建议用bing国际版搜索火狐浏览器官网，下载安装。

比方：你在百度里搜索的火狐浏览器，基本都是这个网址：

[https://www.firefox.com.cn/](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cuZmlyZWZveC5jb20uY24v)

页面拉到最下面是这样的

<figure class="wp-block-image size-large">![图片[3]-【零基础学渗透】被动信息收集-FancyPig's blog](img/0a3757c06c05e8de5d03f8cd78db4d6d.png)</figure>

然而真正的官网应该是：

[https://www.mozilla.org](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cubW96aWxsYS5vcmc=)

然而就算你从这个链接点进去，也会跳转到国内版的网站（狗头保命）

所以，点另一个试试看：

[https://www.mozilla.org/en-US/firefox/](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cubW96aWxsYS5vcmcvZW4tVVMvZmlyZWZveC8=)

现在拉到最下面就没有了：

<figure class="wp-block-image size-large">![图片[4]-【零基础学渗透】被动信息收集-FancyPig's blog](img/49cee4158acce14dd5330d9efd4e340c.png)</figure>

### 域名

Domain Name，由一串用点分隔的名字组成的Internet上某一台计算机或计算机组的名称

比如：

`baidu.com`

`qq.com`

由于IP地址具有不方便记忆并且不能显示地址组织的名称和性质等缺点(`baidu.com`,你就知道是百度)

从而设计出域名，并通过网域名称系统（DNS，Domain Name System）来将域名和IP地址相互映射，使人更方便地访问互联网，而不用去记住想要访问的IP。 你想访问百度，不用去记住一长串数字IP了，只用记住baidu.com就好

IP地址和域名是一一对应的，这份域名地址的信息存放在一个叫域名服务器(DNS，Domain name server)的主机内，域名服务器就是提供IP地址和域名之间的转换服务的服务器。

**DNS**就像是一个自动的电话号码簿，我们可以直接拨打baidu的名字来代替电话号码（IP地址）。我们直接调用网站的名字以后，DNS就会将便于人类使用的名字（如`baidu.com`）转化成便于机器识别的IP地址（如`108.210.12.265`）。

**域名的种类：**

不同级别的域名，包括顶级域名，二级域名，三级域名……

```
比如：www.baidu.com
顶级域名：com
二级域名：baidu.com
三级域名：www.baidu.com
```

**域名备案：**

国内网站都有备案，**注册人的信息（注册人、邮箱、联系方式、地理位置）**可以从获取到

**CDN：**

CDN的全称是Content Delivery Network，即**内容分发网络**

CDN的基本原理是广泛采用各种缓存服务器，将这些缓存服务器分布到用户访问相对集中的地区或网络中，在用户访问网站时，将用户的访问指向距离最近的工作正常的缓存服务器上，由缓存服务器直接响应用户请求。

其目的是使用户可就近取得所需内容，解决 Internet网络拥挤的状况，提高用户访问网站的响应速度。

简单来理解，打比方，就是一个服务器架设在东北的网站，我们从海南、西北等距离较远的地方直接访问网站内容会比较慢，有了CDN技术以后，全国各大城市，都有缓存服务器，把网站的内容缓存起来，离得近的用户输入域名的时候，直接连接到缓存服务器上，而不是连比较远的真实服务器。

**域名的历史解析ip：**

由于绑定CDN、网站升级或者业务变迁，域名解析的ip会发生改变，通过查询某个域名的历史解析IP，有可能绕过CDN云，找到他曾经直接解析的**真实IP**上。

你可以使用IP138工具，输入域名后查询到历史解析IP，一定程度上可以找到服务器的源IP地址。

### 域名信息收集的利用

信息收集的目标公司：广州市华软科技发展有限公司

主站网址是`[https://www.huaruan.com.cn/](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cuaHVhcnVhbi5jb20uY24v)`

一、whois 查询

我们从`www.zzy.cn`和站长之家查询得到如下结果

<figure class="wp-block-image size-full">![图片[5]-【零基础学渗透】被动信息收集-FancyPig's blog](img/0da022d26a9b69038bb72b8e8df3d21a.png)</figure>

简单分析一下可能能用的信息：

<figure class="wp-block-image size-full">![图片[6]-【零基础学渗透】被动信息收集-FancyPig's blog](img/9db840a00a540078edd74813b5da8270.png)</figure>

联系人是：**广州市华软科技发展有限公司**

联系邮箱是：`[choice@huaruan.com.cn](https://www.iculture.cc/?golink=aHR0cDovL21haWx0bzpjaG9pY2VAaHVhcnVhbi5jb20uY24v)`

联系人和联系邮箱都可以进行**whois反查以及作为制作字典的素材，可以看这个联系人还注册了哪些域名。**

<figure class="wp-block-image size-large">![图片[7]-【零基础学渗透】被动信息收集-FancyPig's blog](img/eb2c2d3f56418c392ba6fec721884a5d.png)</figure>

99开头的新的**域名**和**邮箱**就出来了

<figure class="wp-block-image size-full">![图片[8]-【零基础学渗透】被动信息收集-FancyPig's blog](img/eb57ccb9eb6e084ae5ed711404806a78.png)</figure>

我们把1开头的邮箱进一步反查：

<figure class="wp-block-image size-large">![图片[9]-【零基础学渗透】被动信息收集-FancyPig's blog](img/98b96262aae85dc471b8dca5f3491dd7.png)</figure>

又得到2个新域名和注册人、联系方式。

……

所有得到的域名**都要打开一遍**，去对应的网站**翻一遍**还有没有什么信息可以搜集到,比如**人名、联系方式、联系邮箱**，后续可以**社工和whois反查**。

域名还可以放到**微步在线网站**，查询更多能利用的信息。

所有得到的公司名称，都丢到**企查查**去看看，对应的老板**名下还有哪些企业**，该企业还有没有什么**分公司**，对应的企业再回来查**whois**。

以上所有整理的和目标相关的信息，利用**各种搜索引擎**进一步搜集，注意要搜索那些**年份比较早的信息**，近几年的可能已经被改过了，但是互联网是有记忆的，**早期安全意识比较差**的时候，很多**真实信息会暴露**。（搜索引擎可设置查询年月日）

关于信息搜集整理工具，建议用**XMIND思维导图**，要不然信息过多你就梳理不清了

相信到这一步，你一定能通过被动信息收集，编织出一大张网状思维导图，根本目的，**就是把目标的整个组织架构给摸清楚，越细节越好**。

这是后续开展渗透测试的重中之重，信息搜集的越多，攻击面就越大。

### 网络空间测绘引擎

您还可以通过网络空间测绘引擎，快速收集目标资产

*   服务器
*   数据库
*   某个网站管理后台
*   路由器
*   交换机
*   公共ip的打印机
*   网络摄像头
*   门禁系统
*   Web服务

### 真实IP收集方式

**验证是否存在CDN**

**方法1：**

很简单，使用各种多地 ping 的服务，查看对应 IP 地址是否唯一，如果不唯一多半是使用了CDN，

多地 Ping 网站有：

[http://ping.chinaz.com/](https://www.iculture.cc/?golink=aHR0cDovL3BpbmcuY2hpbmF6LmNvbS9odHRwOi8vcGluZy5haXpoYW4uY29tL2h0dHA6Ly9jZS5jbG91ZC4zNjAuY24=)

[http://ping.aizhan.com/](https://www.iculture.cc/?golink=aHR0cDovL3BpbmcuY2hpbmF6LmNvbS9odHRwOi8vcGluZy5haXpoYW4uY29tL2h0dHA6Ly9jZS5jbG91ZC4zNjAuY24=)

[http://ce.cloud.360.cn/](https://www.iculture.cc/?golink=aHR0cDovL3BpbmcuY2hpbmF6LmNvbS9odHRwOi8vcGluZy5haXpoYW4uY29tL2h0dHA6Ly9jZS5jbG91ZC4zNjAuY24=)

**方法2：**

使用 nslookup 进行检测，原理同上，如果返回域名解析对应多个 IP 地址多半是使用了 CDN。

有 CDN 的示例：

```
www.163.com服务器: public1.114dns.com
Address: 114.114.114.114
非权威应答:名称: 163.xdwscache.ourglb0.com
Addresses:
58.223.164.86
125.75.32.252
Aliases:
www.163.com
www.163.com.lxdns.com
```

无 CDN 的示例：

```
xiaix.me服务器: public1.114dns.com
Address: 114.114.114.114
非权威应答:名称: xiaix.me
Address: 192.3.168.172
```

**绕过 CDN 查找网站真实 IP**

**1.多地ping**

因为CDN的原因，国内的站用国外ping，反之亦然，可能获取真实IP。

不同的地方去Ping服务器，如果IP不一样，则目标网站肯定使用了CDN。 

**在线ping:**

 [http://ping.chinaz.com/](https://www.iculture.cc/?golink=aHR0cDovL3BpbmcuY2hpbmF6LmNvbS8=) 

[http://ce.cloud.360.cn/](https://www.iculture.cc/?golink=aHR0cDovL2NlLmNsb3VkLjM2MC5jbi8=) 

[http://www.webkaka.com/ping.aspx](https://www.iculture.cc/?golink=aHR0cDovL3d3dy53ZWJrYWthLmNvbS9waW5nLmFzcHg=)

[https://asm.ca.com/en/ping.php](https://www.iculture.cc/?golink=aHR0cHM6Ly9hc20uY2EuY29tL2VuL3BpbmcucGhw)

**2.子域名探测法**

有些子域名可能由于成本的原因没有用CDN，所以要尽可能的多收集子域名，从而有效判断真实ip。

或许可以找到一些没有部署 CDN 的子域名，拿到某些服务器的真实 ip/ 段

**3.搜索引擎 **

常见的有钟馗之眼，shodan，fofa搜索。以fofa为例，只需输入:title:“网站的title关键字”或者body: “网站的body特征”就可以找出fofa收录的有这些关键字的ip域名，很多时候能获取网站的真实ip。

**4.ssl证书收集**

Censys工具能实现对整个互联网的扫描，Censys是一款用以搜索联网设备信息的新型搜索引擎，能够扫描整个互联网，Censys会将互联网所有的ip进行扫描和连接，以及证书探测。

举例：

`xyz123boot.com`证书的搜索查询参数为：`parsed.names：xyz123boot.com`

只显示有效证书的查询参数为：`tags.raw：trusted`

攻击者可以在Censys上实现多个参数的组合，这可以通过使用简单的布尔逻辑来完成。

组合后的搜索参数为：`parsed.names: xyz123boot.com and tags.raw: trusted`

censys将向你显示符合上述搜索条件的所有标准证书，以上这些证书是在扫描中找到的。

要逐个查看这些搜索结果，攻击者可以通过单击右侧的“Explore”，打开包含多个工具的下拉菜单。

`What's using this certificate? > IPv4 Hosts`

此时，攻击者将看到一个使用特定证书的IPv4主机列表，而真实原始 IP就藏在其中。

**5.历史解析记录**

一般网站从部署开始到使用cdn都有一个过程，周期如果较长的话 则可以通过这类历史解析记录查询等方式获取源站ip，查看IP与域名绑定的历史记录，可能会存在使用CDN前的记录。

找国外的比较偏僻的DNS解析服务器进行DNS查询，因为大部分CDN提供商只针对国内市场，而对国外市场几乎是不做CDN，所以有很大的几率会直接解析到真实IP 。

全世界DNS地址：

[http://www.ab173.com/dns/dns_world.php](https://www.iculture.cc/?golink=aHR0cDovL3d3dy5hYjE3My5jb20vZG5zL2Ruc193b3JsZC5waHA=)

[https://dnsdumpster.com/](https://www.iculture.cc/?golink=aHR0cHM6Ly9kbnNkdW1wc3Rlci5jb20v)

[https://dnshistory.org/](https://www.iculture.cc/?golink=aHR0cHM6Ly9kbnNoaXN0b3J5Lm9yZy8=)

[http://whoisrequest.com/history/](https://www.iculture.cc/?golink=aHR0cDovL3dob2lzcmVxdWVzdC5jb20vaGlzdG9yeQ==)

[https://completedns.com/dns-history/](https://www.iculture.cc/?golink=aHR0cHM6Ly9jb21wbGV0ZWRucy5jb20vZG5zLWhpc3Rvcnkv)

 [http://dnstrails.com/](https://www.iculture.cc/?golink=aHR0cDovL2Ruc3RyYWlscy5jb20v) 

[https://who.is/domain-history/](https://www.iculture.cc/?golink=aHR0cHM6Ly93aG8uaXMvZG9tYWluLWhpc3Rvcnkv)

[http://research.domaintools.com/research/hosting-history/](https://www.iculture.cc/?golink=aHR0cDovL3Jlc2VhcmNoLmRvbWFpbnRvb2xzLmNvbS9yZXNlYXJjaC9ob3N0aW5nLWhpc3Rvcnk=)

[http://site.ip138.com/](https://www.iculture.cc/?golink=aHR0cDovL3NpdGUuaXAxMzguY29tLw==) 

[http://viewdns.info/iphistory/](https://www.iculture.cc/?golink=aHR0cDovL3ZpZXdkbnMuaW5mby9pcGhpc3Rvcnk=) 

[https://dnsdb.io/zh-cn/](https://www.iculture.cc/?golink=aHR0cHM6Ly9kbnNkYi5pby96aC1jbi8=)

[https://www.virustotal.com/](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cudmlydXN0b3RhbC5jb20v)

[https://x.threatbook.cn/](https://www.iculture.cc/?golink=aHR0cHM6Ly94LnRocmVhdGJvb2suY24v)

[http://viewdns.info/](https://www.iculture.cc/?golink=aHR0cDovL3ZpZXdkbnMuaW5mby8=) 

[http://www.17ce.com/](https://www.iculture.cc/?golink=aHR0cDovL3d3dy4xN2NlLmNvbS8=)

[https://securitytrails.com/](https://www.iculture.cc/?golink=aHR0cHM6Ly9zZWN1cml0eXRyYWlscy5jb20v)

[https://tools.ipip.net/cdn.php](https://www.iculture.cc/?golink=aHR0cHM6Ly90b29scy5pcGlwLm5ldC9jZG4ucGhw)

**6.邮件记录获取**

就是想办法让目标Web服务器给自己发一封邮件，常见于注册账户、密码找回、订阅、咨询等功能点。

收到邮件后，我们可以查看源代码，特别是其中的邮件头，记录下其中的所有IP地址，包括子域名，这些信息很可能与托管服务有关。

然后，我们可以尝试通过这些地址访问目标。

7.**利用网站返回的内容寻找真实原始IP**

如果原始服务器IP也返回了网站的内容，那么可以在网上搜索大量的相关数据。

浏览网站源代码，寻找独特的代码片段。

在JavaScript中使用具有访问或标识符参数的第三方服务（例如Google Analytics，reCAPTCHA）是攻击者经常使用的方法。

以下是从HackTheBox网站获取的Google Analytics跟踪代码示例：

ga（’create’，’UA-93577176-1’，’auto’）;

可以使用80.http.get.body：参数通过body/source过滤Censys数据

8.利用网站证书查询源IP

### 子域名探测

参考先知社区的[《子域名探测方法大全》](https://www.iculture.cc/?golink=aHR0cHM6Ly94ei5hbGl5dW4uY29tL3QvODY1Mg==)

### 内网信息收集

参考Freebuf的

## 相关文档

这里也收藏了一些热心网友分享的文档供大家参考

<figure class="wp-block-image size-full">![图片[10]-【零基础学渗透】被动信息收集-FancyPig's blog](img/161110f3b22f55d5adbaf71ccc4dcf28.png)</figure>