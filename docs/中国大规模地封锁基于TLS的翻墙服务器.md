<!--yml
category: 防火墙
date: 2026-06-12 19:01:24
-->

# 中国大规模地封锁基于TLS的翻墙服务器

> 来源：[https://gfw.report/blog/blocking_of_tls_based_circumvention_tools/zh/](https://gfw.report/blog/blocking_of_tls_based_circumvention_tools/zh/)

自北京时间2022年10月3日起，超过一百名用户报告他们至少有一台基于TLS的翻墙服务器被封锁了。被封锁的服务器使用的协议包括了[trojan](https://github.com/trojan-gfw/trojan)，[Xray](https://github.com/XTLS/Xray-core)，[V2Ray TLS+Websocket](https://www.v2fly.org/config/transport/websocket.html)，[VLESS](https://www.v2fly.org/config/protocols/vless.html)，以及[gRPC](https://www.v2fly.org/config/transport/grpc.html)。我们还未收到任何[naiveproxy](https://github.com/klzgrad/naiveproxy)被封锁的消息。

下面是我们总结的关于这次封锁的一些信息，以其我们的一些推测和分析

封锁先是针对翻墙服务的端口。如果用户在端口被封后，[改换了端口](https://gfw.report/blog/ss_tutorial/zh/#%E9%85%8D%E7%BD%AE%E5%A4%87%E7%94%A8%E7%AB%AF%E5%8F%A3%E6%9D%A5%E7%BC%93%E8%A7%A3%E7%AB%AF%E5%8F%A3%E5%B0%81%E9%94%81)，那么整个服务器都会被封锁。需要指出，封锁似乎只是基于端口或IP地址，与翻墙服务有关的域名似乎并没有被加入到GFW的DNS或SNI黑名单中。

尽管大多数用户报告443端口被封，一部分使用非443端口的用户也报告了封锁。尽管大多数用户的服务器在流行的VPS提供商那里（[比如](https://bandwagonhost.com/)），但至少有一位用户位于欧洲的家中的服务器也被封锁了。

在一些案例中（并非全部案例中），封锁是动态的：用户通过浏览器还是可以直接访问翻墙端口，但同一个端口，用翻墙软件就连不通。

所有以上的信息都指向GFW已经可以精准的识别并封锁这些翻墙协议，而并非简单地封锁所有的443端口，或封锁所有的流行机房。

基于以上信息，我们推测（但还未进行实证性的测量），这些封锁可能与翻墙软件客户端发出的[Clienthello指纹](https://tlsfingerprint.io/)相关。开发者们或许可以考虑采用[uTLS](https://github.com/refraction-networking/utls)。这个[论文阅读小组](https://github.com/net4people/bbs/issues/54)，[这篇总结](https://gfw.report/blog/v2ray_weaknesses/zh/#%E7%8B%AC%E7%89%B9%E7%9A%84tls-clienthello%E6%8C%87%E7%BA%B9)，以及[这篇博文](https://zhufan.net/2022/06/18/tls%E6%8F%A1%E6%89%8B%E6%8C%87%E7%BA%B9%E6%A3%80%E6%B5%8B%E6%81%B6%E6%84%8F%E8%BD%AF%E4%BB%B6%E6%B5%81%E9%87%8F/)都是关于TLS指纹的，也许会有帮助。

下一步，我们将调查GFW是否真的使用了客户端发出的TLS指纹来识别这些协议。与此同时，如果您有任何翻墙服务器被封锁，或者有任何可以证实或反驳我们的推测的例子，我们都欢迎您或公开地或私下地与我们分享。因为这会帮助我们快速定位许多问题的根源。我们私下的联系方式可见[GFW Report](https://gfw.report/)的页脚。

* * *