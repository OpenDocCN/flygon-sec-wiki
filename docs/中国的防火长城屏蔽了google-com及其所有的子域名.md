<!--yml
category: 防火墙
date: 2026-06-12 19:01:28
-->

# 中国的防火长城屏蔽了google.com及其所有的子域名

> 来源：[https://gfw.report/blog/blocking_of_google_com/zh/](https://gfw.report/blog/blocking_of_google_com/zh/)

我们证实中国的防火长城已经屏蔽了google.com及其所有的子域名。这一封锁策略影响[超过1100个相关域名](https://github.com/net4people/bbs/files/9690188/affected_google_domains.txt)以及大量的流行服务。在这篇文章中，我们介绍观察到的审查者的两次大动作。我们同时分享测量网站审查的方法，以鼓励更多人独立地检测并曝光审查行为。

审查者首先在北京时间2022年9月22日星期四，早上6点23分到晚上7点33分之间的某一时刻将`google.com`和`*.google.com`加入到SNI黑名单中。GFW会检查所有TLS ClientHello包，如果其中的SNI与黑名单匹配，GFW就会立即发送伪造的TCP RST包来切断TCP连接。

八天之后，审查者又在北京时间2022年9月30日星期五，下午1点56分到下午2点35分之间的某一刻将`google.com`和`*.google.com`加入到DNS黑名单中。GFW会检查所有DNS请求包，如果请求的域名与黑名单相匹配，GFW就会立即发送伪造的、含有错误IP地址的应答包给客户端。

下面是一些常见问题：

## Q: 被审查的域名都有哪些？

所有符合`google.com`或`*.google.com`规则的域名都受到了审查。比如，谷歌翻译的域名`translate.google.com`就被审查了。

我们还确认`*google.com`和`google.com.*`还不是黑名单规则。比如说，`madgoogle.com`还有`translate.google.com.co`就还没被审查。

## Q: 这次审查有何影响？

包括一些热门服务在内的[超过1100个域名](https://github.com/net4people/bbs/files/9690188/affected_google_domains.txt)受到了审查。比如说，`firebase.google.com`，`translate.google.com`，`maps.google.com`，`scholar.google.com`，`feedburner.google.com`，还有`ads.google.com`。

我们已经将受到影响的域名[附在](https://github.com/net4people/bbs/files/9690188/affected_google_domains.txt)了这篇帖子上。

## Q: 你们有没有观察到被白名单豁免的域名?

没有。我们测试的1147个`*.google.com`域名全都被屏蔽了，无一例外。

## Q: 你们还观察到了什么？

*   SNI审查和DNS审查的开始时间均在中国的工作时间内。
*   `google.com`被GFW的三种不同的DNS审查机器列入黑名单；而`*.google.com`则只被2号和3号机器列入黑名单。(每个机器发的DNS伪造包的指纹详见[这篇论文](https://censorbib.nymity.ch/pdf/Anonymous2020a.pdf#page=4)的Table 3。)

## Q: 你们是怎么知道这次屏蔽事件的？

我们通过发送含有不同域名的DNS请求包和含有不用SNI的ClientHello包，来持续地监测中国的网站审查。当新的域名遭到审查时，我们就留下了相应的记录。

我们鼓励读者你也独立地测量和监控互联网审查。因为独立测量审查的人越多，我们作为一个集体就越能更快速地发现新的审查事件。

测量一个域名是否受到DNS审查的办法是：向境外的**非**DNS服务器发送一个含有该域名的请求包。选择境外服务器是为了让你的包经过国际网络出口，从而让GFW看到。因为你发送请求的目的地服务器不是DNS服务器，因此如果你收到了任何DNS应答包，那么都是中间人伪造的（常见的中间人就是GFW）。比如说如果你想测试`google.com`是否被审查了，则可以：

```
dig @23.197.152.0 google.com 
```

因为`23.197.152.0`是一台境外的非DNS服务器，所以如果你收到了任何DNS响应包的话，就证明`google.com`被审查了。

测量一个域名是否受到SNI审查的办法是：向境外服务器的*开放端口*发送一个含有该域名的TLS Clienthello包。选择境外服务器是为了让你的包经过国际网络出口，从而让GFW看到。如果你收到了TCP RST包，那么有可能是因为你的连接被GFW阻断了。比如说如果你想测试`google.com`是否被审查了，则可以：

```
openssl s_client -servername google.com -tlsextdebug -msg -connect 96.17.116.205:80 
```

其中`96.17.116.205`是一台境外服务器，`80`是它的一个开放的端口。

如果openssl报错`write:errno=104`，那么说明你的连接被TCP重置了。而这就证明`google.com`*有可能*被审查了。我们说“有可能”是因为有假阳性的可能。为了减少假阳性的可能，你应该用一个不太可能被审查的域名设置一个对照组（比如用`baidu.com`）。然后观察向同一个服务器的同一个端口发送含有`baidu.com`SNI的ClientHello，连接是不是就不会被重置。

* * *