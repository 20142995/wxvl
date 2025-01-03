#  安全热点周报：黑客利用两个零日漏洞攻破 Palo Alto Networks 的数千个防火墙   
 奇安信 CERT   2024-11-25 09:05  
  
<table><tbody style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><tr bgless="lighten" bglessp="20%" data-bglessp="40%" data-bgless="lighten" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 4px solid rgb(68, 117, 241);visibility: visible;"><th align="center" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;background-color: rgb(254, 254, 254);font-size: 20px;line-height: 1.2;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(68, 117, 241);visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-size: 17px;visibility: visible;">安全资讯导视 </span></strong></span></th></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 国家数据局印发《可信数据空间发展行动计划（2024—2028年）》</p></td></tr><tr data-bglessp="40%" data-bgless="lighten" data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 《工业和信息化领域数据安全合规指引》正式发布</p></td></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 国家网络安全通报中心：多个与某大国政府有关的境外黑客组织持续攻击国内单位企业</p></td></tr></tbody></table>  
  
**PART****0****1**  
  
  
**漏洞情报**  
  
  
**1.Apple多个在野高危漏洞安全风险通告**  
  
  
11月20日，奇安信CERT监测到Apple发布新版本修复了存在在野利用的Apple多款产品输入验证错误漏洞(CVE-2024-44308)，远程攻击者可以诱骗受害者访问特制的网页并在系统上执行任意代码。Apple多款产品跨站脚本漏洞(CVE-2024-44309)，诱骗受害者点击特制的链接，在用户浏览器中在任意网站的上下文中执行任意HTML和脚本代码。鉴于这些漏洞已发现在野利用，建议客户尽快做好自查及防护。  
  
  
**2.Palo Alto Networks PAN-OS身份认证绕过漏洞安全风险通告**  
  
  
11月19日，奇安信CERT监测到官方修复Palo Alto Networks PAN-OS身份认证绕过漏洞(CVE-2024-0012)，PAN-OS设备管理Web界面中存在身份认证绕过漏洞，未经身份验证的远程攻击者可以通过网络访问管理Web界面，从而进行后续活动，包括修改设备配置、访问其他管理功能以及利用Palo Alto Networks PAN-OS权限提升漏洞(CVE-2024-9474)获取root访问权限。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为13039个，关联IP总数为2260个。目前该漏洞技术细节与PoC已在互联网上公开，鉴于此漏洞已发现在野利用，建议客户尽快做好自查及防护。  
  
  
**PART****0****2**  
  
  
**新增在野利用**  
  
  
**1.********Apple 多款产品输入验证错误漏洞(CVE-2024-44308)**  
  
**&****Apple 多款产品跨站脚本漏洞(CVE-2024-44309)**  
  
  
11月21日，苹果公司紧急发布了 macOS 和 iOS 的主要安全更新，以修复两个已被广泛利用的漏洞。  
  
苹果公司在其发布的公告中证实，这些漏洞是由谷歌的 TAG（威胁分析小组）发现的，正在基于英特尔的 macOS 系统上被积极利用。  
  
按照惯例，苹果的安全响应团队没有提供任何有关所报告的攻击或攻击指标 (IOC) 的详细信息，以帮助防御者寻找感染迹象。  
  
Apple 多款产品输入验证错误漏洞(CVE-2024-44308)，远程攻击者可以诱骗受害者访问特制的网页并在系统上执行任意代码。Apple 多款产品跨站脚本漏洞(CVE-2024-44309)，诱骗受害者点击特制的链接，在用户浏览器中在任意网站的上下文中执行任意 HTML 和脚本代码。  
  
本月初，朝鲜加密货币窃贼再次以 macOS 用户为目标，发起新的恶意软件活动，使用网络钓鱼电子邮件、伪造的 PDF 应用程序和一种新颖的技术来逃避 Apple 的安全措施。  
  
敦促整个 Apple 生态系统的用户紧急应用 iOS 18.1.1、macOS Sequoia 15.1.1和旧版本 iOS 17.7.2。  
  
  
参考链接：  
  
https://www.securityweek.com/apple-confirms-zero-day-attacks-hitting-intel-based-macs/  
  
  
**2.****Oracle Agile PLM Framework 授权不当漏洞(CVE-2024-21287)**  
  
  
11月21日，Oracle 警告称，影响敏捷产品生命周期管理 (PLM) 框架的高严重性安全漏洞已被广泛利用。该漏洞的编号为CVE-2024-21287（CVSS 评分：7.5），可在无需身份验证的情况下被利用来泄露敏感信息。  
  
该公司在一份公告中表示：“此漏洞无需身份验证即可被远程利用，也就是说，它可以通过网络被利用，无需用户名和密码。如果成功利用，此漏洞可能会导致文件泄露。”  
  
CrowdStrike 安全研究人员 Joel Snape 和 Lutz Wolf 因发现并报告该漏洞而受到赞誉。  
  
目前尚无关于谁在利用此漏洞、恶意活动的目标是谁以及这些攻击的范围有多广的相关信息。Oracle 安全保障副总裁 Eric Maurice表示：“如果成功利用该漏洞，未经身份验证的犯罪者可以从目标系统下载可在 PLM 应用程序使用的权限下访问的文件。”  
  
鉴于积极利用，建议用户尽快应用最新补丁以获得最佳保护。  
  
  
参考链接：  
  
https://thehackernews.com/2024/11/oracle-warns-of-agile-plm-vulnerability.html  
  
  
**3.****VMware vCenter Server 堆溢出漏洞(CVE-2024-38812)**  
  
**&VMware vCenter Server 权限提升漏洞(CVE-2024-38813)**  
  
  
11月20日，Broadcom 警告称，攻击者目前正在利用两个 VMware vCenter Server 漏洞，其中一个是严重的远程代码执行漏洞。  
  
TZL 安全研究人员报告了 RCE 漏洞(CVE-2024-38812)。该漏洞是由 vCenter 的 DCE/RPC 协议实现中的堆溢出弱点引起的，影响包含 vCenter 的产品，包括 VMware vSphere 和 VMware Cloud Foundation。  
  
目前另一个存在在野利用的 vCenter Server 漏洞（由同一研究人员报告）是权限提升漏洞，其编号为CVE-2024-38813，攻击者可以利用该漏洞使用特制的网络数据包将权限提升到 root 权限。  
  
Broadcom 表示：“更新公告指出，Broadcom 旗下的 VMware 确认 CVE-2024-38812 和 CVE-2024-38813 已被利用。”  
  
该公司于 9 月发布了安全更新来修复这两个漏洞。然而，大约一个月后，它更新了安全公告，警告称原来的 CVE-2024-38812 补丁并未完全解决该漏洞，并“强烈”鼓励管理员应用新补丁。  
  
这些安全漏洞没有可用的解决方法，因此建议受影响的客户立即应用最新更新版本以阻止主动利用这些漏洞的攻击。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/critical-rce-bug-in-vmware-vcenter-server-now-exploited-in-attacks/  
  
  
**4.****Progress Kemp LoadMaster 操作系统命令注入漏洞(CVE-2024-1212)**  
  
  
11月18日，美国网络安全和基础设施安全局 (CISA) 在其已知利用漏洞 (KEV) 目录中增加了影响 Progress Kemp LoadMaster 的关键操作系统命令注入漏洞。  
  
该漏洞由 Rhino Security Labs 发现，编号为 CVE-2024-1212，已于 2024 年 2 月 21 日发布的更新中得到解决 。然而，这是首次有报告称该漏洞在野外遭到积极利用。  
  
Progress Kemp LoadMaster 包含一个操作系统命令注入漏洞，允许未经身份验证的远程攻击者通过 LoadMaster 管理界面访问系统，从而执行任意系统命令。影响 LoadMaster 7.2.48.1 之前、7.2.54.8 之前和 7.2.55.0 之前的版本。  
  
LoadMaster 是一种应用程序交付控制器 (ADC) 和负载平衡解决方案，大型组织使用它来优化应用程序性能、管理网络流量并确保高服务可用性。  
  
目前尚未发布有关主动利用活动的详细信息，并且其在勒索软件活动中的利用状态被标记为未知。  
  
建议受影响客户尽快升级至安全版本，以避免已知漏洞的攻击。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/cisa-tags-progress-kemp-loadmaster-flaw-as-exploited-in-attacks/  
  
  
**5.****Palo Alto Networks PAN-OS 身份认证绕过漏洞(CVE-2024-0012)**  
  
**&Palo Alto Networks PAN-OS 权限提升漏洞(CVE-2024-9474)**  
  
  
11月18日，Palo Alto Networks 发布了针对其下一代防火墙 (NGFW) 中两个被积极利用的零日漏洞的安全更新。  
  
PAN-OS 设备管理 Web 界面中存在身份认证绕过漏洞（CVE-2024-0012），未经身份验证的远程攻击者可以通过网络访问管理 Web 界面，从而进行后续活动，包括修改设备配置、访问其他管理功能以及利用 Palo Alto Networks PAN-OS 权限提升漏洞(CVE-2024-9474)获取root访问权限。  
  
Palo Alto Networks 最初发现针对有限数量的设备管理 Web 界面的威胁活动。此原始活动于 2024 年 11 月 18 日报告，主要源自已知的用于匿名 VPN 服务的代理/隧道流量的 IP 地址。  
  
Unit 42 正在积极地聚类和描述最初观察到的威胁活动。最初观察到的后利用活动包括交互式命令执行和在防火墙上投放恶意软件（例如 Web Shell）。Unit 42 建议监控并调查在互联网上具有管理网页界面的设备上的任何可疑或其他异常活动，因为确切的入侵后活动和有效载荷可能会有所不同。  
  
Palo Alto Networks 仍在积极调查和补救所有已发现的威胁活动。自 2024 年 11 月 19 日第三方研究人员公开发布技术见解和成果后，Palo Alto Networks 发现威胁活动显著增加。目前，Unit 42 高度确信，链接 CVE-2024-0012 和 CVE-2024-9474 的功能漏洞已公开，这将引发更广泛的威胁活动。  
  
Unit 42 还继续观察手动和自动扫描活动，这与第三方工件广泛使用的时间线保持一致。与第三方报告一致，Unit 42 还观察到入侵后活动的多样性增加，包括开源 C2 工具和加密矿工等其他有效载荷。  
  
Palo Alto Networks 建议客户更新以接收修复 CVE-2024-0012 和 CVE-2024-9474 的最新补丁。  
  
  
参考链接：  
  
https://unit42.paloaltonetworks.com/cve-2024-0012-cve-2024-9474/  
  
**PART****0****3**  
  
  
**安全事件**  
  
  
**1.国家网络安全通报中心：多个与某大国政府有关的境外黑客组织持续攻击国内单位企业**  
  
  
11月21日国家网络安全通报中心消息，中国国家网络与信息安全信息通报中心第二次公告，持续发现一批境外恶意网址和恶意IP，有多个具有某大国政府背景的境外黑客组织，利用这些网址和IP持续对中国和其他国家发起网络攻击。这些恶意网址和IP都与特定木马程序或木马程序控制端密切关联，网络攻击类型包括建立僵尸网络、网络钓鱼、勒索病毒、窃取商业秘密和知识产权、侵犯公民个人信息等，对中国国内联网单位和互联网用户构成重大威胁，部分活动已涉嫌刑事犯罪。相关恶意网址和恶意IP归属地主要涉及：美国、德国、加拿大、新加坡、芬兰、保加利亚等。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/cYBCgbQ2ZI83LZ804BFY3Q  
  
  
**2.因泄露超23.5万患者数据，美国地方医疗机构赔偿超千万元**  
  
  
11月15日GovinfoSecurity消息，美国纽约州一家法院已初步批准一项150万美元（约合人民币1086万元）的和解协议，用于解决针对One Brooklyn Health健康系统的修订后合并拟议集体诉讼。该诉讼源于2022年11月的一次网络攻击事件，该事件导致超过23.5万人的敏感健康数据遭到泄露，泄露数据包括用户身份、支付、诊疗、处方、保险等大量信息。此次事件波及One Brooklyn Health旗下位于纽约市布鲁克林区的三家医院，包括Brookdale医院医疗中心、Interfaith医疗中心和Kingsbrook犹太医疗中心，以及多个护理院和健康诊所。  
  
  
原文链接：  
  
https://www.govinfosecurity.com/one-brooklyn-agrees-to-15m-settlement-in-2022-hack-lawsuit-a-26830  
  
  
**PART****0****4**  
  
  
**政策法规**  
  
  
**1.国家数据局印发《可信数据空间发展行动计划（2024—2028年）**  
  
  
11月23日，国家数据局印发《可信数据空间发展行动计划（2024—2028年）》，提出到2028年，可信数据空间运营、技术、生态、标准、安全等体系取得突破，建成100个以上可信数据空间。该文件要求两类安全保障能力，一是安全防护能力，可信数据空间应针对数据流通的全生命周期，构建必要的防范、检测和阻断等技术手段，防止数据泄露、窃取、篡改等危险行为发生，并建立相关的管理制度和应急处置措施；二是合规监管能力，可信数据空间应监测空间中违反相关法律法规的行为，并应在行为发生时及时采取相应的处置措施。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/DRQvoYdkVMKVtmepri3kyQ  
  
  
**2.国家数据局《国家数据基础设施建设指引》公开征求意见**  
  
  
11月22日，国家数据局组织起草了《国家数据基础设施建设指引（征求意见稿）》，现公开征求意见。该文件提出，国家数据基础设施应全程安全可靠，需要构建标准化、多层次、全方位的安全防护框架，推动安全防护由静态保护向动态保护、由边界安全向内生安全、由封闭环境保护向开放环境保护转变，形成贯穿数据全生命周期各环节的动态安全防护能力，系统保障数据基础设施相关的网络、算力、数据安全。该文件专门设立了“安全防护”章节，重点对国家数据基础设施安全保障、数据流通利用安全提出具体要求。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/UbO85J8EvqS9AT85iGBmdg  
  
  
**3.《网络安全标准实践指南——粤港澳大湾区（内地、香港）个人信息跨境处理保护要求》发布**  
  
  
11月21日，网安标委秘书处联合香港私隐公署编制了《网络安全标准实践指南——粤港澳大湾区（内地、香港）个人信息跨境处理保护要求》，以促进粤港澳大湾区个人信息跨境安全有序流动。该文件规定了粤港澳大湾区（内地、香港）个人信息处理者或者接收方，在大湾区内地和香港间通过安全互认方式进行大湾区内个人信息跨境流动应遵守的基本原则和要求，适用于指导大湾区内个人信息处理者开展个人信息跨境处理活动。  
  
  
原文链接：  
  
https://www.tc260.org.cn/upload/2024-11-21/1732148395263067719.pdf  
  
  
**4.《工业和信息化领域数据安全合规指引》正式发布**  
  
  
11月19日，中国钢铁工业协会、中国有色金属工业协会、中国石油和化学工业联合会等17家行业组织联合发布《工业和信息化领域数据安全合规指引》，引导工业和信息化领域数据处理者规范开展数据处理活动。该文件共9个章节，包括概述、数据分类分级、数据安全管理体系、数据全生命周期保护、数据安全风险监测&预警&报告&处置、数据安全事件应急处置、数据安全风险评估、数据出境安全管理、数据交易。该文件聚焦数据处理者在履行数据安全保护义务过程中的难点问题，明确数据安全合规依据，提供实务指引，指导数据处理者开展数据安全合规管理，以提升数据安全保护能力。  
  
  
原文链接：  
  
https://ccsaweb.eos-beijing-7.cmecloud.cn/ccsa/20241119/93caaf11c87e4530bda1e6d230b5bb8b/93caaf11c87e4530bda1e6d230b5bb8b.pdf  
  
  
**5.美国土安全部发布《关键基础设施中人工智能的角色和职责框架》**  
  
  
11月14日，美国国土安全部发布《关键基础设施中人工智能的角色和职责框架》，为在关键基础设施中安全开发和部署人工智能提供建议。该文件梳理了关键基础设施中人工智能的三类漏洞，包括利用人工智能的攻击、针对人工智能系统的攻击、设计和实施失败。为解决这些漏洞，该文件针对每个关键利益相关者提出了行动建议，包括云和计算设施提供商、人工智能开发商、关键基础设施所有者和运营商、公民组织、公共部门等。该文件将推动建立行业标准，增强透明度和问责制，保护公民权利和自由。  
  
  
原文链接：  
  
https://www.dhs.gov/sites/default/files/2024-11/24_1114_dhs_ai-roles-and-responsibilities-framework-508.pdf  
  
  
**往期精彩推荐**  
  
  
[【已复现】Palo Alto Networks PAN-OS身份认证绕过漏洞(CVE-2024-0012)安全风险通告第二次更新](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502473&idx=1&sn=996d4946cf07c672aed821bfd621c228&chksm=fe79ee11c90e67073edd9159e6dc7a11bb693970335a9bdd2815ae0532da91d5bb7c8a534df9&token=2102648290&lang=zh_CN&scene=21#wechat_redirect)  
[【在野利用】Apple 多个在野高危漏洞安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502473&idx=2&sn=e5d76f1cef13f032535065aae0924b0a&chksm=fe79ee11c90e67077b61dd29c7571b5fa2c8d1a5ac9c4dc5629276ded403c4e0495fdeba1feb&token=2102648290&lang=zh_CN&scene=21#wechat_redirect)  
  
[【在野利用】Palo Alto Networks PAN-OS 身份认证绕过漏洞(CVE-2024-0012)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502450&idx=1&sn=a1fc8665dfbb9c1703fef3c40ff25310&chksm=fe79eeeac90e67fcbf34eede7835db3aa9916a7a50c559be8c34d0446433f0f4d41189eb8b1a&token=2102648290&lang=zh_CN&scene=21#wechat_redirect)  
  
  
  
  
本期周报内容由安全内参&虎符智库&奇安信CERT联合出品！  
  
  
  
  
  
  
  
  
