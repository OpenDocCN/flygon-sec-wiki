<!--yml
category: 防火墙
date: 2026-06-12 19:02:21
-->

# 总结近期发现的V2Ray弱点

> 来源：[https://gfw.report/blog/v2ray_weaknesses/zh/](https://gfw.report/blog/v2ray_weaknesses/zh/)

近期数个V2Ray的弱点被发现。这些弱点可以被用来识别使用VMess、TLS或HTTP协议的V2Ray客户端和服务器。 以下是我们对这些弱点的总结和理解。

总体上，这些弱点可分为三类：

*   VMess服务器没有正确验证客户端的请求，使得服务器可受到重放攻击。
*   客户端硬编码了一套罕见的TLS密码套件，导致客户端发送的TLS ClientHello拥有几乎独一无二的指纹。
*   伪装成HTTP服务器的企图失败。

## 针对VMess协议的重放攻击

[VMess协议](https://www.v2ray.com/developer/protocols/vmess.htmlhttps://www.v2ray.com/developer/protocols/vmess.html)的请求构造如下：

| 16 字节 | *X* 字节 | 余下部分 |
| --- | --- | --- |
| 认证信息 | 指令部分 | 数据部分 |

*   `认证信息` 是一个16字节的HMAC，基于`用户 ID`和`UTC 时间辍`.
*   `指令部分` 由`AES-128-CFB(iv， key)`算法加密， 其中[`iv` 是`UTC 时间辍`的md5哈希值](https://github.com/v2ray/v2ray-core/blob/4b81ba947f89218ea7c99362b43beeeb5c3cf37b/proxy/vmess/encoding/server.go#L137)， `key`是V2Ray用户自己设置的那个密码。

下表为`指令部分`解密后的结构：

| 1 字节 | 16 字节 | 16 字节 | 1 字节 | 1 字节 | 4 位 | 4 位 | 1 字节 | 1 字节 | 2 字节 | 1 字节 | *N* 字节 | *P* 字节 | 4 字节 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 版本号 Ver | 数据加密 IV | 数据加密 Key | 响应认证 V | 选项 Opt | 余量 P | 加密方式 Sec | 保留 | 指令 Cmd | 端口 Port | 地址类型 T | 地址 A | 随机值 | 校验 F |

*   `数据加密 IV`和`数据加密 Key`是用来加密/解密`数据部分`的，不是用来加密/解密`指令部分`的。
*   `余量 P`和`随机值`是填充机制的一部分。其中`余量 P`占4位，用来表示`随机值`的长度。长度取值在0到15字节之间。
*   `校验 F`是一个[MAC](https://zh.wikipedia.org/zh-cn/%E8%A8%8A%E6%81%AF%E9%91%91%E5%88%A5%E7%A2%BC)。在合法的请求中，它的值应该是除自己以外所有`指令部分`的FNV1a哈希值。

### 验证客户端请求时的弱点

2020年5月31日，[@p4gefau1t](https://github.com/p4gefau1t)[报告](https://github.com/v2ray/v2ray-core/issues/2523)，由于客户端请求的合法性没有得到恰当的认证，VMess服务器可以被基于重放攻击的主动探测识别。

VMess服务器分两步，分别用`认证信息`和`校验 F`来鉴定客户端请求的合法性。不幸的是，这两步都可以被攻击者绕过。

第一步， VMess服务器验证包含在`认证信息`中的`时间辍`是否已经过期。 保质期最长为[120秒](https://github.com/v2ray/v2ray-core/blob/4b81ba947f89218ea7c99362b43beeeb5c3cf37b/proxy/vmess/validator.go#L18)，平均值为60秒。其具体的实现细节请见[这里](https://github.com/v2ray/v2ray-core/blob/4b81ba947f89218ea7c99362b43beeeb5c3cf37b/proxy/vmess/encoding/server.go#L132)还有[这里](https://github.com/v2ray/v2ray-core/blob/4b81ba947f89218ea7c99362b43beeeb5c3cf37b/proxy/vmess/validator.go#L130)。 这也就是说， 攻击者可以先记录下正常客户端发出的合法的`认证信息`，然后只要在60秒左右的时间内，在自己的恶意请求中使用这段合法的`认证信息`，就可以通过服务器第一步的验证。

第二步， 由于用来加密`指令部分`的`aes-cfb`算法本身不提供数据认证， VMess协议使用了一个[MAC-then-Encrypt](https://zh.wikipedia.org/zh-cn/%E8%AE%A4%E8%AF%81%E5%8A%A0%E5%AF%86#MAC-then-Encrypt_%28MtE%29)的机制校验数据的真实性和完整性。 @p4gefau1t[指出](https://github.com/v2ray/v2ray-core/issues/2523#issuecomment-636528060)， VMess协议掉入了[与Shadowsocks OTA模式当年同样的陷阱](https://printempw.github.io/why-do-shadowsocks-deprecate-ota/)。(英文版的Shadowsocks OTA模式漏洞请见[这里](https://groups.google.com/forum/#!msg/traffic-obf/CWO0peBJLGc/Py-clLSTBwAJ)。) 具体来讲， 由于`随机值`的长度在每个请求中是变化的， 因此服务器被迫在还没有验证`余量 P`的真实性之前就盲目的信任这个值。 然后服务器才能确定`校验 F` (MAC)在请求中的位置。 (具体实现细节请见[这里](https://github.com/v2ray/v2ray-core/blob/4b81ba947f89218ea7c99362b43beeeb5c3cf37b/proxy/vmess/encoding/server.go#L163-L198)) 也就是说， 在未经校验的情况下读取了P+4字节后， V2Ray才能开始验证请求的真实性和完整性。 如果验证未通过， V2Ray服务器就会断开连接。

VMess服务器确实有一个[对抗重放攻击的机制](https://github.com/v2ray/v2ray-core/blob/4b81ba947f89218ea7c99362b43beeeb5c3cf37b/proxy/vmess/encoding/server.go#L159)。 不管请求是否合法， 服务器都会记录下每个请求中使用的`数据加密 IV`-`数据加密 Key`对； 如果一个请求中的`数据加密 IV`-`数据加密 Key`对已经在之前的请求中使用过了， 服务器就会立即断开连接。 因此取决于其需要，攻击者可以对这个对抗重放攻击的机制做两件事：

1.  攻击者可以修改密文中对应`数据加密 IV`或`数据加密 Key`的部分，从而绕过这个对抗重放攻击的机制。
2.  攻击者可以故意触发这个对抗重放攻击的机制，然后观察对于同一个`数据加密 IV`-`数据加密 Key`对，服务器在第一次和第n>1次见到它时反应是否不同。

利用以上弱点， 各种基于重放攻击的，针对VMess服务器的主动探测被创造出来。 我们现在对这些攻击按类别进行介绍。

### 修改填充长度的重放攻击

基于[@p4gefau1t](https://github.com/p4gefau1t)[发现的VMess弱点和攻击](https://github.com/v2ray/v2ray-core/issues/2523)， [@studentmain](https://github.com/studentmain)提出了一种更强的攻击来识别VMess服务器。这种攻击后又被[@p4gefau1t](https://github.com/p4gefau1t)再次修改增强。 为了叙述简洁， 我们用一种稍有不同的方式呈现它。

该主动探测的载荷基于对合法客户端发送的密文请求的修改，其构造如下：

| 16 字节 | 41 字节 | *M* 字节 |
| --- | --- | --- |
| 认证信息 | 恶意修改的指令部分 | 零 |

`恶意修改的指令部分`的结构如下:

| 1 字节 | 16 字节 | 16 字节 | 1 字节 | 1 字节 | 4 位 | 4 位 | 1 字节 | 1 字节 | 2 字节 | 1 字节 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 版本号 Ver | 数据加密 IV | 数据加密 Key | 响应认证 V | 选项 Opt | 余量 P | 加密方式 Sec | 保留 | 指令 Cmd | 端口 Port | 地址类型 T |

攻击者在一次攻击中，共向服务器发起16次连接。在每次连接中攻击者：

1.  首先记录下某一合法请求的前16+41字节。
2.  然后将其中对应`数据加密 Key`的最后一字节和对应`余量 P`的部分修改为与原来不同的值，并发送。注意要保证不同连接中修改后的值都不一样。
3.  最后每隔一秒发送一字节的零（或随机）数据，直到服务器主动断开连接。记发送的零（或随机）数据的字节长度为*M*。

如果16次连接中记录的*M*没有重复且最大值和最小值的差为15，那么被探测的服务器就很有可能是在使用VMess协议。

对于该攻击的解释如下：

*   为了绕过服务器基于`认证信息`的校验，攻击者在大约60秒的时间内重复使用同一个来自合法客户端的`认证信息`。
*   为了绕过服务器基于`数据加密 IV`-`数据加密 Key`对的过滤器， 攻击者修改了每次连接中的`数据加密 Key`。
*   为了避免修改`数据加密 Key`后[错误扩散](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Error_propagation)到`余量 P`，攻击者精心的选择了只修改`数据加密 Key`的最后一字节。因为这一字节与`余量 P`同属于一个16字节的密码块。（`AES-128-CFB`算法的[错误扩散](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Error_propagation)是这样的： 改变密文块*Ci*中的任意1位，将会 1）改变其对应明文块*Pi*中对应的1位；2)并可能随机的改变其之后所有明文块中的任意位。）
*   攻击者利用流加密算法的[malleability](https://en.wikipedia.org/wiki/Malleability_%28cryptography%29)特性， 在16次连接中，通过遍历`余量 P`所有的密文空间来遍历其所有的明文空间。
*   在读取16+41字节后， 服务器会期待客户端发送*N*字节的`地址 A`、*P*字节的`填充`以及4字节的`校验 F`。 因此*M*的实际值为*N+P+4*。
*   攻击者因此可以利用*M*猜出每次连接中`余量 P`的明文值。 （因为`地址类型 T`是不变的，所以`地址 A`占用的长度为固定值。）

### 触发服务器不同反应的重放攻击

[@nametoolong](https://github.com/nametoolong) [发现了另外两种重放攻击](https://github.com/v2ray/v2ray-core/issues/2539#issuecomment-638533283)，可以用来识别即使修复了上述问题的VMess服务器。

这两种攻击都与服务器何时及如何关闭连接有关。 我们在这里仅介绍第一种攻击，并把第二种攻击的原理留给读者作为练习。

[@nametoolong](https://github.com/nametoolong)描述了第一种攻击载荷和其期待引起服务器反应：

```
 攻击 1：
    M1为一个合法连接的前54个字节。
    M1修改第48字节，其余字节保持不便，记作M2。
    发送M1，服务器会立即断开连接。
    发送M2，服务器不会立即断开连接。
    再次发送M2，服务器会立即断开连接。 
```

注：被替换的*第48字节*（从0开始数的）即为的`数据加密 Key`的最后一个字节。

在这次攻击中， 攻击者故意触发重放防御机制， 并期待着服务器在第一次与更多次见到同一个`数据加密 IV`-`数据加密 Key`对时， 会有不同的反应。 具体介绍如下：

1.  因为已经在之前的合法连接中见过M1中的`数据加密 IV`-`数据加密 Key`对， 所以服务器会检测出M1是重放攻击，并立即断开连接。
2.  当第一次发送M2时，服务器是第一次见到其包含的`数据加密 IV`-`数据加密 Key`对， 所以服务器不会认定M2是重放攻击，因此会继续等待攻击者发送更多的字节，而不断开连接。
3.  当第二次发送M2时，服务器因为已经见过同样的`数据加密 IV`-`数据加密 Key`对， 所以服务器会认定这次的M2是重放攻击，并且立即断开连接。

V2Ray已经将断开连接时所需的时长和所需接收的字节数随机化， 但由于不是在遇到所有类型的错误时都统一采用了这一机制， 因此给了攻击者可乘之机。

[@nametoolong](https://github.com/nametoolong)因此建议：

```
 断开连接的机制要保持统一。
    但也需要考虑断开连接这一行为本身是否会泄露什么特征。 
```

### 我们的评论

证据显示，以上提到的攻击以GFW现有的能力来讲是切实可行的。 比如说，GFW[被观察到](https://gfw.report/blog/gfw_shadowsocks/zh/#%E4%B8%BB%E5%8A%A8%E6%8E%A2%E6%B5%8B%E7%9A%84%E5%BB%B6%E8%BF%9F%E6%80%A7)可以记录合法客户端的连接，并在0.4秒到数百小时之间的延迟后，将修改过的（或未修改过的）载荷发送给被怀疑的服务器。

下一步，我们将调查GFW是否已经使用了针对V2Ray的主动探测攻击。与此同时，如果你有任何翻墙服务器被封锁，我们都欢迎你或公开的或私下的与我们分享你的配置。因为这会帮助我们快速定位许多问题的根源。

也许同时基于`实效性`和`重复性`的请求验证是抵抗GFW重放攻击的好办法。 一方面， V2Ray在`认证信息`中仅使用了基于`实效性`的重放攻击防御措施。 但这会导致在一定时间内的任何重放攻击都是有效的。 另一方面， Shadowsocks-libev使用了一个[基于nonce的重放过滤器](https://github.com/shadowsocks/shadowsocks-org/issues/44)。 但为了让仅基于nonce的重放过滤器有效检测重放攻击， 服务端被不现实的要求（在主密钥更换之前）必须要一直记住所有**合法**连接中的nonce，即使是服务端重启之后！ 因此， 同时基于`实效性`和`重复性`的防御机制似乎是抵抗GFW重放攻击的更有效办法。

[Frolov et al.](https://censorbib.nymity.ch/pdf/Frolov2020a.pdf)发现, 包括obfs4, Shadowsocks Outline, 赛风的OSSH和蓝灯的Lampshade在内的许多翻墙协议或工具都可以被服务器断开连接的方式所识别。 具体而言，不同的主动探测可能导致服务器以不同的TCP包或不同的时长来断开连接。 Frolov et al.给出的建议是让[服务器在遇到错误时“永远读取buffer”](https://censorbib.nymity.ch/pdf/Frolov2020a.pdf#page=13)。 直到攻击者自己主动断开连接。 这样做一方面减少了超时信息的泄漏， 另一方面也使得服务器只会使用FIN/ACK断开连接， 而不用RST和FIN/ACK的混合方式（原理请见[Fig. 1](https://censorbib.nymity.ch/pdf/Frolov2020a.pdf#page=5)）。

## 独特的TLS ClientHello指纹

2020年5月20日，[@p4gefau1t](https://github.com/p4gefau1t) [报告](https://github.com/v2ray/discussion/issues/704)V2Ray客户端发送的TLS ClientHello有着[非常独特的指纹](https://tlsfingerprint.io/id/8c48b95f67260663)。 这样独特的指纹不但给了审查者怀疑和识别V2Ray客户端和服务端的机会， 而且还允许审查者在不造成大量误伤（collateral damage）的情况下[精确的](https://github.com/v2ray/discussion/issues/704#issuecomment-636351112)封锁V2Ray产生的TLS流量。

[@p4gefau1t](https://github.com/p4gefau1t)进一步识别出这些独特的指纹是由[一套硬编码的密码套件](https://github.com/v2ray/v2ray-core/blob/edb4fed387d27890902e7ee97aae0d97292f912b/transport/internet/tls/config.go#L176-L230)造成的。 具体而言， 当`AllowInsecureCiphers`的值为默认的`false`时， 那套硬编码的密码套件就会被使用。

V2Ray开发者[@xiaokangwang](https://github.com/xiaokangwang)让V2Ray自`v4.23.4`起，使用go-tls库默认的设置来[缓解](https://github.com/v2ray/v2ray-core/issues/2542)这一弱点 （更多补丁细节请见 [#2510](https://github.com/v2ray/v2ray-core/pull/2510)，[#2512](https://github.com/v2ray/v2ray-core/pull/2512)和[#2518](https://github.com/v2ray/v2ray-core/issues/2518)。 [@tomac4t](https://github.com/tomac4t)总结了一个表格， 里面使用[tlsfingerprint.io](tlsfingerprint.io)来比较不同版本及配置下V2Ray使用的[ClientHello的指纹的流行程度](https://gist.github.com/tomac4t/efd739d197f9f864a10f39c01d5c893f)。 但结果显示， 即使是已经使用go-tls库默认设置后的指纹， 在实际的流量统计中似乎仍是很少见的。

据我们所知， 在2019年11月， [@klzgrad](https://github.com/klzgrad/)就曾[调查过V2Ray v4.21.3](https://gist.github.com/klzgrad/25b2612d266a450abca6129a7ca595a4#v2ray-v4213) 以及其他基于TLS的翻墙工具的ClientHello指纹。 其[结果](https://gist.github.com/klzgrad/25b2612d266a450abca6129a7ca595a4)显示， 很多工具在当时被调查的版本中的ClientHello有着罕见的指纹。

旁注：

## 伪装成HTTP服务器的企图失败

2020年6月2日，[@p4gefau1t](https://github.com/p4gefau1t) [报告](https://github.com/v2ray/v2ray-core/issues/2537) V2Ray没能成功模仿真正的HTTP通讯。 汇报了的问题有两个：

1.  V2Ray客户端和服务端都只会在第一个TCP数据包中加入HTTP头部。这种奇特的TCP连接容易被检测和怀疑。
2.  V2Ray服务器对任何请求错误都笼统的回复一个[硬编码的500回复](https://github.com/v2ray/v2ray-core/blob/85633ec25ea06aff31fb1754992ebf86a3a737bd/transport/internet/headers/http/http.go#L236-L263)。

介于2013年起[鹦鹉已死](https://people.cs.umass.edu/~amir/papers/parrot.pdf)， 与其尝试复活鹦鹉， 不如改用真正的HTTP引擎。 现在的许多翻墙软件已经使用了`应用前置`（`application fronting`）的概念，这些软件包括但不限于[forwardproxy](https://github.com/caddyserver/forwardproxy)， [naiveproxy](https://github.com/klzgrad/naiveproxy) 和[trojan](https://github.com/trojan-gfw/trojan)。

## 贡献

文中提到的一切贡献、成果均属于该工作的原作者。

## 致谢

我们想在此感谢[@studentmain](https://github.com/studentmain)和[@p4gefau1t](https://github.com/p4gefau1t)。 他们帮助我们理解了他们所提出的重放攻击的许多细节，并与我们分享了他们对于下一步工作的建议。 我们还想感谢David Fifield和@studentmain对于这篇总结给出的详细反馈和建议。

## 联系

这篇报告首发于[GFW Report](https://gfw.report/blog/v2ray_weaknesses/zh/)。我们还在[net4people](https://github.com/net4people/bbs/issues/36#issuecomment-644929739)和[ntc.party](https://ntc.party/t/summary-on-recently-discovered-v2ray-weaknesses/556)同步更新了这篇报告。

下一步，我们将调查GFW是否已经使用了针对V2Ray的主动探测攻击。与此同时，如果您有任何翻墙服务器被封锁，我们都欢迎您或公开的或私下的与我们分享您的配置。因为这会帮助我们快速定位许多问题的根源。我们私下的联系方式可见[GFW Report](https://gfw.report)的页脚。

* * *