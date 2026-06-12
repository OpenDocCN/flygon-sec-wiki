<!--yml
category: 防火墙
date: 2026-06-12 18:59:58
-->

# 积至公司与MESA实验室：防火长城史上最大规模文件外泄分析

> 来源：[https://gfw.report/blog/geedge_and_mesa_leak/zh/](https://gfw.report/blog/geedge_and_mesa_leak/zh/)

2025 年 9 月 11 日星期四，中国防火长城（GFW）经历了其历史上最大规模的内部文件泄露事件。超过 500 GB 的源代码、工作日志和内部通信记录被泄露，揭示了 GFW 的研发和运作细节。

此次泄露源自 GFW 背后的一支重要技术力量：积至（海南）信息技术有限公司（Geedge Networks Ltd.） 及中国科学院信息工程研究所（简称：中科院信工所）第二研究室的处理架构组 MESA 实验室。文件显示，该公司不仅为新疆、江苏、福建等地政府提供服务，还在“一带一路”框架下向缅甸、巴基斯坦、埃塞俄比亚、哈萨克斯坦以及一个未被识别的国家输出审查与监控技术。

鉴于这些泄露材料高度敏感，我们强烈建议任何选择下载和分析它们的人采取适当的操作安全防护措施，并假设这些文件可能包含潜在的风险内容（例如在不安全的环境中存储或分析它们可能会导致监控或恶意软件攻击）。请考虑仅在无网络连接的隔离环境中分析这些文件（如虚拟机或不联网的主机）。

中国的防火长城GFW是一系列互联网审查系统的统称。其背后的研发，运维，硬件，管理，各司其职、相互协作。除了固定的政府部门（如国家互联网应急中心），根据每次的合同和招标，还会有不同的单位作为技术支撑。此次泄露源自 GFW 背后**研发力量**中的重要一支：积至和 MESA。MESA 实验室就是中国科学院信息工程研究所（简称：中科院信工所）第二研究室的处理架构组。

这一切最早追溯到被誉为"防火长城之父“的方滨兴来到北京，先是在2008年底成立信息内容安全国家工程实验室（NELIST），依托中国科学院计算技术研究所。后在2012年起依托单位改为中国科学院信息工程研究所。2012年1月，部分NELIST的人马在信工所组建团队，并于2012年6月将团队正式定名处理架构团队，英文名称 MESA（Massive Effective Stream Analysis）。下面是 MESA 自我介绍的一段摘抄：

到了2018年，方滨兴此时已经在海南也站稳了脚跟，积至（海南）信息技术有限公司（Geedge Networks Ltd.）就在同年诞生了。方滨兴作首席科学家，“核心研发人员来自中科院、哈尔滨工业大学、北京邮电大学等高校和科研院所“。其中大部分血液就来自 MESA，比如郑超担任CTO。细心的读者会发现，MESA 大事记中的许多导师和学生都会在泄漏的积至公司的git commit中出现。

此次泄漏文件的非源代码部分已经被多个专业团队详细分析，这些团队包括但不限于 InterSecLab、国际特赦组织（Amnesty International）、Justice for Myanmar、环球邮报（The Globe and Mail）、Der Standard，以及 Follow the Money。

> 以下是三篇新闻文章的笔记和重点。
> 
> 泄露的内部文件显示，Geedge 直接与各国政府和互联网服务提供商（ISP）合作，安装用于审查和监控的产品。他们提供的功能包括追踪用户位置和网络访问历史，以及封锁服务和翻墙系统。
> 
> > ……超过 10 万份与 [Geedge Networks](https://www.geedgenetworks.com/) 有关的内部文件泄露。这是一家鲜为人知的中国公司，却悄然在开发防火长城和向世界各国政府提供类似的审查能力方面扮演了关键角色…… 这些文件不仅揭示了 Geedge 如何向专制客户出口先进的审查技术，使他们获得本来不具备的能力，也揭示了防火长城本身的演变。 其中包括过滤网站和应用程序的方案、实时在线监控、对特定地区进行网络限速或断网、通过在线痕迹识别匿名用户，以及封锁包括 VPN 在内的翻墙工具。
> 
> Geedge 至少涉足五个国家：哈萨克斯坦、埃塞俄比亚、缅甸（[#369](https://github.com/net4people/bbs/issues/369)）、巴基斯坦，以及一个仅以代号 A24 出现的未公开国家。哈萨克斯坦是其 2018 年成立后的早期客户。
> 
> > Geedge 成立于 2018 年后不久，首批客户之一便是哈萨克斯坦政府，公司向其出售了旗舰产品「天狗安全网关」（TSG），该产品具备类似中国防火长城的功能，监控和过滤所有经过的网络流量，并检测和阻止翻墙行为。 同样的工具也在埃塞俄比亚和[缅甸](https://www.justiceformyanmar.org/stories/the-myanmar-juntas-partners-in-digital-surveillance-and-censorship)部署，在缅甸军政府禁止 VPN 的过程中发挥了关键作用。在许多情况下，Geedge 与其他私营公司合作，包括埃塞俄比亚的 Safaricom、缅甸的 Frontiir 和 Ooredoo 等 ISP，共同实施政府审查。文件显示，这些 ISP 都未回应置评请求。
> 
> 缅甸在 “正义缅甸” 报告 [《监控丝绸之路》](https://www.justiceformyanmar.org/stories/silk-road-of-surveillance) 中被特别提及。巴基斯坦则在国际特赦组织报告 [《控制的阴影》](https://www.amnesty.org/en/documents/asa33/0206/2025/en/) 中被特别提及。
> 
> 关于巴基斯坦，这篇 *环球邮报* 的文章指出，Geedge 在 Sandvine 撤出后，把自己的新系统（包括 TSG）安装在原有 Sandvine 留下的设备上。（Sandvine 现已更名为 AppLogic。）
> 
> > Sandvine 于 2023 年在外界日益严格的审查下退出巴基斯坦，很快就被 Geedge 替代。文件显示，Geedge 不仅利用了现有的 Sandvine 安装设备，还提供了新技术，驱动巴基斯坦的“网络监控系统”（该国的国家级防火墙）。 AppLogic 在声明中表示，他们并不了解 Geedge，且任何被重新利用的硬件都是“现成设备”，不具备 Sandvine 解决方案所独有的特殊功能。
> 
> 文章引用了同一份招聘广告（曾在 [#369 (comment)](https://github.com/net4people/bbs/issues/369#issuecomment-3254638017) 出现），其中提到另外四个国家：马来西亚、巴林、阿尔及利亚和印度：
> 
> > Geedge 最近发布的一则[招聘广告](https://archive.ph/GYfv4)还提到了“一带一路”。该广告寻找“能说英语或其他外语”的候选人，并愿意赴“巴基斯坦、马来西亚、巴林、阿尔及利亚和印度”进行 3 至 6 个月的商务出差。
> 
> 除了海外，文件显示 Geedge 也在新疆、江苏和福建省有部署。这可能意味着一种更加分布式、区域化的防火墙系统，类似河南的情况，见 [#416](https://github.com/net4people/bbs/issues/416) 和 [《一堵墙后的墙》](https://gfw.report/publications/sp25/en/)。
> 
> Geedge 与中国科学院大学的 MESA 实验室关系密切。我们之前在 [#471 (comment)](https://github.com/net4people/bbs/issues/471#issuecomment-2803829013) 的读书组帖子中提到过 MESA 的“SAPP”网络分析平台。Geedge 的首席技术官 [郑超](https://github.com/net4people/bbs/issues/369#issuecomment-2195455424) 是 MESA 2012 年 1 月的[联合创始人](https://web.archive.org/web/20181019035422/https://www.mesalab.cn/f/team/event)。
> 
> > Geedge 文件中自豪地称方某为“防火长城之父”。公司其他高管，如 CEO 王远地和 CTO 郑超，被列为[互联网审查相关论文的合著者](https://ieeexplore.ieee.org/author/37085413301)，以及 [Geedge 提交的专利](https://patents.google.com/?assignee=Zhongdian+Jizhi+Hainan+Information+Technology+Co+Ltd) 的发明人。公司与中科院 [MESA 实验室](https://web.archive.org/web/20241202193832/https://mesalab.cn/)保持紧密关系，文件显示双方人员有定期合作。 一位 MESA 研究员曾记录 2024 年 7 月新疆会议的内容，与会者谈到利用技术“打击翻墙工具”，并建立“新疆分中心”作为“反恐先锋”和“省级能力示范”。
> 
> 公司对翻墙系统和 VPN 进行专门研究，以便封锁它们。
> 
> > 文件显示，公司员工致力于对许多常见工具进行逆向工程，并寻找封锁方法。其中一组文件列出了九个商业 VPN 已被“解决”，并提供了识别和过滤流量的多种方式。这些功能与防火长城长期展示的能力一致，目前大多数商业 VPN 在中国境内无法访问，许多专门的翻墙工具也难以使用。
> 
> 本文列出了 Geedge 技术的其他功能，除了追踪用户和屏蔽访问之外，还包括：在 HTTP 会话中注入恶意代码，以及直接发起 DDoS 流量攻击。
> 
> > Geedge Networks 提供的技术极其强大，[Intersec Lab 的 IT 安全专家的分析](https://interseclab.org/research/8)表明，它们能让当局在特定地区（如抗议期间）监控个人的数据流量。它们可以精准识别并封锁特定 VPN，而用户此前一直依赖 VPN 来绕过当局的数字审查。甚至可以向网站插入恶意代码，或发起 DDoS 攻击，使特定网站瘫痪。
> 
> 文中也提到 Geedge 软件部署在巴基斯坦 Sandvine 硬件上。显然，Geedge 特别强调软硬件解耦。
> 
> > 后来，加拿大公司 Sandvine 向巴基斯坦提供了一套系统，使当局能够封锁不良网站。2023 年，Sandvine（后更名 Applogic Networks）退出巴基斯坦，但显然留下了部分硬件。 调查显示，这些硬件至少在最初被 Geedge Networks 重新利用。Applogic Networks 告诉《标准报》，他们对此毫不知情，并强调其技术无法解密用户数据或植入间谍软件。
> 
> 法国公司 [泰雷兹集团](https://en.wikipedia.org/wiki/Thales_Group)为 Geedge 提供许可管理。Geedge 至少使用过一个德国服务器进行软件下载分发。（或许是为了避免若服务器在中国境内会被防火长城干扰。）
> 
> > 一家法国公司——可能是无意中——成了 Geedge 的帮手。泰雷兹集团出售的软件可用于许可证管理。Geedge 显然利用该软件来控制其售出的产品，比如限制软件的使用时长。 泰雷兹集团向《标准报》确认这家中国公司是其客户之一。但 Geedge 软件并不依赖法国产品才能运行。泰雷兹称自己与监控无关。 此外，Geedge 还利用德国服务器，通过下载链接向客户分发软件。其动机尚不清楚，但众所周知，中国防火长城正日益限制海外访问中国网站。德国相关部门未对《标准报》的询问作出回应。
> 
> 本文概述了 Geedge 的多种产品，这些产品可能打包出售，也可能单独提供。“Cyber Narrator” 是一种高层监控面板，非技术用户也能直接使用。“天狗安全网关”（TSG）是实际执行网络监控和封锁的设备。TSG Galaxy 是数据存储与分析流水线。“Network Zodiac” 是一个对其他系统进行管理与监控的工具。
> 
> > Geedge 的产品组合包括多种技术。[InterSecLab 的数据分析](https://interseclab.org/research/8)显示，“Cyber Narrator”是客户的主要界面，即使非技术人员也能利用它监控特定区域的互联网用户（如示威期间）。 其次是被认为是旗舰产品的“天狗安全网关”，它能封锁 VPN，还能向网站注入恶意代码或发起攻击。 另一个产品是“TSG Galaxy”，用于存储收集到的用户数据；而“Network Zodiac”则监控其他所有系统，并报告错误。
> 
> Geedge 的设备（如 TSG）可能安装的范围远超此次泄露提及的国家，因为其官网声称“服务 40+ 全球运营商”：
> 
> > 据中国媒体报道，2024 年 Binxing 在一次演讲中宣布，公司计划“拓展国际市场”，推动中国技术全球化。文件显示，缅甸、巴基斯坦、埃塞俄比亚和哈萨克斯坦都至少持有旗舰产品 TSG 的许可证。此外，Geedge 官网宣称服务“40+ 全球运营商”，暗示其影响远超泄露文件所示。
> 
> 一份 2023 年 2 月的 Geedge 工单涉及埃塞俄比亚的社交媒体封锁，与当时已知的封锁情况（[#210](https://github.com/net4people/bbs/issues/210)）相符。
> 
> > 2023 年 2 月，全国抗议浪潮期间，Geedge 的一份工单显示其专家受召处理与 YouTube、Twitter 等社交媒体平台相关的问题。同一时间段，外界也报道了这些平台的封锁情况。
> 
> 至少有一份 Jira 工单显示了电子邮件明文拦截的证据：
> 
> > 内部文件显示，Geedge 的工具（包括 TSG）已在[巴基斯坦]使用——至少在一起案例中，某全球航运公司与一家巴基斯坦公司的电子邮件通信被拦截。

> [InterSecLab 报告](https://interseclab.org/research/the-internet-coup/)（[PDF 76 页](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf)）写得非常好，包含大量具体的技术细节。它更详细地解释了 Geedge 的产品套件、其与 [MESA](https://github.com/net4people/bbs/issues/471#issuecomment-2803829013) 研究实验室的关系，以及在各国的部署时间线。
> 
> [p.7](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=7)
> 
> > 基于对一批超过 100,000 份 Geedge Networks 文档泄露材料的分析（该材料被分享给 InterSecLab），本研究揭示了 Geedge Networks 系统的功能与能力，包括深度包检测、对移动用户的实时监控、对互联网流量的精细化控制，以及可按地区定制的审查规则。泄露材料还揭示了 Geedge Networks 与学术实体 Mesalab 的关系，以及他们与客户政府之间的互动。其对数据主权的影响十分重大，我们的发现对监控与信息控制技术商品化的趋势提出了担忧。 本研究审视了 Geedge Networks 在多个国家的系统的最新进展，包括其已知的部署时间线。通过分析该公司的内部文档，InterSecLab 得以记录商业化“国家防火墙”的扩张，并在这些系统扩散的背景下，推测其对全球互联网未来的影响。
> 
> ## Geedge 产品
> 
> 天狗安全网关（TSG）是多用途防火墙与监控设备的名称。TSG 包含所有主要的 DPI、过滤、跟踪、限速与攻击功能。TSG 提取的数据会进入 TSG Galaxy 进行存储与分析。
> 
> [p.22](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=22)
> 
> > TSG 的能力非常广泛，具备通过深度包检测进行监控与审查、识别并封锁 VPN 和各种翻墙工具、限速流量、监控、跟踪、标记并封锁个体互联网用户，以及以恶意软件感染用户等功能。
> 
> TSG 可以安装在称为 [TSGX](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=29) 的一体化硬件平台上，也可以搭配客户的现有硬件运行。（报告称在巴基斯坦，Geedge 的 TSG 安装在 Sandvine 留下的设备上。）TSG 运行名为 [TSG-OS](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=30) 的操作系统，其基于 Red Hat Enterprise Linux 与 Docker（参见 郑超 等人的《[“A Flexible and Efficient Container-based NFV Platform for Middlebox Networking”](https://github.com/net4people/bbs/issues/282)》）。
> 
> 可按需安装任意数量的 TSG 节点，名为 [Ether Fabric](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=31) 的分流设备会按 5 元组哈希在所有节点之间做负载均衡。用于管理 TSG 集群的系统称为 [Central Management](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=30) 或“毕方”（Bifang）。
> 
> TSG 依赖名为 MARSIO 的用户态网络系统。也就是说，它自行进行路由与报文处理，为了效率而绕过 Linux 内核。它使用了 [DPDK](https://www.dpdk.org/)。（再次参见 2018 年的《[“A Flexible and Efficient Container-based NFV Platform for Middlebox Networking”](https://github.com/net4people/bbs/issues/282)》。）
> 
> ### TSG Galaxy
> 
> TSG Galaxy 是一个数据存储与汇聚系统（[抽取-转换-加载](https://en.wikipedia.org/wiki/Extract%2C_transform%2C_load) 的 [数据仓库](https://en.wikipedia.org/wiki/Data_warehouse)），保存诸如 TCP 与 UDP 会话及其协议（包括 TLS、SIP、DNS、QUIC）的元数据。Galaxy 中的信息可由 Cyber Narrator 进行查询。
> 
> [p.20](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=20)
> 
> > TSG Galaxy 是 Geedge Networks 的 ETL（抽取、转换、加载）数据仓库方案， 为互联网规模的大规模监控而设计，在客户国家收集并汇聚所有互联网用户及其在互联网上传输的数据的大量信息。 它构建在开源的 Apache Kafka 流处理平台之上，这是一种常见的数据处理软件，常被在线零售商和广告商用于提供客户分析。 针对本研究分析的泄露数据包括一份 TSG Galaxy 的 SQL 架构，表明 TSG Galaxy 用于存储全国范围内所有 TCP 与 UDP 会话的记录——这些传输协议广泛用于宽带与移动数据——以及所有的 SIP 会话。SIP 是一种用于 VoIP（网络语音通信）的协议，是大多数现代电话网络的基础。这意味着 TSG Galaxy 不仅允许监控互联网上的网络流量与内容，也允许监控电话通话。
> > 
> > TSG Galaxy 采用 IP 流信息导出（IPFIX）来分析流量，并使用深度包检测（DPI）提取元数据。通过 DPI，他们可以提取详细的指纹信息，包括 TLS 与 QUIC 的 SNI、DNS 查询以及电子邮件头。TSG Galaxy 还实现了连接指纹技术，例如 JA3 哈希，使 Cyber Narrator 能识别模式以帮助判断用户使用的操作系统以及连接所用的应用。这项技术可以用于识别用户是否使用 VPN 等翻墙工具来隐藏流量或绕过审查。在 TSG Galaxy 内，所有这些信息与来自互联网服务提供商的信息相结合，通过多种标识符（包括 IP 地址、用户的订户 ID、IMEI 与 IMSI）关联到具体的互联网用户。来自 TSG Galaxy 的元数据会被发送到一个数据库，客户可以通过 Cyber Narrator 进行查询。
> 
> Cyber Narrator 是一个为非技术用户设计的用户界面，用于查询与展示由 TSG 收集、存储在 TSG Galaxy 中的信息。可以在 Cyber Narrator 中控制对服务与协议的封锁，并提供查找访问过特定内容的用户标识符的功能。Cyber Narrator 使用名为 WebSketch 的远程服务，为 IP 地址等标识符添加来自第三方数据经纪商或 Geedge 自身研究的元数据注释。
> 
> [p.19](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=19)
> 
> > Cyber Narrator 是一款强大的工具，能够在个体客户层面跟踪网络流量，并可通过将活动与特定小区标识（cell ID）关联起来而实时识别移动用户的地理位置。该系统还允许政府客户查看聚合的网络流量。
> > 
> > ……Cyber Narrator 可以让客户政府与安全力量更容易地将使用翻墙工具或访问其他被其视为潜在恶意的应用或网页的个体用户标记出来。Cyber Narrator 的分析能力还可用于阻断对特定网站或 VPN 服务的访问。通过 Cyber Narrator，客户政府还可以识别在限制之前访问过相关内容或服务的个人。
> 
> Network Zodiac，或称哪吒（Nezha），是一个监控其他组件的系统，类似于 [Grafana](https://grafana.com/)。看起来 Network Zodiac 的仪表板具备 SSH 到任意其他主机（如某个 TSG 节点）的能力——显然，如果某台 Network Zodiac 主机被攻陷，这将带来巨大的风险集中。
> 
> [p.33](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=33)
> 
> > 与流行的开源解决方案相比，一个显著的差异化特性是其集成的 Web 终端，使网络管理员能够通过 SSH 远程连接到任意被监控的端点。该功能为客户提供了对网络设备进行故障排查和管理的直接访问。然而，它也使客户面临重大安全风险。在最糟糕的情况下，黑客可能获取对全国范围内部署的所有安全设备的访问权。
> 
> ## TSG 的能力
> 
> TSG 具备典型的多协议深度包检测与封锁能力，同时还具有令人意外的限速、注入、跟踪与攻防功能。
> 
> ### 镜像模式与在线模式
> 
> TSG 与 Ether Fabric 可以以旁路（“镜像”或“被动”）模式部署，也可以以内联（可阻断流量或“主动”）模式部署。
> 
> [p.37](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=37)
> 
> > Geedge 系统可在两种主要模式下部署——镜像与在线——以帮助控制互联网。在镜像模式下（文档中有时称为“被动”），数据通过网络 TAP 被镜像到 Geedge 设备。具体而言，该网络 TAP 是一种光旁路开关。数据包无需等待处理即可继续前往目的地。在此模式下，即便 Geedge 系统发生故障，互联网仍可继续运作。该模式的优势在于不会因处理延迟或拥塞而增加网络时延。在镜像模式下，客户无法阻止特定流量通过，不得不依赖数据包注入来阻断连接。 在线模式（文档中也称为“主动”模式）要求流量在继续前往目的地之前必须先通过 Geedge 设备……该模式的优势在于可以彻底阻止特定流量流经网络。通常，这是那些希望获得绝对控制的客户所选择的方案，但代价是可靠性与网络质量的下降。
> 
> 将其与 2024 年[巴基斯坦一位官员的说法](https://github.com/net4people/bbs/issues/510#issue-3334906527)对比：
> 
> > 但为了监控本地流量，新的防火墙将采用所谓的“内联网络”，其作用类似安检点，每个数据包都必须被检查，并被允许通过或被阻断——不同于仅观察并记录流量而不干预其流动的另一种机制。 这位 ISP 官员说，使用内联网络“必然会降低网速”。
> 
> ### 深度包检测
> 
> 报告提到的协议包括 HTTP、DNS、电子邮件、TLS、QUIC 与 SIP。
> 
> 可从 TLS 与 QUIC 中提取服务器名称指示（SNI）。（关于基于 QUIC SNI 的中国审查，参见《[“Exposing and Circumventing SNI-based QUIC Censorship of the Great Firewall of China”](https://gfw.report/publications/usenixsecurity25/en/)》。）
> 
> [p.20](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=20)
> 
> > 使用 DPI，他们可以提取详细的指纹信息，包括 TLS 与 QUIC 的 SNI、DNS 查询以及电子邮件头。
> 
> 若在客户端安装 MITM 证书，TLS 流量可被解密；否则 TSG 必须依赖对加密流量的分类启发式：
> 
> [p.23](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=23)
> 
> > TSG 能通过两种主要方法分析传输层安全（TLS）流量。第一种方法是使用中间人（MITM）技术进行完全解密，这需要订户安装自签名的根 CA 证书。第二种方法采用 DPI 与机器学习技术，从加密流量中提取元数据。后者更常用，因为它对互联网用户不可见，从而无需用户安装 CA 证书或配置任何代理设置。……负责实施 TLS MITM 攻击的组件被称为 Tiangou Frontend Engine（TFE）。
> 
> ### 流量限速
> 
> [p.25](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=25)
> 
> > TSG 集成了流量整形能力，能够对特定服务的流量进行优先级分配或限速，从而在不直接封锁的情况下降低服务质量。这可以通过直接的流量整形来实现，也可以通过应用差分服务代码点（DSCP）标记来实现，后者是限制或优先处理流量的行业标准。
> 
> ### 注入与修改
> 
> TSG 能够注入与修改流量。它既可以用于封锁目的，也可以用于以恶意软件感染用户，或诱使其对目标发动 DDoS 攻击，类似于[“大炮”（Great Cannon）](https://censorbib.nymity.ch/#Marczak2015a)。
> 
> [p.23](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=23)
> 
> > TSG 还能通过诸如伪造重定向响应、修改头部、注入脚本、替换文本与覆盖响应体等技术，实时修改 HTTP 会话。
> 
> [p.26](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=26)
> 
> > TSG 配备了在线注入能力，允许在通过网络传输的文件中插入恶意代码。Geedge Networks 明确表示该功能旨在在互联网流量经过 TSG 系统时插入恶意软件。 TSG 的在线注入能力系统允许对特定用户进行精细化定向，可对多种文件格式进行“即时”修改，包括 HTML、CSS 与 JavaScript，以及 Android APK、Windows EXE、macOS DMG 镜像与 Linux RPM 包。此外，TSG 还能修改多种图像格式（如 JPG、GIF、PNG 与 SVG）以及各种归档格式（如 ZIP 与 RAR），以及办公文档、PDF、JSON 与 XML 文件。Cyber Narrator 还提供了分析功能，能够识别最合适的劫持 URL 以感染特定个体。例如，它可以针对那些未使用 TLS 的、某人的高频访问网站。
> 
> [p.27](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=27)
> 
> > 在泄露数据中识别出的 Geedge Networks 最令人困惑的产品之一是 **DLL 主动防御（DLL Active Defence）**，这种产品通常可以在网络犯罪黑市中见到。乍看之下，它似乎是一个用于防御 DDoS 攻击的系统；然而更仔细的检视表明，它实际上是一个针对被视为政治上不受欢迎的网站与其他互联网服务发动 DDoS 攻击的平台。这看起来是 Geedge 自己实现的中国“大炮”，正如 2015 年 Citizen Lab 报告所描述的那样。[13](https://citizenlab.ca/2015/04/chinas-great-cannon/) DLL 通过利用互联网扫描来识别流量放大点（例如递归 DNS 服务器），这些放大点可作为反射式拒绝服务攻击的发射台。它使用 TSG 的在线注入能力，有效地“招募”不知情的用户计算机参与攻击，从而形成一个僵尸网络。这标志着首次有网络安全公司向客户提供本质上是“肉鸡租用”（booter）式的 DDoS 攻击服务。
> 
> ### 将网络流量归因到真实身份
> 
> [p.25](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=25)
> 
> > Sanity Directory（SAN）或用户信誉流量管理系统是一种订户感知系统，旨在将 TSG 与 ISP 现有的信令与 AAA（认证、授权与计费）协议（包括 RADIUS、3GPP 与 CGNAT）无缝集成。该集成有助于将流量流归因到真实身份。
> 
> [p.49](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=49)
> 
> > Geedge 的 Sanity Directory 组件的核心特性之一是将流量归因到特定的 SIM 卡。这不仅使大规模监控成为可能，也使在巴基斯坦和 Geedge 经营的其他国家对特定个人进行高度定向的微观监控成为可能。
> 
> ### 识别与封锁翻墙工具
> 
> Geedge 购买了 VPN 账户，并运营一个安装了 VPN 应用的移动设备集群，以研究其网络行为：
> 
> [p.24](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=24)
> 
> > TSG 还采用 DPI 全面识别与 VPN 与翻墙工具相关的协议，如 OpenVPN 与 WireGuard。随后，它允许客户与 Geedge Networks 合作制定规则集以封锁特定服务提供商；Geedge 运营一个移动设备农场，在受控环境中安装并运行 VPN 应用。
> 
> [p.63](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=63)
> 
> > 为了创建这些封锁规则，Geedge Networks 使用逆向工程，采用静态与动态分析。静态分析涉及反编译应用源代码以找到返回服务器列表的 API，从而对其进行封锁。动态分析则是在运行 VPN 应用的同时分析其网络流量，以识别可用于封锁的模式。 泄露证据显示，Geedge Networks 与流行的 VPN 提供商保持付费账户，用于分析并封锁其应用。TSG 硬件还可以识别流行的 VPN 协议，如 IPSec、OpenVPN 与 WireGuard。
> 
> [![一个控制面板截图，显示一串带编号的 VPN 名称：15033 Cyber Ghost VPN；15031 Hotspot VPN；15029 Opera VPN；15027 Ivacy VPN；15025 Urban VPN；15023 Gecko VPN；15021 TunnelBear VPN；15019 Atlas VPN；15017 Cyberghost-UDP；15015 Windscribe VPN；15013 Ultrasurf VPN；15011 Hide Me VPN；15009 Express VPN；还有更多未显示。底部文字为“Total: 4081”。有一个对话框勾选了六个应用：15031 Hotspot VPN；15027 Ivacy VPN；15023 Gecko VPN；15015 Windscribe VPN；15011 Hide me VPN；15003 Tor Browser；对话框下方有“Confirm to delete 6 items?” 的 Yes/No 按钮。](img/b1698806cc8e998f393dca747a06aaba.png)](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=25)
> 
> 有一个名为 AppSketch 的应用网络指纹数据库，包含大量具体应用（如各家 VPN 服务）的指纹。见上图截图。
> 
> 关于收集 AppSketch 指纹的脚注 10 提到了我们之前讨论过的 SAPP（[#471](https://github.com/net4people/bbs/issues/471)）与 Maat（[#444](https://github.com/net4people/bbs/issues/444)）等技术。
> 
> > 为了提取这些（AppSketch）指纹，Geedge 与 Mesalab 的学生使用了他们称为 tcpdump_mesa 的开源工具 tcpdump 的修改版。随后，这些指纹被转化为规则集，使用四种 DPI 系统之一：SAPP（Stream Analyze Process Platform，一种 C 语言的数据包解析与注入库）；Stellar（一个比 SAPP 抽象层更高的有状态防火墙插件平台）；或 Maat（一个声明式系统）。与 SAPP 与 Stellar 不同，Maat 在开发新规则时不需要编程知识。Maat 能匹配常见的连接指纹，包括 IP 地址、域名、TLS SNI、JA3/JA4 指纹——这些都以 JSON 文件形式指定。通过使用 Redis 数据库，Maat 规则会在 TSG 集群的各节点之间进行同步，从而确保规则应用的一致性。
> 
> 一个有趣且令人意外的能力是：通过观察既往已知 VPN 用户的行为来发现新的 VPN 端点。（让人联想到 MESA 成员参与的《[“Identifying VPN Servers through Graph-Represented Behaviors”](https://github.com/net4people/bbs/issues/455)》。）
> 
> [p.9](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=9)
> 
> > 此外，Geedge Networks 的产品能够将特定个人识别为已知的 VPN 用户。一旦这些已知的翻墙工具用户转向尚未被封锁的新服务提供商，Geedge Networks 就可以观察其流量，并利用其留下的“轨迹”来识别未来要封锁的新 VPN。
> 
> [p.26](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=26)
> 
> > 进一步地，系统可以将个体订户识别为已知的 VPN 用户，随后跟踪他们的互联网使用，并将任何未来未知的高带宽流量归类为可疑。这种个体化的分类可能导致在互联网用户切换到新的 VPN 提供商时，识别并封锁先前尚未识别的服务，从而不仅牵连该用户，也牵连使用该服务的所有其他用户。
> 
> 无法识别的高带宽流量也可能用于指导封锁：
> 
> [p.25](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=25)
> 
> > 即便 TSG 无法识别用户活动所对应的具体应用或服务，它也可以将任何异常的大流量标记为可疑。此后，系统可被配置为在预设时间（例如 24 小时）后封锁被标记的流量。该做法与对 GFW 的观测相对应：即便无法识别具体流量类型，GFW 也会在一定持续时间后封锁任何高带宽的加密流量[9](https://www.usenix.org/conference/usenixsecurity23/presentation/wu-mingshi)。
> 
> 报告（[p.63](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=63)）讨论了 Tor 的桥接、[Snowflake](https://github.com/net4people/bbs/issues/366) 与 [WebTunnel](https://github.com/net4people/bbs/issues/263)。报告暗示 Geedge 具备枚举 Tor 桥的办法，但尚不确定是内部能力还是外包所得。Cyber Narrator 的一张[宣传截图](https://gitlab.torproject.org/tpo/anti-censorship/censorship-analysis/-/issues/40043#note_3044257)包含“Snowflake”字样。泄露材料包含 MESA 学生关于 WebTunnel 的研究，但当时尚未发现封锁技术。
> 
> Geedge 有一个专门用于枚举 Psiphon 端点的工具，称为 Psiphon3-SLOK。它与 2024 年 5 月 Geedge 进入缅甸时在当地观察到的 Psiphon 连接变化相吻合。
> 
> [p.64](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=64)
> 
> > 根据泄露数据，Geedge Networks 似乎尝试通过开发一款名为 Psiphon3-SLOK 的内部工具来绕过这一防护措施。 我们与 Psiphon 团队交流得知，2024 年 5 月下旬，来自缅甸的用户数量急剧上升，客户端选择服务器的方式也发生了变化，这与服务器枚举与定向封锁的情况一致。此时段恰逢 Geedge 系统在缅甸部署。
> 
> ### 对客户网络的远程访问
> 
> 存储在 TSG Galaxy 中的客户数据竟然对 MESA 的学生与研究人员可访问，并可能被用于研究。
> 
> [p.21](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=21)
> 
> > 本研究的一个重要发现是：在政府客户现场收集到并存储于 TSG Galaxy 的全部互联网用户数据似乎对 Geedge Networks 的员工可用。数据还显示，真实客户数据的快照有时会被分享给与 Geedge Networks 关系密切的中国科学院 Mesalab。数据表明，Mesalab 的工程专业学生曾使用真实世界的客户信息开展研究，旨在更好地理解并阻断互联网审查规避。
> 
> [p.24](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=24)
> 
> > 此外，Geedge Networks 的员工似乎具备在其办公室内部创建一个 Wi-Fi 网络的能力，使任何设备都能远程连接到客户网络。该功能使他们能够在真实世界场景中验证封锁机制是否有效运行。
> 
> ## 向中国境外国家的部署
> 
> 报告对 Geedge 在哈萨克斯坦、埃塞俄比亚、巴基斯坦与缅甸的部署给出了详细的总结。关于巴基斯坦与缅甸，还有更详细的信息见 [Shadows of Control](https://www.amnesty.org/en/documents/asa33/0206/2025/en/) 与 [Silk Road of Surveillance](https://www.justiceformyanmar.org/stories/silk-road-of-surveillance)。
> 
> 部署 Geedge 设备涉及 Geedge 员工亲赴目标 ISP 的场所，与该 ISP 人员直接合作。（顺带一提，这一事实揭穿了像[缅甸的 Frontiir](https://github.com/net4people/bbs/issues/369#issuecomment-2899187182) 这样的 ISP 的谎言：当被问及时他们否认与 Geedge 有关，[p.53](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=53)。）
> 
> [p.35](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=35)
> 
> > 在一个新的国家或省份启动部署时，Geedge 员工会前往客户所在地，在政府与当地 ISP 所拥有的场所内安装硬件。当地 ISP 是 Geedge 系统搭建不可或缺的一环。ISP 需要在安装期间向 Geedge 员工开放其场所，并提供网络方案，说明 Geedge 硬件如何集成进 ISP 现有系统。用于采集与存储海量数据的 Geedge 硬件被安置在各个 ISP 的数据中心内。
> 
> 在泄露材料中，各国以代号标识。除一个代号（A24）外，其余均已对应到具体国家。多数情况下，代号由国家名称的首字母与两位年份组成（显然并不总与首次部署年份一致）。
> 
> ### 哈萨克斯坦（代号 K18、K24）
> 
> Geedge 成立于 2018 年。泄露材料显示，哈萨克斯坦政府是其第一位客户，始于 2019 年。报告将 Geedge 的部署与该国推动全国范围 TLS MITM 的愿景联系起来，这一点我们曾在 [#6](https://github.com/net4people/bbs/issues/6)、[#56](https://github.com/net4people/bbs/issues/56) 与 [#339](https://github.com/net4people/bbs/issues/339) 中见到。
> 
> [p.42](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=42)
> 
> > Geedge 的产品 Tiangou Secure Gateway（TSG）能够实施类似政府颁发根证书的攻击，这或许是 Geedge 最初接触哈萨克斯坦政府时的卖点之一。 一张日期为 2020 年 10 月 16 日的图片列出了一个国家中心及其他 17 个城市的 IP 地址，这些地点运行着三种 Geedge 产品：Bifang（集中管理）、Galaxy（TSG-Galaxy 的早期名称）与 Nezha（Network Zodiac 的旧称）。Geedge 的一份不完整的网络规划文档开始于 2020 年 9 月记录与某哈萨克国家中心相关的事件。该日志记录至 2022 年 10 月，并包含一张表，列出与项目相关的修订情况，包括日期、版本号、修改内容以及负责修改的作者。
> 
> ### 埃塞俄比亚（代号 E21）
> 
> Geedge 于 2021 年在埃塞俄比亚开展工作。
> 
> 本节直接点名了[郑超](https://github.com/net4people/bbs/issues/369#issuecomment-2195455424)：
> 
> [p.45](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=45)
> 
> > 一条 2022 年 12 月的日志条目记载：Geedge CTO 郑超批准了位于亚的斯亚贝巴的两座 Safaricom 区域数据中心的相关工作。
> 
> 我们已提到，TSG 可在镜像模式或在线模式下运行。报告声称，从镜像切换到在线可能先于一次断网，并将其与[2023 年 2 月的社交媒体封锁](https://github.com/net4people/bbs/issues/210)联系起来。
> 
> [p.46](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=46)
> 
> > Geedge 的修订日志显示，从镜像模式切换到在线模式与政府准备实施断网之间存在相关性。例如，切换到在线配置可能表明断网即将到来；从总体上看，镜像模式更优化于监控，而在线模式更适合断网。日志总计显示在埃塞俄比亚发生了 18 次切换为在线模式的变更，其中两次发生在 2023 年 2 月断网之前、位于 Safaricom 数据中心。
> 
> ### 巴基斯坦（代号 P19）
> 
> Geedge 于 2023 年进入巴基斯坦，同年 Sandvine 退出该国。在 [Shadows of Control](https://www.amnesty.org/en/documents/asa33/0206/2025/en/) 报告中，国际特赦组织将 Geedge 运营的防火墙称为 “WMS 2.0”（网络管理/监控系统 2.0），以与其所取代的早期版本 WMS 区分。
> 
> Geedge 在巴基斯坦的存在与[此前关于中国参与国家防火墙的报道](https://github.com/net4people/bbs/issues/510)相一致；巴基斯坦官员的表述与 Geedge 的 TSG 已知能力相吻合：
> 
> [p.48](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=48)
> 
> > 接受半岛电视台采访的一位巴基斯坦资深 ISP 高管所使用的语言与 Geedge 的营销材料高度契合。[51](https://www.aljazeera.com/news/2024/11/26/pakistan-tests-china-like-digital-firewall-to-tighten-online-surveillance) 这位未具名高管称，新的 WMS 不仅部署在国家互联网关口，也部署在移动服务提供商与 ISP 的本地数据中心。[52] 由于之前的系统只能监控出入境的内容，巴基斯坦无法审查由 Netflix 与 Meta 等运行的本地缓存 CDN 所托管的内容。对比 WMS 1.0 与 WMS 2.0，这位高管表示：“与 Sandvine 系统不同，新的基于 DPI 的系统现在能够监控本地互联网流量”，它使用“内联网络”，这也更可能降低用户的上网速度。该高管还指出，这项中国（Geedge）技术提供了在“细粒度层面”管理应用与网站的能力，特性优于 Sandvine。
> 
> Geedge 的 Sanity Directory 具备将网络行为归因到特定 SIM 卡的能力。在巴基斯坦，SIM 卡又与现实身份相绑定：
> 
> [p.49](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=49)
> 
> > ……自 2015 年起，该国每张发放给移动用户的新 SIM 卡都必须注册到特定用户名下，并与通过国家数据库与登记局（NADRA）登记的生物特征（包括指纹）绑定。人们需要 NADRA 档案才能获得医疗、银行与教育等基本服务。NADRA 档案还与其他数据库相连，如选民登记与税务记录，合在一起对每个公民形成一份全面记录。[[57]](https://www.amnesty.org/en/documents/asa33/0206/2025/en/) Geedge 的 Sanity Directory 组件的核心功能之一，就是将流量归因到特定 SIM 卡。
> 
> ### 缅甸（代号 M22）
> 
> 缅甸之所以重要，是因为这是 Geedge 首次在国外的工作被公开知晓，当时由 [Justice for Myanmar 报道](https://github.com/net4people/bbs/issues/369#issuecomment-2195258977)。
> 
> 除了此前报道过的 [Frontiir](https://github.com/net4people/bbs/issues/369#issuecomment-2899187182) 之外，泄露材料还列出了缅甸所有 ISP 的数据中心。此前，当被询问时，Frontiir 曾虚假否认参与任何监控项目。
> 
> [p.53](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=53)
> 
> > 该规划文档列出了该国所有 ISP（无论国营或私营）所在的数据中心。“四大” ISP（MyTel、Ooredoo、MPT 与 ATOM）在列，同时也列出了较小的服务商，如 Frontiir、Global Technology Group、Golden TMH Telecom、Stream Net、IM-Net、Myanmar Broadband Telecom、Myanmar Telecommunications Network Public Company Limited、Campana 与中国联通。 文档还包含所有 ISP 的链路测试报告。这些报告提供了 2024 年不同日期进行的网站连通性测试信息。测试目标似乎是评估各 ISP 网络上的审查效果。
> 
> > Frontiir 的一位发言人否认其曾“在其网络上构建、规划或设计过任何与监控相关的内容”。然而，泄露文档表明，Geedge 硬件安装在缅甸所有 ISP 的机房中，包括 Frontiir。
> 
> 有关政府希望封锁的应用与 VPN 列表的信息：
> 
> [p.54](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=54)
> 
> > 泄露文档还包含关于封锁 VPN、Tor（尤其是由 Tor 驱动的移动应用 Orbot）与 Psiphon 的详细信息。与埃塞俄比亚或哈萨克斯坦等其他客户国家提供的 VPN 封锁清单相比，缅甸的“欲封锁 VPN 清单”更长。文档记录了制定“高优先级应用”封锁规则的过程，其中包含 55 款应用，包括消息应用 Signal 与 WhatsApp。
> 
> ### 代号 A24
> 
> 有一位 Geedge 客户仅以代号 A24 为人所知。在泄露发生时，这段业务关系显然还处于早期阶段。
> 
> [p.55](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=55)
> 
> > 虽然泄露文档包含与本报告中所列已知客户国家相关的具体地点和/或 ISP，但与 A24 相关的数据并不包含这些可用于识别客户的信息。关于 A24 身份的唯一线索只有首字母 A 与年份 2024。 除此之外，信息显示，截至泄露发生时，A24 与 Geedge 的关系还处在早期。为向客户澄清两种模式的差异，进行了两次 Geedge 设备的概念验证部署：一次为镜像模式，一次为在线模式。
> 
> ## 中国的区域性防火墙
> 
> 报告显示 Geedge 参与了中国的区域（省级）防火墙建设，尤其是在新疆。
> 
> [p.9](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=9)
> 
> > 除与国际政府客户合作外，本研究还提供了证据，显示中国正在出现一种补充国家级“防火长城”的省级防火墙模式。Geedge Networks 正与中国多个地方政府合作构建省级防火墙，其审查规则可能因地区而异。InterSecLab 已识别出在新疆、福建与江苏的中国区域性省级防火墙项目。
> 
> ### 新疆（代号 J24）
> 
> [新疆](https://en.wikipedia.org/wiki/Xinjiang) 的代号为 J24。泄露文档直接指出：新疆的区域防火墙将作为中国全国部署的样板。
> 
> [p.56](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=56)
> 
> > 泄露材料包含 2024 年 6 月 22 日在中国科学院新疆分中心的一次讲话记录。该讲话的笔记（很可能由 Geedge 员工所记）写道，Geedge 的项目“旨在将区域中心打造成反恐的先锋力量，尤其是在翻墙压制方面”。笔记提到，“国家（防火墙）正从集中式向分布式演进”，而新疆区域中心旨在“成为可复制或可借鉴的省级（防火墙）建设样板”。
> 
> 与大多数其他 Geedge 部署相似，新疆的部署遵循一个由“中央指挥中心”连接“运营商”数据中心的结构。
> 
> [p.57](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=57)
> 
> > 与更早的项目相比，J24 规模更为庞大，且不再通过 ISP 作为终端用户来运转。相反，它遵循与 Geedge 在国外客户地区类似的结构：由一个“国家中心”（在新疆，Geedge 将其称为中央指挥中心）来统筹分布式的区域中心（Geedge 称之为运营商中心）。在 J24 下，这些运营商中心位于中国电信、中国移动、中国广电与中国联通的机房中。与所谓国家中心相似，中央指挥中心可以远程管理部署在运营商站点的监控设备。据一份文档显示，这些 ISP 的设施中共有 17 个运营商中心。
> 
> 新疆部署的需求体现出强烈且侵入性的监控，这与我们所了解的该地区压制状况相一致。
> 
> [p.57](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=57)
> 
> > 在题为 “CBNR-J24 需求组织” 的文档中，Geedge 概述了要在 Cyber Narrator 中为新疆部署加入的一系列功能。Geedge Networks 希望在 Cyber Narrator 中支持对用户互联网行为、生活方式模式与关系的归纳与分析功能。他们还希望加入根据目标交流对象构建关系图谱的能力，并按照用户所用应用或访问网站对人群进行分组。 未来开发需求还提到，加入检查连接至特定移动基站的用户能力，以通过这些基站进行位置三角定位，并检测在某一地区出现的大量人群聚集。 此外，项目计划加入创建地理围栏的能力，当特定个体进入指定区域时触发告警。还强调了查询历史位置信息以追踪过往活动轨迹的功能。Geedge 希望能够标记频繁更换 SIM 卡、拨打国际电话或使用翻墙工具与境外社交媒体应用的个人。 J24 项目还包括面向特定群体的功能。这些功能将允许在地图上显示被监控群体的地理分布，并检测群体成员在特定地点的异常聚集情况。这样可使操作员追踪并预判大型抗议与示威的形成。
> 
> ### 福建、江苏与其他省份
> 
> 有一些关于 Geedge 在[福建](https://en.wikipedia.org/wiki/Fujian) 与[江苏](https://en.wikipedia.org/wiki/Jiangsu) 开展工作的文档，但相关信息少于其他地区。
> 
> [p.58](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=58)
> 
> > 文档显示，Geedge Networks 于 2022 年在福建开展了类似的省级防火墙试点项目——福建是位于台湾海峡对岸的一个省份。然而，与其他部署相比，关于该项目的信息相对有限。泄露文档中，该试点没有代号，仅被称为“福建项目”。
> 
> [p.59](https://interseclab.org/wp-content/uploads/2025/09/The-Internet-Coup_September2025.pdf#page=59)
> 
> > 若干文档还提到江苏这个中国东部沿海省份。Geedge 与当地机关（江苏省公安厅，JPSB）合作的动机据称是打击网络诈骗。沟通记录显示，JPSB 对允许 Geedge 构建一套大数据集群持谨慎态度，更倾向于让 Geedge 将其工具部署在现有基础设施上。一个名为“江苏南京”的初始测试环境于 2023 年 2 月投入运行，“江苏反诈项目”似乎在 2024 年 3 月 15 日转入生产模式。

本项工作离不开多个组织和研究团体的贡献。我们特别感谢 InterSecLab、国际特赦组织 (Amnesty International)、Justice for Myanmar、环球邮报 (The Globe and Mail)、Der Standard 以及 Follow the Money 所做出的深入且严谨的工作。作为研究联盟的一部分，InterSecLab 在过去九个月中持续对 600 GB 的泄露数据进行索引、翻译、分析、解读和总结。这一努力对于揭示此次泄露的重要意义起到了不可替代的作用。这些组织的调查、报道和分析为未来的工作提供了重要的背景和洞见。