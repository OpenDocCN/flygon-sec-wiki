<!--yml
category: 防火墙
date: 2026-06-12 19:01:21
-->

# 分享一个采用uTLS指纹的Trojan-go客户端

> 来源：[https://gfw.report/blog/updated_trojan_go/zh/](https://gfw.report/blog/updated_trojan_go/zh/)

[我们更新了trojan-go客户端的TLS指纹](https://github.com/gfw-report/trojan-go/releases)，使其与一些当下最流行的TLS指纹一致。我们希望这次更新可以缓解2022年10月3日以来的[针对基于TLS翻墙软件的大规模封锁](https://github.com/net4people/bbs/issues/129#issuecomment-1266617327)。

## 版本更新

[v0.10.10](https://github.com/gfw-report/trojan-go/releases/tag/v0.10.10):

*   在这个版本中，我们将trojan-go客户端的使用的uTLS从v1.1.5升级到了[v1.2.0](https://github.com/refraction-networking/utls/releases/tag/v1.2.0)。

[v0.10.9](https://github.com/gfw-report/trojan-go/releases/tag/v0.10.9):

*   在这个版本中，我们将trojan-go客户端的使用的uTLS从v1.1.3升级到了[v1.1.5](https://github.com/refraction-networking/utls/releases/tag/v1.2.0)。

[v0.10.8](https://github.com/gfw-report/trojan-go/releases/tag/v0.10.8):

*   在这个版本中，我们将trojan-go客户端的使用的uTLS从v1.1.2升级到了[v1.1.3](https://github.com/refraction-networking/utls/releases/tag/v1.1.3)。
*   新的版本的fingerprint支持新添加的Edge，Safari，360Browser和QQBrowser选项；还更新了原本已经支持的Chrome, Firefox，和iOS选项的TLS指纹。

[v0.10.7](https://github.com/gfw-report/trojan-go/releases/tag/v0.10.7):

## 客户端指纹

根据配置的不同，`v0.10.7`版本的trojan-go客户端会发送以下一种流行的Clienthello指纹。这些指纹已经不同于之前版本用[Go的标准库发送的TLS指纹](https://tlsfingerprint.io/id/ad63dbc630ad9475))：

## 我作为一名用户需要做什么？

*   您需要根据自己的操作系统，选择对应的客户端下载并更新。
*   您应该考虑将配置文件中的`sni`设置为服务器的域名。因为正如上表总结的，如果不配置SNI，客户端发送的Clienthello的指纹还是很特殊。
*   您**不需要**更新你的服务器。因为这次更新只对客户端做了改变。
*   您**不必**在配置文件中特意设置`fingerprint`。因为默认值(`Chrome`)已经是最流行的指纹了。

## 为什么我使用了最新版本的客户端但是服务器还是被封锁了？

您是否在用这里提供的客户端的同时还通过其他客户端（比如手机上的软件）连接了相同的服务器？如果是的话，那就不能排除端口被封锁的是其他客户端的指纹或行为导致的。

我们现在缺少用户的使用情况汇报，如果您可以肯定在一段时间内只用了我们提供的客户端，我们非常欢迎您汇报您的使用情况（被封锁或是没被封锁对我们来说同样重要）。

## 配置文件示例

```
{  "run_type": "client", "local_addr": "127.0.0.1", "local_port": 1080, "remote_addr": "your-domain-name.com", "remote_port": 443, "password": [ "your_awesome_password" ], "ssl": { "sni": "your-domain-name.com",  "fingerprint": "Chrome" } } 
```

## 致trojan-go开发者

我们无意另起炉灶维护一个分支版本的trojan-go。我们之所以发布这个release是为了用户能够立即下载使用编译后的客户端。一旦我们的pull request请求被采纳，我们将归档这个仓库。

## 感谢

我们感谢[uTLS](https://github.com/refraction-networking/utls)的开发者，因为没有他们持续不断的努力，我们不可能轻松地将trojan-go升级到使用最流行的TLS指纹。我们感谢Eric Wustrow帮助我们理解uTLS库。

* * *