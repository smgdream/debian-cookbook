# 安全漏洞追踪

软硬件产品中通常都会存在安全漏洞，安全漏洞轻则影响系统运行或泄露部分系统非敏感信息，重则导致系统崩溃或任意代码执行。而随着开源软件的快速发展和应用及AI辅助漏洞挖掘方法的使用，越来越多的漏洞被挖掘出来。在此情形中及时得知系统中存在的安全漏洞并对其进行修复是非常之紧要的。  

## CVE

CVE™（Common Vulnerabilities and Exposures，通用漏洞披露）是现今公开披露的安全漏洞的行业标准。[CVE项目](https://www.cve.org)负责识别定义并编目公开披露的安全漏洞，并为目录中的每个安全漏洞赋予一个唯一的CVE编号。避免了各组织各自圈地自萌维护自己的安全漏洞数据库，使得数据无法互通的情况出现。

我们可以在[CVE列表下载页面](https://www.cve.org/Downloads)下载[最新的全部CVE漏洞记录](https://github.com/CVEProject/cvelistV5/archive/refs/heads/main.zip)<sup>注1</sup>，也可以在托管在GitHub的[cvelistV5仓库](https://github.com/CVEProject/cvelistV5)中下载每夜更新的全部CVE漏洞（基准）记录或相对于最近基准记录的在一日内整点时刻更新的累计新增CVE漏洞记录。还可以在[CVE项目网站](https://www.cve.org)的上方横幅处通过CVE编号或关键词搜索需要的CVE漏洞记录。

注1：CVE漏洞记录使用JSON格式进行发布，关于CVE JSON文件记录格式的说明参见[CVE Schema](https://github.com/CVEProject/cve-schema)。  

我们亦可以通过如下网站在线查看已公开披露的安全漏洞列表：  

[（美国）国家安全漏洞数据库(NVD) - 安全漏洞列表（按时间降序）](https://nvd.nist.gov/vuln/search?sortOrder=3&sortDirection=2&resultType=records)  
[NVD](https://nvd.nist.gov/)是兼容CVE的数据库，NVD和CVE保持及时同步。  

[（中国）国家信息安全漏洞库(CNNVD) - 安全漏洞列表](https://www.cnnvd.org.cn/frontend/loophole)  
[CNNVD](https://www.cnnvd.org.cn)也是兼容CVE的数据库，但更新频次相对NVD慢一些。CNNVD漏洞列表中的漏洞默认使用CNNVD编号而非CVE编号，如要查看CVE编号要在列表页面进行任意筛选或进入相应漏洞的详情页查看。

[阿里云漏洞库（AVD）](https://avd.aliyun.com)  
AVD亦是兼容CVE的漏洞数据库，其不单提供了[CVE漏洞列表](https://avd.aliyun.com/nvd/list)，还提供了[非CVE漏洞列表](https://avd.aliyun.com/nonvd/list)。  

在CVE记录中将会包含漏洞的上传时间、公开时间、漏洞标题、漏洞描述、出现漏洞的实体、实体对应厂商、存在漏洞的实体版本范围以及漏洞相关的参考文献，还有可能会包含漏洞的CVSS评级评分（包括各项评分）、[CWE™](https://cwe.mitre.org/)（[Common Weakness Enumeration](https://www.cve.org/CVERecord/UserGuide/#cve-cwe)，通用缺陷枚举）、出现漏洞的实体所在的平台、鸣谢方等信息。我们可以根据这些信息判断正在使用的软硬件中是否存在相应的漏洞。

## CVSS

[CVSS](https://www.first.org/cvss/)（Common Vulnerability Scoring System，通用漏洞评分系统）用于评估一个漏洞的严重程度。其用于根据多个项目计算出漏洞的严重程度评分，范围为0到10（分值越高越严重）。分数还对应表示严重程度的5个级别，对照表如下：  

| 评级 | 分数 |
| --------------------- | - |
| None（无危）			| 0.0 |
| Low（低危）			| 0.1 - 3.9 |
| Medium（中危）		| 4.0 - 6.9 |
| High（高危）			| 7.0 - 8.9 |
| Critical（急危；超危）| 9.0 - 10.0 |

关于CVSS的具体漏洞评分细则参见[Common Vulnerability Scoring System version 4.0: Specification Document](https://www.first.org/cvss/v4-0/cvss-v40-specification.pdf)。  

## Debian安全信息
与Debian相关的安全信息可关注[Debian Security Information](https://www.debian.org/security/)页面及该页面的关联页面。

## 漏洞的修复与规避
如发现系统中存在安全漏洞应及时进行更新修复，如无法（没有能力）进行漏洞的修复则在条件允许的情况下可暂时停止使用存在漏洞的实体。

对于Debian用户及其他常见Linux发行版用户应当要正确配置安全源<sup>注2</sup>并及时进行安全更新。不建议使用停止支持的系统版本。

注2：Debian用户可参考[Debian初步配置](../init-conf.md)中配置安全源段落的内容进行安全源的配置。  

## 参考资料
\[1\] [「 网络安全常用术语解读 」通用漏洞披露CVE详解 - 知乎](https://zhuanlan.zhihu.com/p/702314942)  
\[2\] [CVE,NVD,CNVD,CNNVD的区别 - 知乎](https://zhuanlan.zhihu.com/p/620040297)  
\[3\] [CVE Record Format](https://www.cve.org/AllResources/CveServices#CveRecordFormat)  
\[4\] [CVE List Downloads](https://www.cve.org/Downloads)  
\[5\] [CVE Glossary](https://www.cve.org/ResourcesSupport/Glossary)  
\[6\] [CVE Record User Guide](https://www.cve.org/CVERecord/UserGuide)  
\[7\] [National Vulnerability Database](https://nvd.nist.gov/)  
\[8\] [National Vulnerability Database - General FAQs](https://nvd.nist.gov/general/faq-sections/general-faqs)  
\[9\] [什么是CVE - 华为](https://info.support.huawei.com/info-finder/encyclopedia/zh/CVE.html)  
\[10\] [GB/T 30279-2020 信息安全技术 网络安全漏洞分类分级指南](https://openstd.samr.gov.cn/bzgk/std/showGb?type=download&hcno=458BACCE700CA8E0B728CFB5F762DE7A&request_locale=zh)  
\[11\] [Common Vulnerability Scoring System version 4.0 Specification Document v1.2](https://www.first.org/cvss/v4-0/cvss-v40-specification.pdf)   
\[12\] [CVE-2026-80725](https://www.cve.org/CVERecord?id=CVE-2026-80725)  
\[13\] [CVE-2026-9592](https://www.cve.org/CVERecord?id=CVE-2026-9592)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6.0u | Date: 2026-09-28