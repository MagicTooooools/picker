---
title: 并发场景下的竞态条件 | 小记
url: https://moeci.com/posts/2025/11/%E5%B9%B6%E5%8F%91%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AB%9E%E6%80%81%E6%9D%A1%E4%BB%B6-race-condition-note/
source: yiyun's Blog
date: 2025-11-26
fetch_date: 2025-11-27T16:52:39.162522
---

# 并发场景下的竞态条件 | 小记

[yiyun's Blog](/)

愚蠢的是我

* [首页](/)
* [分类](/categories/)
* [标签](/tags/)
* [归档](/archives/)
* [关于](/about/)
* [作品集](/portfolio/)
* [友链](/friends/)
* [RSS](/atom.xml)

* 文章目录
* 站点概览

1. [𥧦𧟎](#%F0%A5%A7%A6%F0%A7%9F%8E)
2. [𢲳𪳌](#%F0%A2%B2%B3%F0%AA%B3%8C)
3. [𨗔𠧼𫦂](#%F0%A8%97%94%F0%A0%A7%BC%F0%AB%A6%82)
4. [𣩄𥛄](#%F0%A3%A9%84%F0%A5%9B%84)
5. [𮆚𤇩](#%F0%AE%86%9A%F0%A4%87%A9)

yiyun

Data Science, Machine Learning, Fullstack Web Developer

[177
日志](/archives/)

[72
分类](/categories/)

[115
标签](/tags/)

[GitHub](https://github.com/yiyungent "GitHub → https://github.com/yiyungent")

[Gitee](https://gitee.com/yiyungent "Gitee → https://gitee.com/yiyungent")

[E-Mail](/cdn-cgi/l/email-protection#dfb69fb2b0babcb6f1bcb0b2 "E-Mail → mailto:i@moeci.com")

0%

Theme NexT works best with JavaScript enabled

# 并发场景下的竞态条件 | 小记

发表于
2025-11-26

分类于

[其它](/categories/%E5%85%B6%E5%AE%83/)

本文字数：
1.6k

阅读时长 ≈
6 分钟

请等待 自动解密 !!! 请等待 自动解密 !!! 请等待 自动解密 !!!

* **本文作者：** yiyun
* **本文链接：**
  [https://moeci.com/posts/2025/11/并发场景下的竞态条件-race-condition-note/](https://moeci.com/posts/2025/11/%E5%B9%B6%E5%8F%91%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AB%9E%E6%80%81%E6%9D%A1%E4%BB%B6-race-condition-note/ "并发场景下的竞态条件 | 小记")
* **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明出处！

[# 其它](/tags/%E5%85%B6%E5%AE%83/)

[Elasticsearch 海量数据查询原理 | 小记](/posts/2025/10/elasticsearch%E6%B5%B7%E9%87%8F%E6%95%B0%E6%8D%AE%E6%9F%A5%E8%AF%A2%E5%8E%9F%E7%90%86-note/ "Elasticsearch 海量数据查询原理 | 小记")

[渝ICP备16001348号-1](https://beian.miit.gov.cn/)
![](/static/images/gongan_icon.png)[渝公网安备50022202000018号](http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=50022202000018)

© 2020 –
2025

yiyun

站点总字数：
652k

站点阅读时长 ≈
39:31

由 [Hexo](https://hexo.io/) & [NexT.Pisces](https://theme-next.js.org/pisces/) 强力驱动

本网站由
[![](/static/images/又拍云_logo2.png)](https://www.upyun.com/?utm_source=lianmeng&utm_medium=referral)
提供CDN加速服务