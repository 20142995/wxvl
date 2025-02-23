#  安全热点周报：Fortinet 警告利用身份验证绕过零日漏洞劫持防火墙   
 奇安信 CERT   2025-01-20 08:50  
  
<table><tbody style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><tr bgless="lighten" bglessp="20%" data-bglessp="40%" data-bgless="lighten" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 4px solid rgb(68, 117, 241);visibility: visible;"><th align="center" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;background-color: rgb(254, 254, 254);font-size: 20px;line-height: 1.2;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(68, 117, 241);visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-size: 17px;visibility: visible;">安全资讯导视 </span></strong></span></th></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 六部门印发《关于完善数据流通安全治理 更好促进数据要素市场化价值化的实施方案》</p></td></tr><tr data-bglessp="40%" data-bgless="lighten" data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 拜登发布第二份网络安全行政令，全面加强美国国家网络防御创新</p></td></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• CNCERT发布美网络攻击我国某先进材料设计研究院事件调查报告</p></td></tr></tbody></table>  
  
**PART****0****1**  
  
  
**漏洞情报**  
  
  
**1.FortiOS和FortiProxy身份认证绕过漏洞在野利用通告**  
  
  
1月15日，奇安信CERT监测到官方修复Fortinet FortiOS和FortiProxy身份认证绕过漏洞(CVE-2024-55591)，FortiOS和FortiProxy中存在一个身份认证绕过漏洞。未经身份验证的远程攻击者可以通过向Node.js websocket模块发送特制请求，成功利用此漏洞可使攻击者获得超级管理员权限。目前该漏洞已发现在野利用，鉴于此漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**2.Ivanti Endpoint Manager多个信息泄露漏洞安全风险通告**  
  
  
1月15日，奇安信CERT监测到Ivanti Endpoint Manager信息泄露漏洞(CVE-2024-10811、CVE-2024-13161、CVE-2024-13160、CVE-2024-13159)在互联网上公开。在Ivanti EPM的代理门户中，存在多个绝对路径遍历漏洞。这些漏洞允许远程未经身份验证的攻击者泄露敏感信息。鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**PART****0****2**  
  
  
**新增在野利用**  
  
  
**1.****Aviatrix Controller 操作系统命令注入漏洞(CVE-2024-50603)**  
  
  
1月16日，云安全领域的知名企业 Wiz Research 发现，影响 Aviatrix Controller 云网络平台的严重安全漏洞 CVE-2024-50603 已被威胁行为者广泛利用。该严重漏洞的 CVSS 评分为 10.0，由于某些 API 端点的输入清理不当，该漏洞允许未经身份验证的远程代码执行(RCE)。  
  
CVE-2024-50603 是云网络平台 Aviatrix Controller 中的一个严重漏洞，允许未经身份验证的远程代码执行。命令注入漏洞是由于某些 API 端点的输入清理不当而产生的。也就是说，Aviatrix Controller 的 PHP API（其中包含用户提供的参数）由于处理不当而容易受到攻击，允许未经身份验证的用户执行恶意操作系统命令。   
  
令人担忧的是，攻击者只需编写恶意命令即可利用此漏洞。例如，攻击者无需提供合法的云类型值，而是可以注入以下命令：; rm -rf /（删除系统上的所有文件）或; download_malware.sh（下载并执行恶意软件）。  
  
问题源于这些用户提供的参数如何集成到 Aviatrix Controller 的内部工作中。这些参数没有经过正确验证和清理输入，而是直接合并到系统命令中。利用此漏洞可让攻击者在系统上执行任意命令，从而可能导致云环境中的权限提升。  
  
Wiz Research 发现，该漏洞正在被广泛利用，攻击者在受感染的系统上部署加密货币挖矿程序和后门。Aviatrix Controller 通常在云环境中拥有重要权限，使攻击者能够横向移动并控制关键云资源。鉴于 Aviatrix Controller 的广泛使用及其在云环境中提升权限的潜力，该漏洞可能会危及众多组织的安全。    
  
主动威胁搜寻至关重要，包括检查安全日志中是否存在可疑活动、在托管 Aviatrix Controller 的计算资源上搜索恶意软件发现以及分析云事件中是否存在异常活动。通过采取这些步骤，组织可以减轻与此严重漏洞相关的风险并防止进一步利用。  
  
   
  
参考链接：  
  
https://hackread.com/hackers-cve-2024-50603-aviatrix-controllers-backdoor/  
  
  
**2.********Fortinet FortiOS 和 FortiProxy 身份认证绕过漏洞(CVE-2024-55591)******  
  
  
1月15日，攻击者正在利用 FortiOS 和 FortiProxy 中新的身份验证绕过零日漏洞劫持 Fortinet 防火墙并侵入企业网络。  
  
该漏洞（编号为CVE-2024-55591）影响 FortiOS 7.0.0 至 7.0.16、FortiProxy 7.0.0 至 7.0.19 以及 FortiProxy 7.2.0 至 7.2.12。成功利用此漏洞后，远程攻击者可通过向 Node.js websocket 模块发出恶意请求来获取超级管理员权限。  
  
Fortinet 表示，在野利用零日漏洞的攻击者会在受感染的设备上创建随机生成的管理员或本地用户，并将他们添加到现有的 SSL VPN 用户组或新添加的用户组中。他们还被发现添加或更改防火墙策略和其他设置，并使用之前创建的恶意账户登录 SSLVPN“以建立通往内部网络的隧道”。  
  
虽然该公司没有提供有关此次活动的更多信息，但网络安全公司 Arctic Wolf 发布了一份包含相应入侵指标 (IOC) 的报告，报告称，自 11 月中旬以来，具有暴露在互联网上的管理界面的 Fortinet FortiGate 防火墙一直受到攻击。  
  
Arctic Wolf 实验室表示：“此次活动涉及防火墙管理界面上的未经授权的管理登录、新账户的创建、通过这些账户进行 SSL VPN 身份验证以及其他各种配置更改。”虽然初始访问向量尚未得到明确确认，但零日漏洞的可能性很高。组织应尽快禁用公共接口上的防火墙管理访问。  
  
该网络安全公司补充道：“虽然此次活动中使用的初始访问载体尚未确认，但 Arctic Wolf Labs 高度确信，考虑到受影响组织和受影响固件版本的压缩时间线，零日漏洞很可能被大规模利用。”  
  
目前官方已有可更新版本，建议受影响用户升级至最新版本。Fortinet 还建议管理员禁用 HTTP/HTTPS 管理界面或限制哪些 IP 地址可以通过本地策略访问管理界面作为解决方法。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/fortinet-warns-of-auth-bypass-zero-day-exploited-to-hijack-firewalls/  
  
  
**3.****Windows Hyper-V NT 内核集成 VSP 权限提升漏洞(CVE-2025-21333、CVE-2025-21334、CVE-2025-21335)**  
  
  
1月15日，微软共发布了159个漏洞的补丁程序，修复了Windows 远程桌面服务、Windows Hyper-V、Windows OLE等产品中的漏洞。随着 Windows Hyper-V 平台中出现三个已被利用的漏洞的新消息传出，微软与零日漏洞的斗争已经延伸到 2025 年。该软件巨头呼吁紧急关注 Windows Hyper-V NT 内核集成虚拟化服务提供商 (VSP) 中的三个独立缺陷，并警告称恶意攻击者已经开始发动权限提升漏洞。  
  
CVE-2025-21333、 CVE-2025-21334 和 CVE-2025-21335 是 Windows Hyper-V NT 内核集成虚拟化服务提供程序 (VSP) 中的 EoP 漏洞。这三个漏洞的 CVSSv3 评分均为 7.8，且被评为重要。经过身份验证的本地攻击者可以利用此漏洞将权限提升至 SYSTEM。三个漏洞中有两个尚未确定来源，其中 CVE-2025-21333 被归因于一位匿名研究人员。  
  
微软在其公告中表示：“成功利用此漏洞的攻击者可以获得系统权限。”按照惯例，该公司没有发布技术细节或 IOC（入侵指标）来帮助防御者寻找入侵迹象。被利用的三个零日漏洞（CVE-2025-21334、CVE-2025-21333和CVE-2025-21335）影响 Windows Hyper-V NT 内核集成虚拟化服务提供程序 (VSP)，该提供程序负责处理主机系统与客户虚拟机 (VM) 之间的高效资源管理和通信。   
  
建议管理员和家庭用户尽快测试和部署微软官方发布的补丁，以避免已知漏洞的攻击。  
  
   
  
参考链接：  
  
https://www.securityweek.com/microsoft-patches-trio-of-exploited-windows-hyper-v-zero-days/  
  
  
**4.********BeyondTrust 多款产品命令执行漏洞(CVE-2024-12686)******  
  
  
1月13日，CISA 已将 BeyondTrust 的特权远程访问 (PRA) 和远程支持 (RS) 中的命令注入漏洞 ( CVE-2024-12686 ) 标记为攻击中积极利用的漏洞。BeyondTrust 特权远程访问 (PRA) 和远程支持 (RS) 包含一个操作系统命令注入漏洞，攻击者可以利用该漏洞上传恶意文件。成功利用此漏洞可让远程攻击者在站点用户的上下文中执行底层操作系统命令。  
  
BeyondTrust 在 12 月初调查其部分远程支持 SaaS 实例被入侵时发现了这个漏洞。攻击者窃取了一个 API 密钥，随后使用该密钥重置本地应用程序帐户的密码。尽管 BeyondTrust 在 12 月份的披露文件中没有明确提及这一点，但威胁行为者很可能利用这个漏洞作为零日漏洞侵入 BeyondTrust 系统以接触其客户。  
  
1月初，美国财政部披露，其网络遭到攻击者入侵，攻击者利用窃取的远程支持 SaaS API 密钥入侵了该机构使用的 BeyondTrust 实例。  
  
此后，该攻击与黑客组织Silk Typhoon 有关。该网络间谍组织以侦察和数据窃取攻击而闻名，在 2021 年初利用Microsoft Exchange Server ProxyLogon 零日漏洞入侵了约 68,500 台服务器后广为人知。威胁行为者特别针对了负责管理贸易和经济制裁计划的外国资产控制办公室（OFAC）和负责审查外国投资是否存在国家安全风险的美国外国投资委员会（CFIUS）。他们还入侵了财政部金融研究办公室的系统，但此次事件的影响仍在评估中。据信 Silk Typhoon 使用窃取的 BeyondTrust 数字密钥访问了“与潜在制裁行动和其他文件有关的非机密信息”。  
  
BeyondTrust 表示，它已在所有云实例上应用了针对 CVE-2024-12686 和 CVE-2024-12356 漏洞的安全补丁。但是，运行自托管实例的用户必须手动部署补丁。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/cisa-orders-agencies-to-patch-beyondtrust-bug-exploited-in-attacks/  
  
**PART****0****3**  
  
  
**安全事件**  
  
  
**1.CNCERT发布美网络攻击我国某先进材料设计研究院事件调查报告**  
  
  
1月17日CNCERT公众号消息，国家互联网应急中心CNCERT发布报告，公布了美国对我国某先进材料设计研究院的网络攻击详情，为全球相关国家、单位有效发现和防范美网络攻击行为提供借鉴。报告称，此次攻击流程分为三步，利用电子文件系统漏洞进行攻击入侵、软件升级管理服务器被植入后门和木马程序、大范围个人主机电脑被植入木马。2024年11月6日至16日，攻击者先后三次针对性窃取了该单位重要商业信息、知识产权信息文件共4.98GB。CNCERT还公布了部分打码后的跳板IP 信息。  
  
  
原文链接：  
  
https://www.cert.org.cn/publish/main/49/2025/20250117141608670773438/20250117141608670773438_.html  
  
  
**2.CNCERT发布美网络攻击我国某智慧能源和数字信息大型高科技企业事件调查报告**  
  
  
1月17日CNCERT公众号消息，国家互联网应急中心CNCERT发布报告，公布了美国对我国某智慧能源和数字信息大型高科技企业的网络攻击详情，为全球相关国家、单位有效发现和防范美网络攻击行为提供借鉴。报告称，此次攻击流程分为三步，利用微软Exchange邮件服务器漏洞进行入侵、在邮件服务器植入高度隐蔽的内存木马、对内网30余台重要设备发起攻击。2023年5月至2023年10月，攻击者发起了30余次网络攻击，窃取了大量敏感邮件数据、核心网络设备账号即配置信息、项目管理文件等。  
  
  
原文链接：  
  
https://www.cert.org.cn/publish/main/49/2025/20250117141306569102021/20250117141306569102021_.html  
  
  
**3.微信支付疑存漏洞，用户被异地刷脸支付成功**  
  
  
1月15日正在新闻公众号消息，1月12日晚6点48分，贵州网友张兰（化名）微信收到“到店刷脸支付开通成功”和“微信支付成功”的通知。随后张兰发现，她被陌生人在安徽亳州微信刷脸成功支付106.64元。收款方为“金色华联亳州万达广场店”，支付方式为刷脸支付。收款商家称确有此事，“那个人结账时不仅刷脸成功，还输入对了手机后四位。”张兰看商家监控后发现，刷脸人与自己长相并不相似；同时张兰表示，手机号是在学校办理，没有更换过，也没有开启过微信到店刷脸支付功能。张兰向微信支付提交被盗申赔，也在亳州当地报警。1月14日，20点53分微信支付官方将被刷钱款退还用户。1月15日，腾讯方面表示，用户付款流程通过手机号、人脸识别双重核验，具体情况正在沟通核实；为避免用户损失，已先行全额补偿。微信刷脸技术会根据用户风险评级，加强支付时身份认证，保障用户资金安全。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/MYv92nhir0hHjnMyyIjTHA  
  
  
**4.Fortinet防火墙近期频遭攻击，因零日漏洞被利用**  
  
  
1月15日DarkReading消息，安全厂商Arctic Wolf的研究人员发现，近期一系列针对Fortinet FortiGate防火墙设备的攻击均利用了一个零日漏洞CVE-2024-55591。据分析，这些设备的管理接口暴露在互联网上，攻击者利用这些接口实施了未经授权的管理员登录、修改配置、创建新账号，并执行了SSL VPN认证操作等。此次攻击活动分为四个阶段，2024年11月中旬执行漏洞扫描、11月底实施侦察活动，12月初修改SSL VPN配置，12月中旬至下旬横向移动。Frotinet公司在2025年1月15日首次披露CVE-2024-55591并称已遭在野利用，Arctic Wolf研究员随后称此次事件利用的就是该漏洞。  
  
  
原文链接：  
  
https://www.darkreading.com/threat-intelligence/zero-day-security-bug-fortinet-firewall-attacks  
  
  
**5.攻击者绕过微软OpenAI云安全护栏，对外售卖违规内容生成服务**  
  
  
1月10日CyberScoop消息，在近期提交给美国弗吉尼亚东区法院的文件中，微软起诉了10名个人，指控他们使用被盗凭证和定制软件入侵运行微软Azure OpenAI服务的计算机，生成“有害内容”，并申请关闭外国网络犯罪分子用于绕过生成式AI系统安全指南的互联网基础设施。据悉，2024年7月至8月，攻击者利用被盗的API密钥，访问微软Azure OpenAI服务中的设备和账号，使用软件工具绕过安全护栏，生成了“数千张”违反内容限制的图片，并对外出售这些访问权限。微软称，被告人至少有3名是位于外国的服务提供者，其他人可能是服务使用者。  
  
  
原文链接：  
  
https://cyberscoop.com/microsoft-generative-ai-lawsuit-hacking/  
  
  
**PART****0****4**  
  
  
**政策法规**  
  
  
**1.拜登发布第二份网络安全行政令，全面加强美国国家网络防御创新**  
  
  
1月16日，美国总统拜登正式签发其任内第二份网络安全行政令，承诺在2021年首份网络安全行政令的基础上采取更多行动来改善美国的网络安全，旨在解决联邦系统、关键基础设施和私营部门的脆弱性，追捕和制裁破坏美国互联网和电信系统的外国对手或黑客组织。该文件强调，为应对敌对国家和犯罪分子继续针对美国和美国民众的网络攻击，必须采取更多措施来提高国家网络安全。该文件包括八项重点内容：一是实现第三方软件供应链的透明度和安全性；二是提高联邦系统的网络安全；三是保护联邦通信；四是打击网络犯罪和欺诈；五是利用人工智能促进网络安全；六是确保政策与实践相结合；七是保护国家安全系统和破坏性影响系统；八是采取额外措施打击重大恶意网络活动。  
  
  
原文链接：  
  
https://www.whitehouse.gov/briefing-room/presidential-actions/2025/01/16/executive-order-on-strengthening-and-promoting-innovation-in-the-nations-cybersecurity/  
  
  
**2.六部门印发《关于完善数据流通安全治理 更好促进数据要素市场化价值化的实施方案》**  
  
  
1月15日，国家发展改革委、国家数据局、中央网信办、工业和信息化部、公安部、市场监管总局印发了《关于完善数据流通安全治理 更好促进数据要素市场化价值化的实施方案》。该文件提出了7项主要任务，包括明晰企业数据流通安全规则、加强公共数据流通安全管理、强化个人数据流通保障、完善数据流通安全责任界定机制、加强数据流通安全技术应用 、丰富数据流通安全服务供给、防范数据滥用风险。  
  
  
原文链接：  
  
https://www.ndrc.gov.cn/xxgk/zcfb/tz/202501/P020250115397542070887.pdf  
  
  
**3.美国商务部发布最终规则，限制中国网联汽车技术进入美国市场**  
  
  
1月14日，美国商务部工业与安全局（BIS）发布了题为《保护信息和通信技术与服务供应链：网联汽车》的最终规则，禁止向美国进口和在美国销售特定的与中国有关的车辆连接系统（VCS）硬件与包含VCS或自动驾驶软件的网联汽车整车，狐妖针对乘用车市场，适用于总重量不超过10000磅（约4536公斤）的道路机动车辆。BIS解释称，出台该规则的主要原因是应对智能网联汽车供应链安全风险。这是2024年2月29日和2024年9月23日BIS分别发布该规定的拟议规则预通知和拟议规则通知的最终规则，于2025年3月17日生效。  
  
  
原文链接：  
  
https://www.bis.gov/press-release/commerce-finalizes-rule-secure-connected-vehicle-supply-chains-foreign-adversary  
  
  
**4.工信部办公厅印发《关于加强互联网数据中心客户数据安全保护的通知》**  
  
  
1月14日，工业和信息化部办公厅印发《关于加强互联网数据中心客户数据安全保护的通知》。该文件共5章16条，包括基础要求、加强服务器托管业务场景保障能力、加强数据存储与计算业务场景保障能力、加强数据安全供给支撑、工作实施。该文件要求，按照“权责一致、分类施策、技管结合、确保安全”的原则，加强客户数据安全保障能力建设，提升客户数据安全保护水平。该文件还针对设备供应链管理、算力等重点服务安全管理等方面提出了能力要求。  
  
  
原文链接：  
  
https://www.miit.gov.cn/zwgk/zcwj/wjfb/tz/art/2025/art_1bbf7c744c994183abb3ad6148658960.html  
  
  
**5.四部门印发《关于促进数据标注产业高质量发展的实施意见》**  
  
  
1月13日，国家发展改革委、国家数据局、财政部、人力资源社会保障部印发了《关于促进数据标注产业高质量发展的实施意见》。该文件共6章13项任务，包括总体要求、深化需求牵引、增强创新驱动、培育繁荣生态、优化支撑体系、加强保障措施。该文件专门设立了“促进标注产业安全发展”任务，包括建立健全数据标注安全性风险识别、监测预警、应急响应等相关规范，落实数据标注全过程相关主体的安全责任。合理保护数据标注企业在数据流通过程中形成的相关权益。加强数据标注隐私保护、人工智能对齐、安全评估能力建设。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/Vdij4H1SItWC7otwWPm6iQ  
  
  
**往期精彩推荐**  
  
  
[奇安信集团2025年01月补丁库更新通告-第一次更新](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502851&idx=1&sn=7f2bbf94f6a150066e8b83295b2969dc&token=2011863198&lang=zh_CN&scene=21#wechat_redirect)  
[Ivanti Endpoint Manager 多个信息泄露漏洞安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502786&idx=1&sn=9c3ec9f73e5c305b28c6848870adc528&token=2011863198&lang=zh_CN&scene=21#wechat_redirect)  
  
[【已发现在野利用】FortiOS 和 FortiProxy 身份认证绕过漏洞(CVE-2024-55591)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502778&idx=1&sn=7d1359542670d9b5a1c7d2543cf0345a&token=2011863198&lang=zh_CN&scene=21#wechat_redirect)  
  
  
  
  
本期周报内容由安全内参&虎符智库&奇安信CERT联合出品！  
  
  
  
  
  
  
  
  
  
  
