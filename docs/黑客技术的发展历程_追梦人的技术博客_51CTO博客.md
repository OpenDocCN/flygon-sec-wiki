<!--yml
category: 历史
date: 2022-11-04 11:29:38
-->

# 黑客技术的发展历程_追梦人的技术博客_51CTO博客

> 来源：[https://blog.51cto.com/wuhaoshu/840845](https://blog.51cto.com/wuhaoshu/840845)

**http://book.51cto.com/art/201204/330053.htm** 从黑客技术发展的角度看，在早期，黑客攻击的目标以系统软件居多。一方面，是由于这个时期的Web技术发展还远远不成熟；另一方面，则是因为通过攻 击系统软件，黑客们往往能够直接获取root权限。这段时期，涌现出了非常多的经典漏洞以及"exploit"。比如著名的黑客组织TESO，就曾经编写 过一个攻击SSH的exploit，并公然在exploit的banner中宣称曾经利用这个exploit入侵过cia.gov（美国中央情报局）。

下面是这个exploit 的一些信息。

```
1.  root@plac /bin >> ./ssh  4.  linux/x86 sshd1 exploit by zip/TESO (zip@james.kalifornia.com) - ripped from  5.  openssh 2.2.0 src  7.  greets: mray, random, big t, sh1fty, scut, dvorak  8.  ps. this sploit already owned cia.gov :/  10.  **please pick a type**  12.  Usage: ./ssh host [options]  13.  Options:  14.  -p port  15.  -b base   Base address to start bruteforcing distance, by default 0x1800,  16.  goes as high as 0x10000  17.  -t type  18.  -d           debug mode  19.  -o        Add this to delta_min  21.  types:  23.  0: linux/x86 ssh.com 1.2.26-1.2.31 rhl  24.  1: linux/x86 openssh 1.2.3 (maybe others)  25.  2: linux/x86 openssh 2.2.0p1 (maybe others)  26.  3: freebsd 4.x, ssh.com 1.2.26-1.2.31 rhl  
```

有趣的是，这个exploit还曾经出现在著名电影《黑客帝国2》中：

| [![](img/624b6ba922e674fd197f687e817d7572.png)](http://p_w_picpaths.51cto.com/files/uploadimg/20120415/213830107.jpg)  |
| 电影《黑客帝国2》 |

放大屏幕上的文字可以看到：

| [![](img/288f5c8588da41c9b42bf244ca6b1bd8.png)](http://p_w_picpaths.51cto.com/files/uploadimg/20120415/213936582.jpg)  |
| 电影《黑客帝国2》中使用的著名exploit |

在早期互联网中，Web并非互联网的主流应用，相对来说，基于SMTP、POP3、FTP、IRC等协议的服务拥有着绝大多数的用户。因此黑客们主要的攻击目标是网络、操作系统以及软件等领域，Web安全领域的攻击与防御技术均处于非常原始的阶段。

相对于那些攻击系统软件的exploit而言，基于Web的攻击，一般只能让黑客获得一个较低权限的账户，对黑客的吸引力远远不如直接攻击系统软件。

但是时代在发展，防火墙技术的兴起改变了互联网安全的格局。尤其是以Cisco、华为等为代表的网络设备厂商，开始在网络产品中更加重视网络安全，最终改变了互联网安全的走向。防火墙、ACL技术的兴起，使得直接暴露在互联网上的系统得到了保护。

比如一个网站的数据库，在没有保护的情况下，数据库服务端口是允许任何人随意连接的；在有了防火墙的保护后，通过ACL可以控制只允许信任来源的访问。这些措施在很大程度上保证了系统软件处于信任边界之内，从而杜绝了大部分的攻击来源。

2003年的冲击波蠕虫是一个里程碑式的事件，这个针对Windows操作系统RPC服务（运行在445端口）的蠕虫，在很短的时间内席卷了全球， 造成了数百万台机器被感染，损失难以估量。在此次事件后，网络运营商们很坚决地在骨干网络上屏蔽了135、445等端口的连接请求。此次事件之后，整个互 联网对于安全的重视达到了一个空前的高度。

运营商、防火墙对于网络的封锁，使得暴露在互联网上的非Web服务越来越少，且Web技术的成熟使得Web应用的功能越来越强大，最终成为了互联网的主流。黑客们的目光，也渐渐转移到了Web这块大蛋糕上。

实际上，在互联网安全领域所经历的这个阶段，还有另外一个重要的分支，即桌面软件安全，或者叫客户端软件安全。其代表是浏览器攻击。一个典型的攻击 场景是，黑客构造一个恶意网页，诱使用户使用浏览器访问该网页，利用浏览器中存在的某些漏洞，比如一个缓冲区溢出漏洞，执行shellcode，通常是下 载一个木马并在用户机器里执行。常见的针对桌面软件的攻击目标，还包括微软的Office系列软件、Adobe Acrobat Reader、多媒体播放软件、压缩软件等装机量大的流行软件，都曾经成为黑客们的最爱。但是这种攻击，和本书要讨论的Web安全还是有着本质的区别，所 以即使浏览器安全是Web安全的重要组成部分，但在本书中，也只会讨论浏览器和Web安全有关的部分。