#  Veeam修复了关键的服务提供商控制台 (VSPC) 漏洞   
鹏鹏同学  黑猫安全   2024-12-06 03:47  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/8dBEfDPEce8aAo0n0NicPPA8cDBHQyJibE3x78oWBTMNcQ3S1RsOHS0HXzDcpxbNsicmIPjowfo64b81MyPfyribAA/640?wx_fmt=png&from=appmsg "")  
  
Veeam发布了针对服务提供商控制台 (VSPC) 关键漏洞的安全更新，该漏洞追踪编号为CVE-2024-42448（CVSS评分为9.9）。成功利用此漏洞可能导致在易受攻击的安装上执行远程代码。  
  
Veeam服务提供商控制台 (VSPC) 是一款为提供备份、灾难恢复和云服务的服务提供商设计的管理和监控解决方案。它支持跨多个租户对Veeam驱动解决方案进行集中管理，并提供计费、报告和自动化部署工具。  
  
该漏洞影响Veeam服务提供商控制台8.1.0.21377和所有更早版本的8和7版本。“从VSPC管理代理机器上，如果管理代理在服务器上获得授权，则有可能在VSPC服务器机器上执行远程代码执行 (RCE)。”安全公告中写道。该公司确认其专家在内部测试中发现了此漏洞。  
  
Veeam还修复了一个漏洞，追踪编号为CVE-2024-42449（CVSS评分为7.1），该漏洞可能被利用来泄露VSPC服务器服务帐户的NTLM哈希值并在VSPC服务器机器上删除文件。“从VSPC管理代理机器上，如果管理代理在服务器上获得授权，则有可能泄露VSPC服务器服务帐户的NTLM哈希值并在VSPC服务器机器上删除文件。”安全公告中写道。  
  
这两个漏洞已在8.1.0.21999版本中得到修复。建议组织升级到最新版本的软件。过去，威胁参与者曾利用Veeam漏洞进行勒索软件攻击。11月，研究人员报告称，Veeam备份与复制 (VBR) 中的关键漏洞CVE-2024-40711被用于部署Frag勒索软件。在Akira和Fog勒索软件攻击之后，专家警告说，威胁参与者正在积极利用CVE-2024-40711部署Frag勒索软件。10月中旬，Sophos研究人员警告说，勒索软件运营商正在利用CVE-2024-40711漏洞创建恶意帐户并部署恶意软件。  
  
