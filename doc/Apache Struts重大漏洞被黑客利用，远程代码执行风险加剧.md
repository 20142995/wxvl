#  Apache Struts重大漏洞被黑客利用，远程代码执行风险加剧   
安世加  安世加   2024-12-19 10:36  
  
12月19日 网络安全研究人员最近发现，黑客正利用Apache Struts 2（一种流行的Java Web应用开发框架）中的一个重大漏洞进行网络攻击，**该漏洞的追踪编号为CVE-2024-53677**，能使网络攻击者绕过网络安全措施，从而完全控制受影响的服务器。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/UZ1NGUYLEFgcA7bggpA95iaqhjhPnOKQjQhNwphC9fUibk7wh7njicBlcADpztDFjMJsamftpYDHruaMvfpicho6fA/640?wx_fmt=png&from=appmsg "")  
  
  
**用户面临远程代码执行风险**  
  
Apache Struts是一个开源框架，广泛支持政府部门、金融机构、电子商务平台以及航空公司等众多关键业务领域的运营。  
  
  
根据通用漏洞评分系统（CVSS）4.0的评估，该漏洞的严重级别评分高达9.5分。这一漏洞的核心问题在于其文件上传机制存在缺陷，允许网络攻击者遍历路径并上传恶意文件。这可能导致远程代码执行（RCE），使网络攻击者能够窃取敏感数据、部署更多的有效载荷或远程执行恶意命令。  
  
  
该漏洞影响了多个版本的Apache Struts，其中包括早已停止支持和维护的Struts 2.0.0至2.3.37系列以及2.5.0至2.5.33版本。此外，最近推出的Struts 6.0.0至6.3.0.2版本也存在这个问题。所有这些受影响的版本均面临严重的安全威胁，极易遭受远程代码执行（RCE）攻击。  
  
  
ISC SANS研究人员Johannes Ullrich在发布的报告中指出，已经监测到利用PoC漏洞利用代码进行的攻击行为。网络攻击者通过上传名为“exploit.jsp”的文件积极扫描易受攻击的系统，并试图通过页面上显示的“Apache Struts”字样来验证其攻击是否成功。  
  
  
Ullrich指出，迄今为止，所有观测到的攻击活动均源自单一的IP地址169.150.226.162。他警告称，随着这个漏洞的公众认知度不断提升，安全形势可能会恶化。此次攻击模式与之前的CVE-2023-50164漏洞相似，这引发了人们的猜测——最新的漏洞可能源于之前修复工作的不彻底，而这一问题对Struts项目来说是一个长期面临的挑战。  
  
  
**需要立即采取行动应对**  
  
开源软件基金会Apache的一位发言人提出，为了有效应对该漏洞，建议用户将Apache Struts升级到Struts 6.4.0或更高版本。  
  
  
然而，单纯的软件升级并不足以全面保障安全。组织还需完成向Action文件上传机制的迁移工作，因为遗留的文件上传逻辑会使系统容易受到攻击。这一迁移涉及重写文件上传操作以适应新机制，而新机制并不支持向后兼容。他说，“这一更改不向后兼容，因为用户必须重写操作以启用用新的Action文件上传机制和相关拦截器，而继续使用原有的文件上传机制将让他们更容易受到这种攻击。”  
  
  
据了解，**目前包括加拿大、澳大利亚和比利时在内的**多个国家的网络安全机构已经公开发布警告，敦促各组织迅速采取行动。如果没有及时采取补救措施，那些易受攻击的系统将面临重大风险。  
  
  
毋庸讳言，此次出现的漏洞再次凸显了与过时和未打补丁的软件相关的持续风险。**Apache Struts框架过去一直是黑客重点关注的攻击目标**，其中包括2017年发生的Equifax数据泄露事件，该事件导致近1.5亿人的个人信息外泄，造成了恶劣的影响。  
  
  
本公众号发布的文章均转载自互联网或经作者投稿授权的原创，文末已注明出处，其内容和图片版权归原网站或作者本人所有，并不代表安世加的观点，若有无意侵权或转载不当之处请联系我们处理！  
  
文章来源：极客网  
  
  
  
安世加为出海企业提供SOC 2、ISO27001、PCI DSS、TrustE认证咨询服务（点击图片可详细查看）  
  
[](https://mp.weixin.qq.com/s?__biz=MzU2MTQwMzMxNA==&mid=2247540448&idx=1&sn=165f2bc3b3233827b2c601a32073aca8&scene=21#wechat_redirect)  
  
  
