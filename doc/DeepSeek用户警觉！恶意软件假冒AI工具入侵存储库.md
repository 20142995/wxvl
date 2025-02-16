#  DeepSeek用户警觉！恶意软件假冒AI工具入侵存储库   
原创 HackerNews  安全威胁纵横   2025-02-06 08:11  
  
恶意软件假借DeepSeek AI工具在Python存储库中传播，窃取用户数据，开发者需提高警惕。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Ok8FsaZqg4zxf4tXPIwEDFgvqxaoGr4yyLaQgWxBZ31ecYYYibxSd3AoruQA7TRyJV35hlxkWTUAp1avT3pe1XA/640?wx_fmt=jpeg "")  
  
  
恶意软件包正悄然侵袭 Python 存储库，其目标直指那些有意向将   
DeepSeek  
 融入自身工作流程的开发人员与工程师。  
  
近期  
，DeepSeek 在人工智能领域掀起了一阵轩然大波，促使科技巨头们加速推进自家 AI 模型的发展进程，普通 AI 爱好者与工程师也进行了尝试。在这股全民热捧的浪潮之中，不法分子妄图趁虚而入，利用那些渴望将 DeepSeek 应用到自身业务中的人员。  
  
Positive Technologies 的研究人员察觉并成功阻止了一起  
针对 Python 包索引（PyPI）存储库  
的恶意活动。  
  
此次攻击的主要对象是开发人员、机器学习工程师以及 AI 爱好者，他们本打算借助 DeepSeek 的 AI 技术来简化项目操作流程。  
  
经研究人员调查发现，此次攻击的发起者名为  
 “bvk”  
，该账户创建于 2023 年 6 月。值得注意的是，bvk 在此之前从未向 PyPI 存储库上传或贡献过任何内容。  
  
然而，这一情况在 2025 年 1 月发生了转变，该用户上传了两个分别名为   
“deepseeek” 和 “deepseekai”   
的软件包。  
  
“Deepseekai” 项目被宣传为 “DeepSeek AI API 的 Python 客户端 —— 可访问大型语言模型和 AI 服务”，这将使得开发人员的代码能够与 DeepSeek 的服务实现交互。  
  
而 “deepseeek” 项目则号称是一个 “DeepSeek API 客户端”，允许开发人员在其 Python 项目中使用并部署 DeepSeek 的服务。  
  
研究人员经核实确认，这些软件包  
均为非法软件  
，且发现它们旨在窃取用户和计算机数据以及环境变量。  
  
环境变量通常包含运行应用程序所必需的敏感信息，例如用于安全防护的 API 密钥、数据库连接详细信息以及系统路径等。  
  
毫不知情的用户若在命令行中运行 “deepseeek” 或 “deepseekai” 命令，可能并不会意识到，他们已经下载了正在后台悄然运行的恶意代码。  
  
此恶意负载会暗中执行有害代码，在此情境下，它会从毫无防备的开发人员那里窃取数据。  
  
研究人员发现的一个恶意代码的显著特点是，其作者利用 AI 来辅助编写脚本。  
  
研究人员在察觉到这一情况后，迅速通知了管理员，相关软件包随即被删除。尽管如此，在被删除之前，这些软件包的  
下载量仍超过了 220 次  
。  
  
近期，在 JavaScript（npm）和 Python（PyPI）等开源存储库中，发现的恶意软件包数量呈现出激增的态势。  
  
研究人员对 700 多万个开源项目进行了分析，结果显示其中竟有  
 7%  
 的软件包包含恶意软件。  
> Sonatype 警告称：“仅在过去一年，就记录了超过 512,847 个恶意软件包，与上一年同期相比，增长幅度高达 156%，这一数据凸显出组织迫切需要调整其使用习惯。”  
  
  
在现代软件中，高达 90% 的部分都依赖于开源组件。仅今年一年，此类软件包的下载量就已突破了 6.6 万亿次。然而，在 700 多万个可用软件包中，只有 10.5% 的开源代码在开发过程中得到了积极使用。  
  
  
转载请注明出处@安全威胁纵横，封面来自网络；  
  
本文  
来源：https://cybernews.com/security/malicious-python-packages-deepseek/  
  
  
  
  
