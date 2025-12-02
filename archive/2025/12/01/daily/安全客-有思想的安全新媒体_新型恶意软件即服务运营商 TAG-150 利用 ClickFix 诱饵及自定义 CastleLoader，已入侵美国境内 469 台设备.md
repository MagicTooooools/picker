---
title: 新型恶意软件即服务运营商 TAG-150 利用 ClickFix 诱饵及自定义 CastleLoader，已入侵美国境内 469 台设备
url: https://www.anquanke.com/post/id/313509
source: 安全客-有思想的安全新媒体
date: 2025-12-01
fetch_date: 2025-12-02T03:19:04.933804
---

# 新型恶意软件即服务运营商 TAG-150 利用 ClickFix 诱饵及自定义 CastleLoader，已入侵美国境内 469 台设备

首页

阅读

* [安全资讯](https://www.anquanke.com/news)
* [安全知识](https://www.anquanke.com/knowledge)
* [安全工具](https://www.anquanke.com/tool)

活动

社区

学院

安全导航

内容精选

* [专栏](/column/index.html)
* [精选专题](https://www.anquanke.com/subject-list)
* [安全KER季刊](https://www.anquanke.com/discovery)
* [360网络安全周报](https://www.anquanke.com/week-list)

# 新型恶意软件即服务运营商 TAG-150 利用 ClickFix 诱饵及自定义 CastleLoader，已入侵美国境内 469 台设备

阅读量**23615**

发布时间 : 2025-12-01 17:47:16

**x**

##### 译文声明

本文是翻译文章，文章原作者 Ddos，文章来源：securityonline

原文地址：<https://securityonline.info/new-maas-operator-tag-150-uses-clickfix-lure-and-custom-castleloader-to-compromise-469-us-devices/>

译文仅供参考，具体内容表达以及含义原文为准。

![]()

网络犯罪领域迎来了一个 **新型激进参与者**。Darktrace 近期报告揭示了名为 TAG-150 的恶意软件即服务（MaaS）运营商，该组织自 2025 年初出现后活动迅速升级。通过将复杂社会工程与模块化定制恶意软件相结合，该团伙在短短几个月内已成功攻陷美国数百台设备。

### 快速崛起的犯罪基础设施

TAG-150 于 2025 年 3 月首次被发现，随即迅速建立起强大的基础设施。报告指出，该组织“展现出快速开发能力，其庞大且不断演变的基础设施专为支持恶意操作而设计”。

其早期成功规模令人震惊：“截至 2025 年 5 月，仅 CastleLoader 就已感染 469 台设备，凸显了 TAG-150 攻击活动的规模与复杂程度”。这种快速扩散主要针对美国境内目标，并采用 **双轨恶意软件策略**。

### ClickFix：操纵人性的入口向量

TAG-150 的入侵向量较少依赖技术漏洞，更多通过名为 **ClickFix** 的技术操纵人类行为。该方法引导用户访问 **完美模仿合法软件界面** 的欺诈域名，例如谷歌 Meet 或浏览器更新通知。

网站不会要求用户下载文件，而是声称用户缺少“验证步骤”或更新。“当用户点击伪造的 Cloudflare‘验证步骤’提示时……服务器响应会通过‘不安全的 CopyToClipboard() 函数’自动复制到用户剪贴板”。随后用户被诱骗将内容（一段恶意 PowerShell 命令）粘贴到终端，**自行完成黑客攻击**。

### 模块化攻击链：从加载器到远程控制

受害者执行命令后，TAG-150 会部署其定制工具包。攻击始于 **CastleLoader**——一款通过“死代码插入和打包技术阻碍静态与动态分析”的复杂加载器。

“TAG-150 将 CastleLoader 用作初始交付机制，而 CastleRAT 作为主要 payload”。这种模块化分离使攻击者能在确保初始感染后，再激活最有价值的工具。

最终 payload **CastleRAT** 赋予攻击者完全控制权：“部署后，CastleRAT 允许攻击者全面控制受感染系统，具备键盘记录、屏幕捕获和远程 shell 访问等功能”。

### 隐形变种与目标定位

报告还强调了其 RAT 的一个特别隐蔽的 Python 变种 **PyNightShade**。该变种“以隐身设计为核心，在各杀毒平台中检测率极低”。值得注意的是，它会与合法地理定位服务 ip-api[.]com 通信，在完全激活前分析受害者位置，确保攻击目标精准。

本文翻译自securityonline [原文链接](https://securityonline.info/new-maas-operator-tag-150-uses-clickfix-lure-and-custom-castleloader-to-compromise-469-us-devices/)。如若转载请注明出处。

商务合作，文章发布请联系 anquanke@360.cn

本文由**安全客**原创发布

转载，请参考[转载声明](https://www.anquanke.com/note/repost)，注明出处： [https://www.anquanke.com/post/id/313509](/post/id/313509)

安全KER - 有思想的安全新媒体

本文转载自: [securityonline](https://securityonline.info/new-maas-operator-tag-150-uses-clickfix-lure-and-custom-castleloader-to-compromise-469-us-devices/)

如若转载,请注明出处： <https://securityonline.info/new-maas-operator-tag-150-uses-clickfix-lure-and-custom-castleloader-to-compromise-469-us-devices/>

安全KER - 有思想的安全新媒体

分享到：![微信](https://p0.ssl.qhimg.com/sdm/28_28_100/t01e29062a5dcd13c10.png)

* [安全资讯](/tag/%E5%AE%89%E5%85%A8%E8%B5%84%E8%AE%AF)
* [网络攻击](/tag/%E7%BD%91%E7%BB%9C%E6%94%BB%E5%87%BB)

**+1**0赞

收藏

![](https://p1.ssl.qhimg.com/t010857340ce46bb672.jpg)安全客

分享到：![微信](https://p0.ssl.qhimg.com/sdm/28_28_100/t01e29062a5dcd13c10.png)

## 发表评论

您还未登录，请先登录。

[登录](/login/index.html)

![](https://p1.ssl.qhimg.com/t014757b72460d855bf.png)

[![](https://p1.ssl.qhimg.com/t010857340ce46bb672.jpg)](/member.html?memberId=171771)

[安全客](/member.html?memberId=171771)

这个人太懒了，签名都懒得写一个

* 文章
* **760**

* 粉丝
* **6**

### TA的文章

* ##### [Devolutions Server 中存在严重漏洞（CVE-2025-13757），已认证的攻击者可利用SQL注入窃取所有存储的密码](/post/id/313498)

  2025-12-01 17:48:48
* ##### [新型TangleCrypt加壳器可隐藏EDR对抗工具，但因编码缺陷反致勒索软件意外崩溃](/post/id/313501)

  2025-12-01 17:48:27
* ##### [GeoServer 中存在高危漏洞（CVE-2025-58360），可利用未授权的XXE攻击实现文件窃取与服务端请求伪造](/post/id/313505)

  2025-12-01 17:48:01
* ##### [新型恶意软件即服务运营商 TAG-150 利用 ClickFix 诱饵及自定义 CastleLoader，已入侵美国境内 469 台设备](/post/id/313509)

  2025-12-01 17:47:16
* ##### [OpenAI在ChatGPT安卓测试版中启动广告功能测试，引发用户对数据隐私的担忧](/post/id/313514)

  2025-12-01 17:46:49

### 相关文章

* ##### [Devolutions Server 中存在严重漏洞（CVE-2025-13757），已认证的攻击者可利用SQL注入窃取所有存储的密码](/post/id/313498)

  2025-12-01 17:48:48
* ##### [新型TangleCrypt加壳器可隐藏EDR对抗工具，但因编码缺陷反致勒索软件意外崩溃](/post/id/313501)

  2025-12-01 17:48:27
* ##### [GeoServer 中存在高危漏洞（CVE-2025-58360），可利用未授权的XXE攻击实现文件窃取与服务端请求伪造](/post/id/313505)

  2025-12-01 17:48:01
* ##### [OpenAI在ChatGPT安卓测试版中启动广告功能测试，引发用户对数据隐私的担忧](/post/id/313514)

  2025-12-01 17:46:49
* ##### [美国CISA已将OpenPLC ScadaBR系统中正遭积极利用的XSS漏洞（CVE-2021-26829）列入其关键漏洞目录](/post/id/313517)

  2025-12-01 17:46:23
* ##### [“哈希劫持”攻击手法通过操纵URL片段标识符（#）来操控AI浏览器的行为](/post/id/313521)

  2025-12-01 17:45:53
* ##### [“河内窃贼”攻击行动：黑客利用伪多语言LNK/图像混合文件，通过DLL侧加载投送LOTUSHARVEST窃密木马](/post/id/313495)

  2025-12-01 17:45:09

### 热门推荐

文章目录

![](https://p0.qhimg.com/t11098f6bcd5614af4bf21ef9b5.png)

安全KER

* [关于我们](/about)
* [联系我们](/note/contact)
* [用户协议](/note/protocol)
* [隐私协议](/note/privacy)

商务合作

* [合作内容](/note/business)
* [联系方式](/note/contact)
* [友情链接](/link)

内容需知

* [投稿须知](https://www.anquanke.com/contribute/tips)
* [转载须知](/note/repost)
* 官网QQ群：568681302

合作单位

* [![安全KER](https://p0.ssl.qhimg.com/t01592a959354157bc0.png)](http://www.cert.org.cn/)
* [![安全KER](https://p0.ssl.qhimg.com/t014f76fcea94035e47.png)](http://www.cnnvd.org.cn/)

Copyright © 北京奇虎科技有限公司 三六零数字安全科技集团有限公司 安全KER All Rights Reserved [京ICP备08010314号-66](https://beian.miit.gov.cn/)[![](https://icon.cnzz.com/img/pic.gif)](https://www.cnzz.com/stat/website.php?web_id=1271278035 "站长统计")

微信二维码

**X**![安全KER](https://p0.ssl.qhimg.com/t0151209205b47f2270.jpg)