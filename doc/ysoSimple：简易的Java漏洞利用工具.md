#  ysoSimple：简易的Java漏洞利用工具   
 黑白之道   2025-01-06 01:55  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/3xxicXNlTXLicwgPqvK8QgwnCr09iaSllrsXJLMkThiaHibEntZKkJiaicEd4ibWQxyn3gtAWbyGqtHVb0qqsHFC9jW3oQ/640?wx_fmt=gif "")  
  
  
**01**  
  
**工具介绍**  
  
ysoSimple：  
简易的Java漏洞利用工具，集成Java反序列化，Hessian反序列化，XStream反序列化，SnakeYaml反序列化，Shiro550，JSF反序列化，SSTI模板注入，JdbcAttackPayload，JNDIAttack，字节码生成。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icZ1W9s2Jp2UHQqCdSBoUg01iaiajdXFjLMMAHr8WYR5uic88OKqzh3pd6eBN6URXPIocQib1oofjyUVp8ibkFNlLrpQ/640?wx_fmt=jpeg&from=appmsg&wxfrom=13 "")  
  
  
ysoSimple工具是基于ysoserial-for-woodpecker和JNDIMap二开的Java利用工具，主要是方便自己学习漏洞利用和方便自己整理漏洞利用。  
  
**02**  
  
**漏洞利用思路**  
  
一般像工具的使用通常都是在黑盒情况下，黑盒测试遇到反序列化和JNDI注入情况时：首先我会用像dns/sleep这种poc进行测试来确系漏洞存在，如果漏洞利用方式支持对目标环境进行探测我会先把目标环境的中间件/JDK版本/操作系统这些信息摸清楚方便后续深入利用，然后在进行漏洞利用链攻击时会打下dnslog/sleep方便确认漏洞利用链肯定能使用，最后就是内存马/反弹Shell这些了。  
### (1)Java反序列化  
### 当黑盒测试Java反序列化漏洞时，从环境探测到漏洞利用思路：  
1. URIDNS/FindClassByBomb测试Java反序列化漏洞是否存在  
  
1. FindClassByDNS/FindGadgetByDNS/FindClassByBomb测试目标环境利用链，中间件，操作系统，jdk版本  
  
1. CommonsBeautils/CommonsCollections/C3P0漏洞利用(dnslog测试，sleep延迟，回显测试，内存马，反弹Shell)  
  
1. dirt-data-length/UTF8-Overlong Encoding对抗WAF  
  
### (2)Hessian反序列化  
### 当黑盒测试Hessian反序列化漏洞时，从环境探测到漏洞利用思路：  
1. LazyValue-InetAddress测试Hessian反序列化漏洞是否存在(用Hessian1和Hessian2协议都打下)  
  
1. LazyValue-BCELLoader/LazyValue-Jndi/LazyValue-XSTL漏洞利用(dnslog测试，sleep延迟，回显测试，内存马，反弹Shell)  
  
1. dirt-data-length/UTF8-Overlong Encoding对抗WAF  
  
### (3)XStram反序列化  
  
1. FindClassByBomb测试XStreasm反序列化漏洞是否存在  
  
1. .....  
  
### (4)SnakeYaml反序列化  
  
1. FindClassByDNS测试SnakeYaml反序列化漏洞是否存在  
  
1. FindClassByDNS探测目标环境中间件，操作系统，jdk版本  
  
1. JdbcRowImpl/ScriptEngineManager/C3P0链漏洞利用  
  
### (5)JNDI注入  
### JNDI系列打法分为RMI注入和JNDI注入：  
  
RMI注入  
1. JDK低版本远程工厂类加载(dnslog测试，sleep延迟，回显测试，内存马，反弹Shell)  
  
1. JDK高版本本地工厂类利用(BeanFactory单参数RCE打法，JDBC Factory数据库连接打法...)  
  
1. JRMPListener JRMP层的攻击  
  
JNDI注入  
1. JDK低版本远程工厂类加载(dnslog测试，sleep延迟，回显测试，内存马，反弹Shell)  
  
1. JDK高版本本地工厂类利用(BeanFactory单参数RCE打法，JDBC Factory数据库连接打法...)  
  
1. ldap打Java反序列化(用的多)，这部分就和上面Java反序列化漏洞利用思路一样了  
  
### (6)SSTI模板注入  
### Java系列的模板：Freemarker，Velocity，Pebble。模板注入都是间接操作JavaAPI，通常没有探测环境的步骤，直接测试漏洞利用，利用时同样是先测试(dnslog测试，sleep延迟)保证利用链可以被利用。  
  
FreeMarker  
- 内建函数?new：Execute命令执行，JythonRuntime命令执行，ObjectConstructor实例化对象  
  
- 内建函数?api：调用对象的方法  
  
- StaticModel：静态方法调用  
  
- dataModel：数据模型(FreeMarker<2.3.30绕过沙箱)  
  
- springMacroRequestContext Request上下文RCE  
  
### (7)JdbcAttack数据库利用  
### 在系统管理后台，包括Fastjson/Java反序列化都可能涉及到JDBCAttack。JDBCAttack整理出来有俩种利用方式: 连接串利用，执行SQL语句利用。  
  
H2 DataBase：数据库连接串漏洞利用  
- H2 CreateAlias 执行Java代码  
  
- H2 RunScript 远程加载SQL文件  
  
- H2 StaticMethod 执行Java静态语法  
  
- H2 Groovy 执行Groovy表达式  
  
- H2 JavaScript 执行JS表达式  
  
Derby：执行SQL语句漏洞利用  
- 直接加载java字节码RCE  
  
- 远程加载jar包  
  
- 落地jar包加载  
  
PostgreSQL：数据库连接漏洞利用  
- socketFactory/socketFactoryArg 单参数构造方法实例化  
  
- loggerLevel/loggerFile 日志文件写入  
  
MySQL：数据库连接漏洞利用  
- ReadFile：读文件  
  
- detectCustomCollations：触发Java反序列化，测试步骤和上面一样  
  
- ServerStatusDiffInterceptor：触发Java反序列化，测试步骤和上面一样  
  
  
  
**03**  
  
**工具使用**  
  
目前ysoSimple集成7种漏洞利用模块：  
1. Java原生反序列化：YsoAttack  
  
1. Hessian反序列化：HessianAttack  
  
1. XStram反序列化：XStramAttack  
  
1. SnakeYaml反序列化：SnakeYamlAttack  
  
1. Shiro550反序列化：YsoAttack  
  
1. JSF反序列化：YsoAttack  
  
1. JdbcAttack模块：JdbcAttack  
  
1. SSTI模板注入：SSTIAttack  
  
1. JNDI服务器：JNDIAttack  
  
1. 字节码生成模块：ThirdPartyAttack  
  
YsoSimple用-m或者-mode参数选择要使用的模块，例如：  
```
java -jar ysoSimple.jar -m YsoAttack
```  
```
java -jar ysoSimple.jar -m YsoAttack -g CommonsBeanutils2 -a "Templateslmpl:dnslog:whoami.dnslog.cn"
```  
  
**JNDIAttack模块启动**  
```
java -jar ysoSimple.jar -m JNDIAttack
```  
  
  
**04**  
  
**工具下载**  
  
****https://github.com/B0T1eR/ysoSimple****  
  
> **文章来源：夜组安全**  
  
  
  
黑白之道发布、转载的文章中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用，任何人不得将其用于非法用途及盈利等目的，否则后果自行承担！  
  
如侵权请私聊我们删文  
  
  
**END**  
  
  
