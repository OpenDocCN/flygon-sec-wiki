<!--yml
category: 防火墙
date: 2026-06-12 19:01:39
-->

# 评估苹果的iCloud Private Relay的抗封锁能力

> 来源：[https://gfw.report/blog/private_relay_censorship/zh/](https://gfw.report/blog/private_relay_censorship/zh/)

苹果公司于2021年9月20日，发布了一项名为[iCloud Private Relay](https://support.apple.com/en-us/HT212614) ([archive](https://web.archive.org/web/20210921182126/https://support.apple.com/en-us/HT212614))的新服务，包含在iOS 15， iPadOS 15和macOS Monterey中。

尽管苹果公司没有将它的翻墙功能作为卖点，在这篇报告中，我们试图理解iCloud Private Relay的翻墙价值。首先，基于我们的测量和对苹果文档的理解，我们介绍Private Relay的工作原理。接着我们通过在中国进行的测量实验实证性地评估Private Relay的抗封锁能力。截止2021年9月23日，我们尚未发现Private Relay被防火长城审查的迹象。我们还将讨论Private Relay面对常见的审查方式（如DNS劫持，SNI过滤，IP封锁，主动探测，和自我审查）时的抗封锁能力。最后，我们将提出一些关于Private Relay的重要但还未解决的问题。

我们无意将这篇报告作得面面俱到。而仅想抛砖引玉地介绍我们的测量方法、观察及想法。我们鼓励更多的互联网审查爱好者做更深入的研究。

## 主要发现

*   截止2021年9月23日，我们尚未发现Private Relay被防火长城审查的迹象
*   Private Relay可以很容易地被常见的审查方式封锁，包括DNS劫持，SNI过滤，IP封锁，主动探测，和自我审查。主动探测Priavte Relay服务器也许也是可行的。
*   Private Relay这项服务已被苹果在包括中国在内的许多国家和地区自我审查。但用户报告只需使用相应的国外iCloud账户即可绕过禁用。

## 介绍

以下介绍基于我们的测量和对[苹果的](https://support.apple.com/en-us/HT212614)[文档](https://developer.apple.com/support/prepare-your-network-for-icloud-private-relay)的理解。简而言之，Private Relay采用两跳结构，由一个入口节点和一个出口节点组成：

```
 ------------ | DNS 服务器  | ------------ ^ | A mask.icloud.com? HTTPS mask.icloud.com?  | 0 | ------           -------------           ------------           ------ |客户端 | <==1==> |   入口节点   | <==2==> |   出口节点   | <==3==> |目标网站| ------           -------------           ------------           ------ 
```

*   第0步： 客户端发送两个明文的DNS请求到DNS服务器。请求的类型为`A`和`HTTPS`，请求的域名为`mask.icloud.com`或`mask-api.icloud.com`。目的是得到入口节点的IP地址。
*   第1步： 客户端选取其中一个DNS服务器返回的IP地址，并发送QUIC初始包到入口节点的443端口。
*   第2步： 根据[文档](https://support.apple.com/en-us/HT212614)，“出口节点由第三方运行，会生成一个临时IP地址，解密得到请求的目标网站地址，并与网站进行连接”。
*   第3步： 出口节点和目标网站间的流量与，不启用Piravte Relay时，客户端和目标网站间的流量完全相同。

## 捕获iPhone与入口节点间的流量

一种自然想到的捕获和分析移动设备流量的方式是，在手机上建立起工作在网络层的VPN，把所有传输层及更上层的流量都转发到一个（本地的）服务器上，再在服务器上运行`tcpdump`或者`wireshark`。然而我们发现在VPN打开的状态下，iCloud Private Relay是无法启用的。

作为替代方式，我们在电脑上建立起无线热点，并让iPhone连上去。我们这样就可以在电脑上捕获并分析流量了。我们用了以下脚本搭建无线热点，脚本借用了[这个教程](https://computingforgeeks.com/create-wi-fi-hotspot-on-ubuntu-debian-fedora-centos-arch/)里的知识。

```
#!/bin/bash   set -x set -e   ## Source: https://computingforgeeks.com/create-wi-fi-hotspot-on-ubuntu-debian-fedora-centos-arch/   ## 记得将IFNAME替换成你的Wi-Fi network interface的名字: `ip link show` IFNAME="wlp4s0" CON_NAME="MyHotSpot" PASSWORD="77fdda98a6feaf6cc9"   nmcli con add type wifi ifname "$IFNAME" con-name "$CON_NAME" autoconnect yes ssid "$CON_NAME"   nmcli con modify "$CON_NAME" 802-11-wireless.mode ap 802-11-wireless.band bg ipv4.method shared   nmcli con modify "$CON_NAME" wifi-sec.key-mgmt wpa-psk nmcli con modify myhotspot wifi-sec.psk "$PASSWORD"   nmcli connection show "$CON_NAME"   nmcli con up "$CON_NAME"   nmcli connection show "$CON_NAME" 
```

在观察DNS和初始的QUIC流量时，我们发现以下过虑条件很好用：

```
quic.long.packet_type == 0 or udp.port == 53 
```

## 测量当前的审查并评估潜在的审查成本

在这一节中，我们将测量中国当前对于Pirvate Relay的审查，并讨论审查者采用常用审查方法检测并封锁Private Relay的成本。

### DNS劫持

前面提到客户端会通过DNS查询得到一个入口节点的IP地址，然后用QUIC协议与其建立连接。因为这些DNS请求（很有可能被故意设计成）是明文的，因此容易受到DNS劫持攻击。事实上，苹果公司自己就[建议](https://developer.apple.com/support/prepare-your-network-for-icloud-private-relay)使用DNS劫持来“最快速和稳定”地封锁Private Relay：

> The fastest and most reliable way to alert users is to return a negative answer from your network’s DNS resolver, preventing DNS resolution for the following hostnames used by Private Relay traffic. Avoid causing DNS resolution timeouts or silently dropping IP packets sent to the Private Relay server, as this can lead to delays on client devices.
> 
> mask.icloud.com
> 
> mask-h2.icloud.com

我们观察到客户端有两种获得入口节点地址的方式。第一种方式是：

1.  客户端首先发送两个DNS查询，查询类型为`A`和`HTTPS`，查询的域名为`mask.icloud.com`。DNS应答包含一个`CNAME`答案`mask.apple-dns.net`，以及多个`A`答案。
2.  客户端选择DNS应答包中的第一个答案，即那个`CNAME`答案。客户端因此需要再次发送两个DNS查询，查询类型还是`A`和`HTTPS`，查询的域名为`mask.apple-dns.net`。
3.  客户端接着还会选取DNS应答包中的第一个答案，这次是`A`答案。

第二种方式是：

1.  客户端首先发送两个DNS请求，查询类型为`A`和`HTTPS`，查询的域名为`mask-api.icloud.com`。DNS应答包含一个`CNAME`答案`mask-api.fe.apple-dns.net`，以及多个`A`答案。
2.  客户端选择DNS应答包中的第一个答案，即那个`CNAME`答案。客户端因此需要再次发送两个DNS查询，查询类型还是`A`和`HTTPS`，查询的域名为`mask-api.fe.apple-dns.net`。
3.  客户端接着还会选取DNS应答包中的第一个答案，这次是`A`答案。

我们没有观察到客户端会查询[文档](https://developer.apple.com/support/prepare-your-network-for-icloud-private-relay)中记录的`mask-h2.icloud.com`。这篇[报告](https://isc.sans.edu/forums/diary/A+First+Look+at+Apples+iOS+15+Private+Relay+feature/27858/)也提到没有观察到包含`mask-h2.icloud.com`的DNS查询。

#### 测量中国当下的DNS审查

虽然污染上述提到的域名对GFW来说小菜一碟，但是我们还并未观察到GFW真的采取审查。具体而言，我们从中国发送上述DNS请求到国外，也从国外发送请求回中国，来让GFW的设备看到我们的请求。你即使不在中国，也同样可以利用GFW不区分DNS来源于国内或国外的特性，来测量DNS污染。需要注意的是，`dig`命令尚不支持发送[`HTTPS类型`](https://www.ietf.org/archive/id/draft-ietf-dnsop-svcb-https-07.html#name-the-svcb-record-type)的DNS请求。如果使用`dig @1.1.1.1 mask.icloud.com -t HTTPS +timeout=2`这样的请求，它会**自动回落到发送`A`类型请求**。因为回落警告不足够明显，大家要当心回落带来的测量失误。

我们因此在从国外发DNS请求往国内时，使用了以下脚本，调用Scapy发送DNS请求。这里我们用到的`104.193.82.0`是一个中国的IP地址，而且这个中国的IP地址没有运行DNS服务，这样如果我们收到了任何DNS答复，那么一定是GFW或其他中间人伪造的。我们因此也就可以判断GFW是否审查了相应的域名。

```
#!/usr/bin/env python3   # 这个脚本只负责发送DNS请求，不负责接收DNS应答。 # 如果想观察DNS应答，请使用tcpdump或者wireshark。比如： # sudo tcpdump host 104.193.82.0   from scapy.all import *   # https://www.ietf.org/archive/id/draft-ietf-dnsop-svcb-https-07.html#name-the-svcb-record-type TYPE_HTTPS=65   CHINESE_IP="104.193.82.0"   for qname in ["mask.icloud.com",  "mask-api.icloud.com", "mask.apple-dns.net", "mask-api.fe.apple-dns.net", "mask-h2.icloud.com"]: for qtype in [TYPE_HTTPS, "A", "AAAA"]: send(IP(dst=CHINESE_IP)/UDP(dport=53)/DNS(rd=1, qd=DNSQR(qname=qname, qtype=qtype))) 
```

### SNI过滤

正如[这个答案](https://stackoverflow.com/a/65400340)所介绍的，虽然QUIC中的Clienthello消息是加密的，但[密钥是由固定的salt和明文的Destination Connection ID导出的](https://datatracker.ietf.org/doc/html/draft-ietf-quic-tls-33#section-5.2)。QUIC的初始包因此也就可以被很容易的解密。事实上，新版的`Wireshark`就可以自动地解密客户端发出的QUIC初始包。

审查者也因此可以加密QUIC初始包并检查其中的SNI值是否为`mask.icloud.com`。

#### 测量当下中国的(QUIC-)SNI审查

我们测试的方法是抓取客户端发送的初始包，然后在中国的服务器上重放。我们观察到服务器会回以QUIC握手包。我们还没有观察到审查者阻断这一过程。

举例来讲我们首先将以下十六进制流存入名为`quic.hex`的文件:

```
c80000000108bec8eac6d55fe88a08e87fe5dfa21d247700452c6a2a855275bb191ffe213c2ad1e07467f9ed24956172c4bee69e8446049a94fbae38973cf11ce80cc1379237e4a0f610ae2408ac096635b3978dcf21b4c81d96a2e53d9a9b04dc234341869f7ea85dc99e2ea028827257c4b6993a29ae07e9368c22426d1780abcf8c4b5ab8b2e3ba4de878306ecf4a4e5851c2168b8412f9a55fa5971520914f13c4a86106161e19bfa1eff9c08a9b566e1656ebceeb7184d60a0328203e5e66fc16ad8452343dee5e2ad22ba0ef80b978ba62c64ac75826b79d119c5a7bf9859655d79116e3f4069e87269bab7f9d0373d8e98e4c4891eaf621ce073c61f59eeddd828c96d785ff3155083f5ac93263e5496a6a38a2a2e0bbf64041e76a500bf4748143f2b8705c3732dc12b218f428eeadd02c50e71c5ffaa1ca14c483ac44c75d10e98d38ddaa38f38c0ba7af20108967541586c51bccd2765781b123b4a91fed0f32f0b11bfc4ff5beaccb023f7d977787a0d09942f5159da772b9ca5a7c512a8644bb399858cc6ee2a5d5c099be6780a619cbadc6407db320d34179bf1ff94401ef0e134d0d8ae705a468b5dcd7b9c078e72ddba146e947dca7d4968b3fc892e425ef60bec05df120b20f26f340ed134b064b4fddd194fee666ff49c943b82f812c6f57daaa70ef5aa7b511e8d9501a447783a4e7eee709499161c4055941d516f16bc4ed114d90f6d49c1a297484749fa99f84eb309c2743018eeebb71c6050061e4899b94ecc746fb98174ce383a9d250f61d3aca4db9249122763ca03c41a67b616e722f5171e34a610aeb9cfb6c74f8bdd549d1b0fbd4a766dd66dc355de7f55d55e029d495687c149d9bac0eba89276a0c8048a97545c08597ea836917ceaebe2e334d9376f3c3dc29ee6df84508558d2c77a1139907aba7735945846e3a8c4675845e01c7e2cb1370b31221f95e1bd0c8e5ecc9e86bf5658859379e3c752e34d6bf9e0e9481cf9ed5df79b9c756cb904603eaab2478b6aaa5740b28213f2716b2b4769e21d9c7e2d62e9708de9644a3de048745f079717e0a565475d0684be9cb13c261f55832953c37078cde29894b2176eab5157e4262dbba7919ef2c66d0cf86d7de93059e9f013e2e82544dc803dea878e184d248065454c65c26d8c67b7778e229390de7560815e6cdc53cce1fb11d62d9b0ecea890b4310ffcf7cd544ca8d6a1b9eed7b92a93fd6c00d0a2338f66ad77c9220c69437b3651b18899c68a8e59f12dc2f014d70a6ca5b4aa419516fde01079a1f76c3198db4f6229641e5e89b1b6aa9797c27b55f439e98858f9d3eef1ffff6f5e52e9e94468d21e8ab965abb864836be07016fdbb63a24e954b863a98d590033bd163df6a7740d256a0bbaa910e45a8f40877b6b84fd2d8f57604d236e4351bd228dc707fe3538440b2796dbab58183f306912c6104d13ea96c649fd338994b4a2d5ecbfdd66b69b5245763371cc38c92774723f546a27519db4660f5dc6312f5f56edad2dcb77bd3034c8a4a084ee7e57016fea8a5fcb114ee5ae97d55b177dd8b1ccd0508fb6baee6244ccedf2705ba35a760b944acb4b3e0394b5add44e851d18e0400d99b4910cd4cb63311727f4a98289ce4ee960c506b72243fde14cf5d3185cc4b598f080faf9ebc75847dc7126bb90c47368c5408898e7bdaf9cde4f04299043600dcdf850c306c737d4be37c316eed63718804e9972f6c95d79771ada173293b06037f1282f4e79f8116e3d4c5fed2ec6db335faf2b0481b3aa3a0192f9ff3fea35ae1bafe71bbcd07a301fe11638a180b6b202c29dea331ac6a2527587a82175cd7b96033b165b88580e83df7759ebe6586d68d4efe6028403d5d0d700e967ca4908bbd8e4 
```

我们再将其发送到入口节点，并得到了回应:

```
xxd -r -p quic.hex | nc -u mask.icloud.com 443 -v 
```

注意从生成这个十六进制荷载的大约两天后，发送它已经不能再能得到入口节点的回应了。如果重放刚生成的初始包，应该还可以触发入口节点的回应。

### (Quic-)TLS指纹过滤

如上所述，QUIC初始包可以被很容易的解密得到ClientHello。这就给了审查者采用[基于TLS指纹的审查](https://github.com/net4people/bbs/issues/54)的可乘之机。

我们对Private Relay的TLS指纹的观察于[这篇报告](https://isc.sans.edu/forums/diary/A+First+Look+at+Apples+iOS+15+Private+Relay+feature/27858/)基本一致：

> The connection to the relay uses QUIC to port 443/UDP and TLS 1.3\. The clienthello includes the server name extension and the server name “mask.icloud.com.” Only 3 cipher suites are offered (TLS_AES_128_GCM_SHA256, TLS_AES_256_GCM_SHA384, TLS_CHACHA20_POLY1305_SHA256). The server ends up selecting the AES128 suite. Application Layer Protocol Negotiation (ALPN) is also used, with unsurprisingly HTTP/3 being the only option.

除了引文中提到的3种密码套件外，我们还观察到了第四种Grease ciphersuit (`0x2a2a`)。

作为旁注，我们还观察到了ClientHello中包含了两个GREASE extensions：[0xAAAA](https://www.rfc-editor.org/rfc/rfc8701.html#section-2-3.1)和[0X3A3A](https://www.rfc-editor.org/rfc/rfc8701.html#section-2-3.4)。他们不太可能是被用于验证相关的目的（如果真的是被用作验证目的，那就是很不服合标准的做法）。GREASE extensions其实并不罕见；浏览器也会发送它们。正如[这篇文档](https://tools.ietf.org/id/draft-ietf-tls-grease-04.html)所解释的，它们被用来“保证TLS实现可以正确的处理不认识的值”。换而言之，因为GREASE的存在，TLS实现就不能假设只会收到和处理某些特定的值了。

我们很好奇[tlsfingerprint.io](https://tlsfingerprint.io/)是否能告诉我们这些（或者任何）(QUIC) ClientHello的指纹有多特殊？([@sergeyfrolov](https://github.com/sergeyfrolov), [@ewust](https://github.com/ewust))

### 主动探测入口节点

我们发现在大约两天之内重放QUIC初始包到入口节点，入口节点都会回以发送握手包。我们还尝试使用[quic-go](https://github.com/lucas-clemente/quic-go)和[`curl --http3`](https://github.com/lucas-clemente/quic-go)对入口节点进行典型的Quic握手，SNI为`mask.apple.com`。但入口节点并不回应。我们怀疑这与合法客户端发送的ClientHello的[ALPN extension](https://datatracker.ietf.org/doc/html/rfc7301)有关。但入口节点是否回应也可能还与其他的验证信息有关。

### 封锁入口节点的IP地址

如前所述，从中国发送的QUIC握手包可以得到入口节点的回应。这说明，至少我们测量的IP地址还没有被封锁。

但是仍有很多方法封锁入口节点的IP地址，比如审查者可以：

1.  封锁所有解析`mask.icloud.com`，`mask-api.icloud.com`或 `mask-h2.icloud.com`时返回的IP地址。
2.  观察QUIC链接中SNI为`mask.apple.com`的服务器IP地址，然后用主动探测确认入口节点。确认后封锁相应IP地址。

### 基于出口节点的IP地址歧视用户

如Tor出口节点一样，苹果也有一个实时更新的[出口节点IP段列表](https://mask-api.icloud.com/egress-ip-ranges.csv) ([存档](https://web.archive.org/web/20210921182354/https://mask-api.icloud.com/egress-ip-ranges.csv))。这个列表可以方便网站基于出口节点的IP地址来歧视Private Relay用户，正如[Tor用户所遭受的歧视一样](https://www.icir.org/vern/papers/tor-differential.NDSS16.pdf)。

## 还未解决的问题

### 苹果是如何实现自我审查的

除了上述的种种审查方式外，苹果公司还通过自我审查的方式阻止在本就身处受到严格审查地区的用户使用Private Relay。因此了解并绕过苹果公司的自我审查尤为重要。

具体来讲，苹果公司[承认](https://support.apple.com/en-us/HT212614)：

> Private Relay isn’t available in all countries and regions. If you travel somewhere Private Relay isn’t available, it will automatically turn off and will turn on again when you re-enter a country or region that supports it. Private Relay will notify you when it’s unavailable and when it’s active again.

根据相关的[新闻](https://www.macrumors.com/2021/09/17/icloud-private-relay-disabled-russia/)[报道](https://www.reuters.com/world/china/apples-new-private-relay-feature-will-not-be-available-china-2021-06-07/)， 苹果在以下地区禁用了Private Relay：中国、白俄罗斯、哥伦比亚、埃及、哈萨克斯坦、沙特阿拉伯、南非、土库曼斯坦、乌干达、菲律宾和俄罗斯。

苹果自我审查的实现机制还有待研究。我们的测试显示，入口节点似乎并不基于用户IP地址来拒绝服务。但是我们仍不清楚苹果公司是如何判断用户是否身处被禁止使用Private Relay的国家。

一项[报告](https://qust.me/post/PrivateRelay/)([存档](https://web.archive.org/web/20210707024209/https://qust.me/post/PrivateRelay/))声称苹果公司是根据用户访问特定的一组苹果服务器时的IP地址，来判断用户位置的；使用境外代理访问这组苹果服务器即可激活Private Relay功能。

而另一个用户[报告](https://v2ex.com/t/803142)([存档](https://web.archive.org/web/20210924192532/https://v2ex.com/t/803142))只需将iCloud的地区设置为非禁用地区，就可以启用Private Relay了。但同一个帖子中的另一用户声称使用了非禁用地区iCloud但仍然无法开启Private Relay。我们欢迎中国的用户分享你的经验。

另外作为一点背景介绍，对于中国的iOS翻墙用户，拥有一个非中国iCloud账户并不罕见。这是由中国的App Store对翻墙软件的严格审查导致的。

### 苹果是如何验证Private Relay用户的？

苹果公司[声称](https://developer.apple.com/support/prepare-your-network-for-icloud-private-relay)：

> Private Relay validates that the client connecting is an iPhone, iPad, or Mac, so you can be assured that connections are coming from an Apple device.

> All connections that use Private Relay validate that the client is an iPhone, iPad, or Mac and that the customer has a valid iCloud+ subscription. Private Relay enforces several anti-abuse and anti-fraud techniques, such as single-use authentication tokens and rate-limiting.

我们好奇苹果是如何验证Private Relay用户的。

### 苹果的Private Relay是如何加密解密的？

前面我们介绍Private Relay采用两跳结构。除此之外我们并不了解更多的技术细节。比如说，这两跳是否采用了如[onion-routing](https://en.wikipedia.org/wiki/Onion_routing)的结构？Amir Houmansadr[表达了对Private Relay协议不透明的关切](https://gfw.report/blog/private_relay_privacy/en/#our-immediate-questions-about-private-relays)。Private Relay的协议和工作原理因此有待更多的调查。

## 致谢

我们感谢一位把iPhone手机借我们测试的人。

## 联系我们

这篇报告首发于[Net4People](https://github.com/net4people/bbs/issues/87)。我们还在[GFW Report](https://gfw.report/blog/private_relay_censorship/zh/)和[ntc.party](https://ntc.party/t/evaluating-the-censorship-resistance-of-apples-icloud-private-relay/1346/2)同步更新了这篇报告。

我们鼓励您公开地或私下地分享与报告中的发现和假设相关的问题、评论或证据。我们私下的联系方式可见[GFW Report](https://gfw.report)的页脚。

* * *