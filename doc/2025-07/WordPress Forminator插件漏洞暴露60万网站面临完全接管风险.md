#  WordPress Forminator插件漏洞暴露60万网站面临完全接管风险  
原创 NightTeam  夜组OSINT   2025-07-07 08:07  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GLyX5CgG8A1iaTj8eIk1SSvkOGg93dHicZxnUtJGrfvvT0H05mDuuCv0P7M2EmOeNXcIZXiaqcULl1ic0iaAJiavkc3g/640?wx_fmt=png&from=appmsg "")  
  
根据近期网络安全研究披露，广受欢迎的WordPress插件Forminator存在一个严重的安全漏洞，可能导致超过60万个使用该插件的网站面临被完全接管的风险。这一漏洞被追踪为CVE-2025-6463，CVSS评分高达8.8（高危级别），影响Forminator插件1.44.2及以下所有版本。以下是对这一漏洞的详细分析以及应对建议。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GLyX5CgG8A0Lfcbia8qt8p5xJibWAUlcfvDVEDlDMiblcRSjsiaZibkt6AFVnKL2TNWkD9vgBnmjlS55NkLdj4RSHdw/640?wx_fmt=png&from=appmsg "")  
  
## 漏洞详情  
  
Forminator是由WPMU DEV开发的一款功能强大的表单构建插件，支持创建联系表单、支付表单、问卷调查等多种类型的表单内容，采用直观的拖放式设计，深受WordPress用户喜爱。据WordPress.org统计，该插件目前在全球超过60万个网站上处于活跃状态。  
  
此次发现的漏洞源于插件后端代码在处理表单字段输入时缺乏充分的验证和清理，以及不安全的文件删除逻辑。具体来说，Forminator的save_entry_fields()  
函数在保存用户提交的表单数据时，未对输入的文件路径进行严格检查。这意味着攻击者可以通过精心构造的表单提交，将任意文件路径（例如WordPress核心文件wp-config.php  
）嵌入到表单字段中。当管理员手动删除这些表单提交，或插件根据设置自动删除旧提交时，指定的文件也会被删除。  
  
删除wp-config.php  
文件会导致WordPress网站进入“安装模式”，此时攻击者可以通过将其连接到自己控制的数据库来完全接管网站。这种攻击无需身份验证即可实施，极大地增加了漏洞的危险性。  
## 漏洞发现与修复  
  
该漏洞由安全研究员“Phat RiO – BlueRock”于2025年6月20日发现，并通过Wordfence的漏洞赏金计划报告。Wordfence在6月23日联系了WPMU DEV，开发者随即着手修复，并于6月30日发布了Forminator 1.44.3版本。新版本通过以下措施修复了漏洞：  
- 添加了字段类型检查，仅允许删除通过“上传”或“签名”字段上传的文件。  
  
- 实施文件路径验证，确保删除操作仅限于WordPress上传目录内的文件。  
  
- 对文件名进行清理和路径规范化，防止恶意路径注入。  
  
Wordfence为发现该漏洞的研究员颁发了8100美元的赏金，以表彰其对WordPress生态系统安全的贡献。  
## 当前风险与影响  
  
尽管修复版本已于6月30日发布，但根据WordPress.org的数据，在发布后的两天内，Forminator 1.44.3的下载量仅为20万次。这意味着仍有约40万个网站可能运行易受攻击的旧版本，面临被接管的高风险。  
  
目前尚未有关于CVE-2025-6463被主动利用的公开报告。然而，由于漏洞的技术细节已公开，且攻击实施的门槛较低，安全专家警告，恶意攻击者可能很快会利用这一漏洞发起大规模攻击。  
## 应对措施  
  
为了保护网站安全，使用Forminator插件的网站管理员应立即采取以下措施：  
1. **更新插件**  
：将Forminator插件升级到1.44.3或更高版本。这是防止漏洞被利用的最直接方法。  
  
1. **禁用插件**  
：如果无法立即更新，建议暂时禁用Forminator插件，直到能够安装安全版本。  
  
1. **审查表单设置**  
：检查表单提交和删除设置，警惕任何可疑活动。  
  
1. **监控文件系统**  
：定期检查网站文件系统，特别是核心文件如wp-config.php  
是否存在异常更改。  
  
1. **部署安全插件**  
：安装Wordfence等安全插件以增强网站防护。Wordfence已于6月26日为高级用户部署了防火墙规则，免费用户预计将于7月26日获得相同保护。  
  
## 更广泛的WordPress安全建议  
  
此次Forminator漏洞再次凸显了WordPress插件作为攻击目标的脆弱性。WordPress作为一个广泛使用的内容管理系统，其插件和主题往往成为黑客攻击的突破口。为降低安全风险，网站管理员应遵循以下最佳实践：  
- **最小化插件使用**  
：仅安装必要的插件，并定期删除未使用的插件。  
  
- **保持更新**  
：确保WordPress核心、主题和插件始终运行最新版本。  
  
- **选择可信开发者**  
：优先使用来自信誉良好的开发者的插件和主题。  
  
- **增强安全措施**  
：部署防火墙、定期备份网站，并使用强密码和双因素认证。  
  
## 结论  
  
Forminator插件的CVE-2025-6463漏洞为WordPress社区敲响了警钟，提醒网站管理员及时更新和加强安全防护。尽管WPMU DEV已迅速发布修复，但仍有大量网站可能处于风险之中。立即采取行动，更新到最新版本或禁用插件，是保护网站免受攻击的关键步骤。同时，持续关注网络安全动态并实施全面的安全策略，将有助于确保WordPress网站在面对日益复杂的网络威胁时保持安全。  
  
