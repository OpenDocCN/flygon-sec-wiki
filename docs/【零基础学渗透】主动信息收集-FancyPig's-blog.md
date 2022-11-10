<!--yml
category: 社会工程
date: 2022-11-10 10:35:02
-->

# 【零基础学渗透】主动信息收集-FancyPig's blog

> 来源：[https://www.iculture.cc/cybersecurity/pig=11160](https://www.iculture.cc/cybersecurity/pig=11160)

<figure class="wp-block-image size-full">![图片[1]-【零基础学渗透】主动信息收集-FancyPig's blog](img/c470debe7f1dae71c03d26ea563402cc.png)</figure>

## 相关声明

以下内容仅限于教育目的，请勿用于非法用途。

主动信息收集通常会与目标发生交互，无法避免的会留下痕迹！

## 学习目标

*   掌握端口扫描、目录扫描的概念
*   熟悉Nmap常见的命令
*   熟悉常见的扫描工具，如Goby、FFuf、masscan、御剑、在线扫描等工具
*   学习针对站点指纹收集的思路
*   了解社工的思路

## 在线学习

我们为您准备了在线视频学习，可以评论获取

除了视频之外，我们还为您准备了详细的笔记

## 端口扫描

*   端口定义
*   端口分类
*   端口查看
*   常用端口和常见端口漏洞利用方式

### 端口定义

网络安全领域中，我们说的端口，一般都指的是逻辑意义上的端口，即TCP/IP协议中的端口

端口号的范围从0到65535，比如用于浏览网页服务的80端口，用于FTP服务的21端口等等。

上面的说法可能过于抽象，我们这里举个例子你就懂了，我们在浏览器里输入的网址或是IP地址

譬如，`http://127.0.0.1:8000`

<figure class="wp-block-image size-large">![图片[2]-【零基础学渗透】主动信息收集-FancyPig's blog](img/5da548eeb41afa1952a2cb0ff1a5a0ab.png)</figure>

通常构成方式就是`IP:端口`，`域名:端口`，**冒号**后面的就是我们这里说的**端口**

**小提示：http开头的网址端口通常是80，https开头的网址端口通常是443**

### 端口分类

#### 1\. 按端口号分布划分

（1）**知名端口**（Well-Known Ports）知名端口即众所周知的端口号，范围从**0到1023**，这些端口号一般固定分配给一些服务。这也是我们为什么在启用服务的时候尽量要求选择大于1024的，就是怕占用了系统的一些服务

比如21端口分配给FTP服务，25端口分配给SMTP（简单邮件传输协议）服务，80端口分配给HTTP服务，135端口分配给RPC（远程过程调用）服务等等。

（2）**动态端口**（Dynamic Ports） 动态端口的范围从1024到65535，这些端口号一般不固定分配给某个服务，也就是说许多服务都可以使用这些端口。只要运行的程序向系统提出访问网络的申请，那么系统就可以从这些端口号中分配一个供该程序使用。 不过，动态端口也常常被病毒木马程序所利用。

#### 2\. 按协议类型划分

按协议类型划分，可以分为**TCP**、**UDP**、**IP**和**ICMP**（Internet控制消息协议）等端口。

下面主要介绍**TCP**和**UDP端口**：

（1）**TCP端口**

TCP端口，即传输控制协议端口，需要在客户端和服务器之间建立连接，这样可以提供可靠的数据传输。常见的包括FTP服务的21端口，Telnet服务的23端口，SMTP服务的25端口，以及HTTP服务的80端口等等。

（2）**UDP端口**

UDP端口，即用户数据包协议端口，无需在客户端和服务器之间建立连接，安全性得不到保障。常见的有DNS服务的53端口，SNMP（简单网络管理协议）服务的161端口，QQ使用的8000和4000端口等等。

### 查看端口

可以使用Netstat命令： 依次点击“开始→运行”，键入“cmd”并回车，打开命令提示符窗口。

在命令提示符状态下键入“netstat -a -n”，按下回车键后就可以看到以数字形式显示的TCP和UDP连接的端口号及状态

我们通常可以通过分析本地地址和外部地址，来挖掘出一些隐藏的服务，譬如恶意挖矿等等

<figure class="wp-block-image size-full">![图片[3]-【零基础学渗透】主动信息收集-FancyPig's blog](img/eb424936df90d382d5228ab8cb7ce387.png)</figure>

#### 一些参数

可以看到上面的图中有`协议栏`、`本地地址栏`、`外部地址栏`、`状态`，其中每一栏的详细参数如下

协议栏：显示TCP或UDP

本地地址栏：显示本地地址和对应服务的端口号

外部地址栏：外连地址和端口号

状态：

LISTEN：侦听来自远方的TCP端口的连接请求

SYN-SENT：再发送连接请求后等待匹配的连接请求

SYN-RECEIVED：再收到和发送一个连接请求后等待对方对连接请求的确认

ESTABLISHED：代表一个打开的连接

FIN-WAIT-1：等待远程TCP连接中断请求，或先前的连接中断请求的确认

FIN-WAIT-2：从远程TCP等待连接中断请求

CLOSE-WAIT：等待从本地用户发来的连接中断请求

CLOSING：等待远程TCP对连接中断的确认

LAST-ACK：等待原来的发向远程TCP的连接中断请求的确认

TIME-WAIT：等待足够的时间以确保远程TCP接收到连接中断请求的确认

CLOSED：没有任何连接状态

#### 命令详解

我们结合具体的场景给大家做了总结，可以参考

*   查看网络链接状态
*   查看路由表
*   查看网络数据统计
*   查看网络接口信息

### 常见端口和端口漏洞利用方式

<figure class="wp-block-table">

| **端口号** | **端口说明** | **攻击技巧** |
| --- | --- | --- |
| **21/22/69** | **ftp/tftp****：文件传输协议** | **爆破****嗅探**溢出；后门**** |
| **22** | **ssh****：远程连接** | **爆破****OpenSSH**；**28**个退格******** |
| **23** | **telnet****：远程连接** | **爆破****嗅探** |
| **25** | **smtp****：邮件服务** | **邮件伪造** |
| **53** | **DNS****：域名系统** | **DNS****区域传输**DNS**劫持**DNS**缓存投毒**DNS**欺骗**深度利用：利用**DNS**隧道技术刺透防火墙******************** |
| **67/68** | **dhcp** | **劫持****欺骗** |
| **110** | **pop3** | **爆破** |
| **139** | **samba** | **爆破****未授权访问**远程代码执行**** |
| **143** | **imap** | **爆破** |
| **161** | **snmp** | **爆破** |
| **389** | **ldap** | **注入攻击****未授权访问** |
| **512/513/514** | **linux r** | **直接使用****rlogin** |
| **873** | **rsync** | **未授权访问** |
| **1080** | **socket** | **爆破：进行内网渗透** |
| **1352** | **lotus** | **爆破：弱口令****信息泄漏：源代码** |
| **1433** | **mssql** | **爆破：使用系统用户登录****注入攻击** |
| **1521** | **oracle** | **爆破：****TNS**注入攻击**** |
| **2049** | **nfs** | **配置不当** |
| **2181** | **zookeeper** | **未授权访问** |
| **3306** | **mysql** | **爆破****拒绝服务**注入**** |
| **3389** | **rdp** | **爆破****Shift**后门**** |
| **4848** | **glassfish** | **爆破：控制台弱口令****认证绕过** |
| **5000** | **sybase/DB2** | **爆破****注入** |
| **5432** | **postgresql** | **缓冲区溢出****注入攻击**爆破：弱口令**** |
| **5632** | **pcanywhere** | **拒绝服务****代码执行** |
| **5900** | **vnc** | **爆破：弱口令****认证绕过** |
| **6379** | **redis** | **未授权访问****爆破：弱口令** |
| **7001** | **weblogic** | **Java****反序列化**控制台弱口令**控制台部署**webshell******** |
| **80/443/8080** | **web** | **常见****web**攻击**控制台爆破**对应服务器版本漏洞******** |
| **8069** | **zabbix** | **远程命令执行** |
| **9090** | **websphere****控制台** | **爆破：控制台弱口令****Java**反序列**** |
| **9200/9300** | **elasticsearch** | **远程代码执行** |
| **11211** | **memcacache** | **未授权访问** |
| **27017** | **mongodb** | **爆破****未授权访问 端口渗透总结** |

</figure>

一些实战案例参考[乌云Drops](https://www.iculture.cc/?golink=aHR0cHM6Ly93eS56b25lLmNpLw==)

## 端口扫描工具

结合上面的概念，我们给大家推荐一些工具

### 御剑

解压密码`www.iculture.cc`

我们除了御剑还给大家准备了其他的工具（工具来自网友分享）

<figure class="wp-block-image size-full">![图片[4]-【零基础学渗透】主动信息收集-FancyPig's blog](img/4f5fd0f3c02528364f541d7c368f73b1.png)</figure>

当我们获取到目标的真实IP后，下一步就是对目标IP进行**端口扫描**和**Banner信息识别**

<figure class="wp-block-image size-full">![图片[5]-【零基础学渗透】主动信息收集-FancyPig's blog](img/af5cadb71e86e47947d5bee6a4601378.png)</figure>

此款工具非常灵活，可批量扫描txt文本内的大量IP，也可扫描IP指定C段，可指定超时时间、每秒扫描的端口速度、自定义端口文件配置（指定扫描哪些端口）、探测指纹。

可以指定IP范围

<figure class="wp-block-image size-full">![图片[6]-【零基础学渗透】主动信息收集-FancyPig's blog](img/a4eecf287213063d56fa8fa896866e0b.png)</figure>

当然，下面也可以选择常见端口

<figure class="wp-block-image size-full">![图片[7]-【零基础学渗透】主动信息收集-FancyPig's blog](img/de6e161eb7bec6546d64edb1e49300e8.png)</figure>

端口配置也支持自定义

<figure class="wp-block-image size-full">![图片[8]-【零基础学渗透】主动信息收集-FancyPig's blog](img/f047e223a403f6f9328ca8c3632bb03c.png)</figure>

<figure class="wp-block-image size-full">![图片[9]-【零基础学渗透】主动信息收集-FancyPig's blog](img/64aac4855cb20e52dfd3406bd8e638d9.png)</figure>

### 在线扫描

优点：用网站提供的服务器去扫目标，不会暴露自己的IP

缺点：使用不如本地版灵活，功能不够丰富

<figure class="wp-block-image size-full">![图片[10]-【零基础学渗透】主动信息收集-FancyPig's blog](img/2590a292f0d14a9a79dc505c4dc0686f.png)</figure>

### masscan

masscan玩法比较多

*   扫描指定网段范围的指定端口
*   获取Banner
*   设置扫描时忽略一些网段
*   输出到指定文件中
*   设置扫描速度
*   用加载配置文件的方式运行
*   结果输出
*   命令行模式详解

放到隐藏部分，您可以评论获取

## Nmap的常见命令

之前我们有很多文章讲过Nmap了，可以参考之前的文章

更多笔记请评论获取隐藏内容

<figure class="wp-block-image size-full">![图片[11]-【零基础学渗透】主动信息收集-FancyPig's blog](img/ef17752cc9741c21356dabb12978ab5c.png)</figure>

<figure class="wp-block-image size-full">![图片[12]-【零基础学渗透】主动信息收集-FancyPig's blog](img/3c8fbd043ee8bc08d5705e48d9b8454d.png)</figure>

### 站点指纹收集

攻击者最常用的方法是首先覆盖目标的网络存在并枚举尽可能多的信息。

利用此信息，攻击者可以製定出准确的攻击方案，这将有效利用目标主机正在使用的**软件类型/版本中的漏洞。**

在攻防环境中信息收集总是非常重要的一个重要环节，多维度信息收集在红队攻防中绘制更完善的攻击面以及攻击思路流程。

#### 判断网站操作系统

Linux大小写敏感

Windows大小写不敏感

Linux操作系统大小写敏感，我们将网址url一些字母修改成修改大小写看网站是否还能正常访问，能访问就是windows服务器，不能则是Linux。

#### 确定网站采用的语言

如PHP / Java / Python等

找后缀，比如php/asp/jsp

<figure class="wp-block-image size-full">![图片[13]-【零基础学渗透】主动信息收集-FancyPig's blog](img/0b74ffb4b7b61da75eb82b9c4d74b09b.png)</figure>

JSESSIONID JSP

PSESSIONID PHP

#### 前端框架

如jQuery / BootStrap / Vue / React / Angular等查看源代码

#### 中间服务器

*   如 Apache / Nginx / IIS 等
*   查看header中的信息
*   根据报错信息判断
*   根据默认页面判断

#### Web容器服务器

如Tomcat / Jboss / Weblogic等

#### **后端框架**

*   根据Cookie判断
*   根据CSS / 图片等资源的hash值判断
*   根据URL路由判断，如wp-admin
*   根据网页中的关键字判断
*   根据响应头中的X-Powered-By

您可以通过使用Wappalyzer插件，快速获取网站中的一些资产信息

<figure class="wp-block-image size-large">![图片[14]-【零基础学渗透】主动信息收集-FancyPig's blog](img/3e44721cb1ee87ddcc120aeb1b7a0a2b.png)</figure>

*   **CDN信息**
*   常见的有Cloudflare、yunjiasu
*   **探测有没有WAF，如果有，什么类型的**
*   有WAF，找绕过方式
*   没有，进入下一步

**Nmap探测WAF有两种脚本**

一种是http-waf-detect。

命令：nmap -p80,443 –script=http-waf-detect ip

一种是http-waf-fingerprint。

命令：nmap -p80,443 –script=http-waf-fingerprint ip

**WAFW00F探测WAF**

命令：wafw00f -a 域名

*   **扫描敏感目录，看是否存在信息泄漏**
*   扫描之前先自己尝试几个的url，人为看看反应
*   使用爬虫爬取网站信息
*   拿到一定信息后，通过拿到的目录名称，文件名称及文件扩展名了解网站开发人员的命名思路，确定其命名规则，推测出更多的目录及文件名

## 目录扫描

目录扫描通常是帮助我们发现敏感信息或者敏感目录，譬如管理员的后台路径、数据库备份文件、网站数据打包文件等等

## 常见敏感文件或目录

**通常我们所说的敏感文件、敏感目录大概有以下几种：**

*   后台
*   robots.txt
*   数据库log
*   sitemap.xml
*   mysql.sql
*   licence.txt
*   Git
*   hg/Mercurial
*   svn/Subversion
*   bzr/Bazaar
*   Cvs
*   WEB-INF泄露
*   备份文件泄露、配置文件泄露
*   还可以使用awvs、burpsuite等爬虫获取

### 使用搜索引擎

site:xxx.xxx system

site:xxx.xxx 内部

site:xxx.xxx 系统

site:目标 admin

site:目标 login

site:目标 管理员登陆

site:目标 后台

site:目标 中心

site:目标 登录

site:目标 登陆

site:目标 管理中心

常见入口目标

关注度低的系统

业务线较长的系统

### 物理路径识别

报错

在处理报错信息的问题上如果处理不当，就可导致路径信息泄露，比如访问一些不存在的文件等思路。

•1.有动态URL的地方可以替换参数 替换参数值为不存在的，很多时候都能爆物理路径

•2.访问不存在的文件名 文件 或者改正常后缀为不支持的后缀。

IIS7.0以上，如果没有修改404页面，只要浏览web任意不存在的文件，都会直接暴出绝对路径。同理，thinkphp也有这个性质。 在id=1的注入点，使用各种不支持的字符，比如id=1’ id=? id=-1 id=\ id=/ 都有可能暴出绝对路径。 还有的时候传一些错误图片会报错 windows不支持的符号，?:<>之类的，还有windows不支持的文件名aux Windows服务器上传aux文件或者新建aux文件夹，因为不允许这种文件存在而报错泄露绝对路径。

•3.尤其是php框架写的站，上传很容易爆出物理路径，根据具体情况了，比如一次提交允许的后缀，整体提交时抓包改为不支持的后缀，放包，很多时候都能爆出物理路径。 有一部分都是sql语句报错，sql很多时候会爆物理路径，所以相信你已经会拓展了。

后台可以登录后台的话，后台首页一般都有服务器信息的，大部分情况下物理路径都在里面。

### 搜索引擎寻找报错

结合关键字和site语法搜索出错页面的网页快照，常见关键字有warning和fatal error。

注意，如果目标站点是二级域名，site接的是其对应的顶级域名，这样得到的信息要多得多。

Site:xxx.edu.tw warning

Site:xxx.com.tw “fatal error”

由于很多网站本身容错做的不好，会有一些暴露物理路径的界面，如果被搜索引擎收录了，那么可以通过搜索引擎来找到

在搜索引擎搜索 site:目标 关键字

我总结了一下常见的报错关键词

warning error module file not exist 数据库 配置出错 找不到包含文件 包含路径 路径为 select Warning: mysqli_query() expects parameter to be mysqli boolean given in on line directory in Fatal error require_once() Failed opening required include_path=

### 容器特性

很多，如：Apache Tomcat、Struts2、CMS、zabix、Nginx等等，例如Nginx的某版本解析漏洞，就可造成路径信息泄露。

•IIS大于6的版本，基本都是 导致他404就可以爆出物理路径、IIS名、IIS版本。这个很简单，随便访问个不存在的目录或文件就可以。

•nginx文件类型错误解析爆路径: 说明：要求Web服务器是nginx，且存在文件类型解析漏洞。有时在图片地址后加/x.php，该图片不但会被当作php文件执行，有可能爆出物理路径 [www.xxx.com/xx.jpg/x.php](https://www.iculture.cc/?golink=aHR0cDovL3d3dy54eHguY29tL3h4LmpwZy94LnBocA==)

•/etc/httpd/conf/httpd.conf

这是apache默认目录，最底下有一句

Load config files in the “/etc/httpd/conf.d” directory, if any. IncludeOptional conf.d/.conf

这代表，在/etc/httpd/conf.d目录下的所有.conf文件都会被加载，也就是说管理员可以在/etc/httpd/conf.d/.conf里面写网站目录。

所以最后读 /etc/httpd/conf.d/vhost.conf 成功读出网站绝对路径

思路就是先读 /etc/httpd/conf/httpd.conf 没有网站目录就看IncludeOptional conf.d/*.conf 看完就尝试读 /etc/httpd/conf.d/httpd.conf

/etc/httpd/conf.d/vhost.conf

/etc/httpd/conf.d/httpd-vhost.conf

/etc/httpd/conf.d/httpd.conf.bak 等等

### 文件泄露

通过遗留文件获得，比如 phpinfo.php info.php site.php 1.php a.php 一些探针文件啊都有，等等。在遗留文件中搜索 SCRIPT_FILENAME。

很多网站的根目录下都存在测试文件，脚本代码通常都是phpinfo()， 

如：

test.php ceshi.php info.php phpinfo.php php_info.php 1.php

phpmyadmin爆路径

一旦找到phpmyadmin的管理页面，再访问该目录下的某些特定文件，就很有可能爆出物理路径。

至于phpmyadmin的地址可以用wwwscan这类的工具去扫，也可以选择google。

/phpmyadmin/libraries/lect_lang.lib.php

/phpMyAdmin/index.php?lang[]=1

/phpMyAdmin/phpinfo.php load_file()

/phpmyadmin/themes/darkblue_orange/layout.inc.php

/phpmyadmin/libraries/select_lang.lib.php

/phpmyadmin/libraries/lect_lang.lib.php

/phpmyadmin/libraries/mcrypt.lib.php

XML处

一些XML限制或删除不完全，可导致服务器等信息泄露。

### 配置文件找路径

如果注入点有文件读取权限，就可以手工load_file或工具读取配置文件，再从中寻找路径信息（一般在文件末尾）。各平台下Web服务器和PHP的配置文件默认路径可以上网查，这里列举常见的几个。

Windows:

c:\windows\php.ini php配置文件

c:\windows\system32\inetsrv\MetaBase.xml IIS虚拟主机配置文件

如果有root读取文件的权限，或者任意文件读取漏洞，可以读取容器的配置文件，或者集成环境的固定web目录，判断集成环境，可以通过mysql的根目录判断，前面注入时说到的@@datadir: 常见配置文件: C:\Windows\system32\inetsrv\metabase.xml

C:\Windows\System32\inetsrv\config\applicationHost.config

C:\xampp\apache\conf\httpd.conf /var/www/conf/httpd.conf

常见集成环境默认目录，后面往往还有以域名命名的目录，比如:

C:\www\baidu

C:\Inetpub\wwwroot

C:\xampp\htdocs

D: \phpStudy\WWW

/home/wwwroot/ /www/users/

### CMS识别

CMS指纹识别又有很多方法，比如说**御剑指纹识别、Webrobot工具、whatweb工具、还有在线查询的网站**等等。

CMS在线指纹识别：

[http://whatweb.bugscaner.com/look/](https://www.iculture.cc/?golink=aHR0cDovL3doYXR3ZWIuYnVnc2NhbmVyLmNvbS9sb29r)

[http://finger.tidesec.net/](https://www.iculture.cc/?golink=aHR0cDovL2Zpbmdlci50aWRlc2VjLm5ldC8=)

[https://scan.top15.cn/web/](https://www.iculture.cc/?golink=aHR0cHM6Ly9zY2FuLnRvcDE1LmNuL3dlYi8=)

[https://www.yunsee.cn/](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cueXVuc2VlLmNuLw==)

[https://www.godeye.vip/index/](https://www.iculture.cc/?golink=aHR0cHM6Ly93d3cuZ29kZXllLnZpcC9pbmRleC8=)

也可在github上找一些高star的指纹识别工具

### ffuf的使用

### Goby的使用

参考[https://cn.gobies.org/](https://www.iculture.cc/?golink=aHR0cHM6Ly9jbi5nb2JpZXMub3JnLw==)

## 社工的思路

这个我们博客分享了太多太多了，可以参考

社区里也有网友分享自己的维权整个过程

《[【防骗课堂】第二课：被骗维权之路（写实）](https://www.iculture.cc/forum-post/8167)》