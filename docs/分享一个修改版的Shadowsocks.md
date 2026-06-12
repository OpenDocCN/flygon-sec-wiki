<!--yml
category: 防火墙
date: 2026-06-12 19:01:13
-->

# 分享一个修改版的Shadowsocks

> 来源：[https://gfw.report/blog/modified_shadowsocks/zh/](https://gfw.report/blog/modified_shadowsocks/zh/)

我们在这篇文章中发布和开源一个修改版的Shadowsocks。这个版本的Shadowsocks可以绕过当前GFW的检测以及封锁。我们首先介绍这个修改后的Shadowsocks的原理，再分享一个如何部署服务器和客户端的简单教程。我们还会介绍其他两种当前能够帮助Shadowsocks和VMess绕过当前GFW封锁的办法。

## 动机

我们在此时发表这个版本有三个目的：

1.  首先，我们想为中国网民提供一个（暂时）可行的翻墙方案。用增加翻墙协议多样性的方式，缓解10月3号以来的GFW对多种翻墙工具的大规模封锁。

2.  其次，我们想抛砖引玉地引起研究者和开发者的讨论。我们实证性的研究显示，当前的GFW已经可以精准地识别Shadowsocks，VMess，以及Obfs4这类完全加密协议（full-encrypted protocol）。我们估算GFW当前的流量检测算法会误伤约`0.6%`的非翻墙链接，而假阴性则低得几乎可以忽略不计。这迫切的需要我们群力群策地改进当前的协议。

3.  最后，我们想把这次发布当作一场实验，同时观察审查者和反审查社区在面对新的（反）审查事件时的反应速度。

## 为什么这个修改后的Shadowsocks可以规避GFW当前的检测和封锁？

[我们与其他研究人员合作发现](https://gfw.report/publications/usenixsecurity23/zh/)，当前的GFW会利用多种不同的规则来识别Shadowsocks，VMesss，以及Obfs4这类完全加密协议。其中一条规则就利用了这些加密流量的0比特与1比特的比例接近1:1的特性。因此，如果我们在加密流量中加入更多的0或1，再对比特序列进行重排，就可以达到改变原有比例特征，绕过检测和封锁的目的。

## 我怎么用这个修改版的Shadowsocks？

这个修改版的Shadowsocks基于[Shadowsocks-rust](https://github.com/shadowsocks/shadowsocks-rust)，我们还利用[Shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android)编译了apk文件供安卓用户使用。所有的客户端和服务端软件都可以在[这个branch](https://github.com/gfw-report/shadowsocks-rust/tree/low-entropy)和[这个release](https://github.com/gfw-report/shadowsocks-rust/releases)找到。

### 安装服务端

安装修改版的服务器的过程与安装任何其他`Shadowsocks-rust`服务器无异。

1.  首先你需要登陆你的远程服务器，然后下载[编译好的文件](https://github.com/gfw-report/shadowsocks-rust/releases)：

```
wget https://github.com/gfw-report/shadowsocks-rust/releases/download/v0.0.1-beta/shadowsocks-v1.15.0-alpha.9.x86_64-unknown-linux-gnu.tar.xz tar xvf shadowsocks-v1.15.0-alpha.9.x86_64-unknown-linux-gnu.tar.xz 
```

2.  接着你需要创建一个配置文件：

`sudo nano server_config.json`

将下面的配置文件复制粘贴。注意，你需要把里面的`ExamplePassword`替换成一个更强的密码。你可以用以下命令在终端生成一个强密码：`openssl rand -base64 16`。你也应该考虑更改服务器端口`8388`。

```
{  "server": "0.0.0.0", "server_port": 8388, "password": "ExamplePassword", "method": "aes-256-gcm" } 
```

将上方替换过密码的配置信息复制粘贴到配置文件后， 按`Ctrl + x`退出。 退出时，文本编辑器将问你`"Save modified buffer?"`，请输入`y`然后按`回车键`。

3.  你现在就可以运行二进制的服务器了。但是为了让它在你退出SSH后还能继续运行，你可以考虑建立一个`tmux`会话：

然后再运行服务器：

```
./ssserver -c ./server_config.json 
```

最后，按`Ctrl + b`再按`d`就可以脱离tmux会话了。

### 防火墙配置

我们使用`ufw`来管理Shadowsocks服务器的防火墙。

在基于Debian的服务器上，可以通过如下命令安装`ufw`：

```
sudo apt update && sudo apt install -y ufw 
```

然后开放有关`ssh`和`Shadowsocks-rust`的端口。 请注意，以下命令假设你在`server_config.json`中的`server_port`的值为`8388`。 如果你的`server_port`用了其他的值，请对以下命令作相应的修改：

```
sudo ufw allow ssh sudo ufw allow 8388 
```

现在我们启动`ufw`:

启动时如果弹出`Command may disrupt existing ssh connections. Proceed with operation (y|n)?`，请输入`y`并按回车键。

最后，请用`sudo ufw status`检查一下你的配置是否和下面的一样：

```
Status: active   To                         Action      From --                         ------      ---- 22/tcp                     ALLOW       Anywhere 8388                       ALLOW       Anywhere 22/tcp (v6)                ALLOW       Anywhere (v6) 8388 (v6)                  ALLOW       Anywhere (v6) 
```

### 客户端配置

下面是桌面版客户端的配置文件，记得`server`的值替换为你远程服务器的IP地址。如果你们是用了我们提供的安卓apk在手机上使用，那么配置就和往常的使用`Shadowsocks-android`的办法一样。

```
{  "server": "ExampleServerIP", "server_port": 8388, "password": "ExamplePassword", "method": "aes-256-gcm", "local_address": "127.0.0.1", "local_port": 1080 } 
```

### 不足

*   因为我们对Shadowsocks协议做了修改，因此暂不兼容其他Shadowsocks客户端和服务端。用户需要下载我们准备的客户端和服务端。
*   目前客户端只支持Windows，Linux，macOS，安卓手机，和安卓电视版。不支持iOS。我们欢迎有能力的iOS开发者们实现兼容这个修改的协议。也欢迎iOS开发者与我们联系，我们将分享其他绕过审查的办法供你们参考。
*   在加密方式上，修改版还不支持[Shadowsocks-2022](https://github.com/Shadowsocks-NET/shadowsocks-specs/blob/main/2022-1-shadowsocks-2022-edition.md)。这并非我们刻意不支持Shadowsocks-2022，只是时间和精力有限，还没来得及支持。我们推荐选择：`chacha20-ietf-poly1305`或者`aes-256-gcm`。
*   在实现方式上，我们直接在核心代码上进行了修改。这并非我们想要另起炉灶，维护一个Shadowsocks协议的分支版本，只是时间和经历有限，还没有把修改后的算法做成一个可供用户选择的选项。我们将与Shadowsocks开发者积极沟通，希望最终能以提供给用户选项的方式，将新协议合并到Shadowsocks中。

## 你们还知道什么其他的可以规避当前封锁的方法吗？

我们还知道两种目前可行的方案，他们都利用了另一种不同的GFW流量检测规则：

如果你是V2Ray用户，你可以开启`ExperimentReducedIvHeadEntropy`选项来避免GFW的检测和封锁。这个方案的好处是你无须在服务器进行任何修改。

如果你是Shadowsocks用户，[@database64128](https://github.com/database64128)还[实现了另外一种绕过审查的办法](https://github.com/shadowsocks/shadowsocks-org/issues/204#issuecomment-1266710067)。因为对协议做了修改，所以需要同时更新客户端和服务端。

## 感谢

我们感谢David Fifield对文章初稿的反馈。

## 联系我们

正如前文所说的，我们发帖的目的就是想引起用户，研究人员和开发者们的讨论。因此我们欢迎您或公开地或私下地与我们分享您的使用体验或想法。我们私下的联系方式可见[GFW Report](https://gfw.report)的页脚。

* * *