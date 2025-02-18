#  安全热点周报：新的 Cleo 零日 RCE 漏洞被利用于数据盗窃攻击   
 奇安信 CERT   2024-12-16 09:20  
  
<table><tbody style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><tr bgless="lighten" bglessp="20%" data-bglessp="40%" data-bgless="lighten" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 4px solid rgb(68, 117, 241);visibility: visible;"><th align="center" style="-webkit-tap-highlight-color: transparent;padding: 5px 10px;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;background-color: rgb(254, 254, 254);font-size: 20px;line-height: 1.2;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(68, 117, 241);visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-size: 17px;visibility: visible;">安全资讯导视 </span></strong></span></th></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;padding: 5px 10px;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 国家发改委公布《电力监控系统安全防护规定》修订版</p></td></tr><tr data-bglessp="40%" data-bgless="lighten" data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;padding: 5px 10px;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 美商务部发布ICTS最终规则，确保信息与通信技术和服务供应链安全</p></td></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;padding: 5px 10px;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 国内最大IT社区CSDN被挂马，CDN可能是罪魁祸首</p></td></tr></tbody></table>  
  
**PART****0****1**  
  
  
**漏洞情报**  
  
  
**1.Apache Struts文件上传漏洞安全风险通告**  
  
  
12月12日，奇安信CERT监测到官方修复Apache Struts文件上传漏洞(CVE-2024-53677)，Apache Struts的文件上传逻辑中存在漏洞，若代码中使用了FileUploadInterceptor，当进行文件上传时，攻击者可能构造恶意请求利用目录遍历等上传文件至其他目录。如果成功利用，攻击者可能能够执行远程代码、获取敏感数据、破坏网站内容或进行其他恶意活动。鉴于此漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**2.OpenWrt Attended SysUpgrade命令注入漏洞安全风险通告**  
  
  
12月10日，奇安信CERT监测到官方修复OpenWrt Attended SysUpgrade命令注入漏洞(CVE-2024-54143)，OpenWrt的Attended SysUpgrade（ASU）服务中存在命令注入漏洞和弱哈希漏洞。该命令注入漏洞源于Imagebuilder在构建过程中未能正确处理用户提供的软件包名称，导致攻击者可以将任意命令注入构建过程，生成恶意固件镜像。同时，由于SHA-256哈希被截断至12个字符，降低了哈希的复杂性，增加了哈希碰撞的可能性，使得攻击者可以利用哈希碰撞技术用恶意镜像覆盖合法缓存镜像，从而影响已交付版本的完整性。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为184058个，关联IP总数为63299个。目前该漏洞技术细节与PoC已在互联网上公开，鉴于此漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**PART****0****2**  
  
  
**新增在野利用**  
  
  
**1.****Cleo 多产品无限制文件上传漏洞(CVE-2024-50623)**  
  
  
12月13日，黑客正在积极利用 Cleo 管理文件传输软件中的零日漏洞来侵入公司网络并进行数据盗窃攻击。  
  
该漏洞存在于该公司的安全文件传输产品 Cleo LexiCom、VLTrader 和 Harmony 中，该漏洞允许不受限制的文件上传和下载，从而导致远程代码执行。Cleo MFT 漏洞影响 5.8.0.21 及更早版本，是之前修复的漏洞 CVE-2024-50623 的绕过方法，Cleo于 2024 年 10 月解决了该漏洞。然而，修复并不完整，威胁行为者得以绕过该漏洞并继续利用它进行攻击。  
  
Huntress 安全研究人员首次发现了Cleo MFT 软件的主动攻击，他们还在一篇新文章中发布了概念验证 (PoC) 漏洞，警告用户采取紧急行动。Huntress 解释道：“该漏洞正在被广泛利用，运行 5.8.0.21 的完全修补系统仍然可以被利用。”有证据表明， CVE-2024-50623被积极利用始于2024年12月3日，12月8日观察到的攻击量显著上升。虽然攻击原因尚不清楚，但这些攻击与美国、加拿大、荷兰、立陶宛和摩尔多瓦的以下 IP 地址有关。  
  
攻击利用 Cleo 漏洞将名为“healthchecktemplate.txt”或“healthcheck.txt”的文件写入目标端点的“autorun”目录，这些文件由 Cleo 软件自动处理。当发生这种情况时，文件会调用内置的导入功能来加载其他有效负载，例如包含 XML 配置（“main.xml”）的 ZIP 文件，其中包含将要执行的 PowerShell 命令。PowerShell 命令与远程 IP 地址建立回调连接、下载额外的 JAR 负载并擦除恶意文件以阻碍法医调查。Huntress 表示，在后利用阶段，攻击者使用“nltest.exe”枚举 Active Directory 域，部署 webshell 以在受感染系统上进行持久远程访问，并使用 TCP 通道最终窃取数据。当完成对系统的利用后，威胁行为者会执行 PowerShell 命令来删除攻击中的文件，例如“C:\LexiCom\cleo.1142”。  
  
Huntress 的遥测表明，这些攻击影响了至少十个使用 Cleo 软件产品的组织，其中一些组织从事消费品、食品行业、卡车运输和航运业务。Huntress 指出，在其可见范围之外还有更多的潜在受害者，Shodan 互联网扫描返回了 390 个 Cleo 软件产品的结果，绝大多数（298 个）易受攻击的服务器位于美国。  
  
鉴于 CVE-2024-50623 的活跃利用以及当前补丁（版本 5.8.0.21）的无效性，用户必须立即采取措施降低受到攻击的风险。建议使用防火墙，并限制对 Cleo 系统的外部访问。  
  
   
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/new-cleo-zero-day-rce-flaw-exploited-in-data-theft-attacks/  
  
  
**2.********Windows 通用日志文件系统驱动程序权限提升漏洞(CVE-2024-49138)******  
  
  
12月11日，微软共发布了72个漏洞的补丁程序，修复了Windows 通用日志文件系统、Windows 轻量级目录访问协议 (LDAP)、Windows 远程桌面服务等产品中的漏洞，微软呼吁紧急关注 Windows 通用日志文件系统 (CLFS) 中已被利用的零日漏洞。  
  
反恶意软件供应商 CrowdStrike 报告了 CLFS 漏洞，该漏洞被标记为 CVE-2024-49138，CVE-2024-49138 是 Windows 通用日志文件系统 (CLFS) 驱动程序中的一个 EoP 漏洞，并被标记为正在被积极利用。该漏洞的 CVSS 严重性评分为 7.8/10。  
  
CLFS 驱动程序漏洞允许攻击者通过基于堆的缓冲区溢出获得系统权限。微软警告称，成功利用该漏洞无需用户交互，且执行权限较低。按照惯例，该公司没有发布入侵指标 (IOC) 或任何其他遥测数据来帮助防御者寻找入侵迹象。  
  
在过去五年中，CLFS（用于数据和事件记录的 Windows 子系统）中至少有 25 个已记录的漏洞。今年早些时候，微软表示正在试验一项重要的新安全缓解措施，以阻止针对 Windows CLFS 漏洞的网络攻击激增。  
  
建议管理员和家庭用户尽快测试和部署微软官方发布的补丁，以避免已知漏洞的攻击。  
  
  
参考链接：  
  
https://www.securityweek.com/microsoft-ships-urgent-patch-for-exploited-windows-clfs-zero-day/  
  
**PART****0****3**  
  
  
**安全事件**  
  
  
**1.国内最大IT社区CSDN被挂马，CDN可能是罪魁祸首**  
  
  
12月12日奇安信威胁情报中心公众号消息，奇安信威胁情报中心研究员近日观察到，某恶意域名的访问量从9月初陡增，10月底开始爆发，并观察到恶意的Payload，基于相关日志确认CSDN被挂马。奇安信全球鹰测绘数据显示，国内大量网站正文页面中包含该恶意域名，包含政府、互联网、媒体等网站，所涉及的域名均挂有CDN，对应IP也都为CDN节点，由于该研究缺乏大网数据，只能推测CDN厂商疑似被污染。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/qQw1DXE25Gkz_P8pEPVaHg  
  
  
**2.国家安全部：境外间谍机关利用众包模式对我开展窃密活动**  
  
  
12月4日国家安全部公众号消息，国家安全部发文称，近年来国家安全机关工作发现，境外间谍情报机关利用众包模式对我开展窃密活动，手法尤为隐蔽，需引起警惕。个别境外间谍情报机关借此大肆搜集我海洋水文、矿产分布、能源储备、高精度地理信息等敏感数据，对我国家安全造成危害。国家安全部指出两类典型行为易涉及“众包窃密”，一是境外间谍情报机关可能假借软件开发为由，在“众包”平台发布信息数据征集任务，要求参与者安装其开发的专业地理测绘软件，并到指定点位上传数据即可获取相应物质奖励。其指定点位往往涉及我敏感涉密场所，众人多角度数据上传将对我敏感点位信息安全造成威胁。二是境外间谍情报机关可能利用众包模式，向参与者提供相关物联网设备，要求参与者自行架设，企图运用无线通讯和区块链技术搭建点对点的无线网络，让所有参与者成为网络的运营者之一，这样参与者本人及其所收集的信息数据均上传至该网络。因其网络覆盖面较大、匿名性较强、物理真实性较好，可能构建去中心化情报信息收集网络。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/rt6QPaB6ES21Ro31BnfwyQ  
  
  
**PART****0****4**  
  
  
**政策法规**  
  
  
**1.四部门发布《中小企业数字化赋能专项行动方案（2025-2027年）》**  
  
  
12月13日，工业和信息化部、财政部、中国人民银行、金融监管总局发布《中小企业数字化赋能专项行动方案（2025-2027年）》，旨在由点及面、由表及里、体系化推进中小企业数字化转型。该文件部署了7类18个重点任务，其中包括全面增强中小企业数据与网络安全防护能力。该重点任务要求引导中小企业建立健全网络和数据安全管理制度，促进态势感知、工业防火墙、入侵检测系统等安全产品部署应用。支持中小企业开展网络和数据安全演练，提升中小企业网络风险防御和处置能力。鼓励中小企业通过购买网络安全保险等方式降低安全风险。  
  
  
原文链接：  
  
https://www.miit.gov.cn/zwgk/zcwj/wjfb/tz/art/2024/art_b286a153d2ff4494a6d8956964499d24.html  
  
  
**2.国家发改委公布《电力监控系统安全防护规定》修订版**  
  
  
12月11日，国家发展和改革委员会修订出台了《电力监控系统安全防护规定》。该文件共6章37条，包括总则、安全技术、安全管理、应急措施、监督管理、附则。此次修订主要做了多方面调整完善，一是强化安全接入区防护要求，明确安全接入区加密认证、安全监测等技术要求；二是强化技术防护措施，在坚持十六字原则的基础上，补充安全免疫、态势感知、动态评估和备用应急措施；三是定义电力监控专用网络，明确承载电力监视和控制业务的专用广域数据网络、专用局域网络以及专用通信线路属于电力监控专用网络范畴，四是强化供应链及电力监控系统专用安全产品管理，明确运营者应当以合同条款的方式对电力监控系统供应商提出安全要求，明确由国家电力调度控制中心牵头组建电力监控系统专用安全产品管理委员会。  
  
  
原文链接：  
  
https://www.ndrc.gov.cn/xxgk/zcfb/fzggwl/202412/P020241211585903621428.pdf  
  
  
**3.教育部、国家版权局发布《关于做好教育系统软件正版化工作的通知》**  
  
  
12月11日，教育部、国家版权局发布《关于做好教育系统软件正版化工作的通知》，旨在进一步完善教育系统软件正版化工作长效机制，推进教育系统软件正版化工作规范化、常态化、制度化。该文件要求强化软件采购源头管理，新购置的办公终端应预装或配套采购正版操作系统软件、办公软件和杀毒软件；要将软件正版化工作与教育数字化、网络安全、信息技术应用创新等工作相衔接，整体设计、一体推进，建立健全本单位软件正版化工作管理制度，明确责任分工、工作要求和工作程序，压实工作责任；要将软件正版化纳入教育数字化、网络安全等相关工作宣传内容，增强教师、学生使用正版软件的意识和习惯。  
  
  
原文链接：  
  
http://www.moe.gov.cn/srcsite/A16/s3342/202412/t20241211_1166570.html  
  
  
**4.广电总局发布《管理提示（AI魔改）》**  
  
  
12月7日，广电总局网络视听司发布《管理提示（AI魔改）》。该文件指出，近期，AI“魔改”视频以假乱真、“魔改”经典现象频发，如《甄嬛传》变身“枪战片”，《红楼梦》改成“武打戏”，孙悟空骑着摩托车扬长而去等。该文件认为，这些视频为博流量，毫无边界亵渎经典IP，冲击传统文化认知，与原著精神内核相悖，且涉嫌构成侵权行为。为营造清朗网络视听空间，该文件提出具体管理要求，一是各相关省局督促辖区内短视频平台排查清理AI“魔改”影视剧的短视频，并于12月10日反馈工作情况。二是严格落实生成式人工智能内容审核要求，举一反三，对各自平台开发的大模型或AI特效功能等进行自查，对在平台上使用、传播的各类相关技术产品严格准入和监看，对AI生成内容做出显著提示。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/PimAyOBFbDRuuSLViyRhUw  
  
  
**5.美商务部发布ICTS最终规则，确保信息与通信技术和服务供应链安全**  
  
  
12月5日，美国商务部工业和安全局发布关于《保障信息与通信技术和服务的供应链安全》的最终规则。该规则旨在加强美国对信息与通信技术和服务（ICTS）供应链的监管，明确调查所谓外国对手对美国信息与通信技术和服务交易造成威胁时须遵循的审查程序，防范所谓外国对手通过ICTS产品或服务威胁美国国家安全、经济利益和关键基础设施。本次最终规则是为具体落实美国《国际紧急经济权力法》和《国家紧急法》下授权签发的第13873号行政令而发布，在原有出口管制的复杂制度体系外，新增进口管制、供应链管理交叉领域的行政法律程序和措施。  
  
  
原文链接：  
  
https://www.bis.gov/press-release/commerce-issues-final-rule-formalize-icts-program  
  
  
**往期精彩推荐**  
  
  
[Apache Struts 文件上传漏洞(CVE-2024-53677)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502622&idx=1&sn=b09b74ae58ce913511ebc0fee0ec7fef&token=1202774588&lang=zh_CN&scene=21#wechat_redirect)  
[微软12月补丁日多个产品安全漏洞风险通告：1个在野利用、17个紧急漏洞](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502600&idx=1&sn=2b1a45b44e8988e6121af578a0aece0a&token=1202774588&lang=zh_CN&scene=21#wechat_redirect)  
  
[OpenWrt Attended SysUpgrade 命令注入漏洞(CVE-2024-54143)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502599&idx=1&sn=08433e68d77f02c833f22ef0e429a3a4&token=1202774588&lang=zh_CN&scene=21#wechat_redirect)  
  
  
  
  
本期周报内容由安全内参&虎符智库&奇安信CERT联合出品！  
  
  
  
  
  
  
  
  
