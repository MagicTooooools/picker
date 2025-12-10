---
title: 让 Fava 不再展示 0 余额账户
url: https://www.tortorse.com/archives/fava-hide-zero-balance-accounts/
source: 愆伏
date: 2025-12-09
fetch_date: 2025-12-10T03:26:08.245194
---

# 让 Fava 不再展示 0 余额账户

[愆伏](/)

互联网杂谈

* [首页](/)
* [分类](/categories/)
* [关于](/about/)
* [归档](/archives/)
* [标签](/tags/)
* [实验室](/lab/)
* [链接](/links/)
* [统计](/statistics/)
* [播客](/podcast/)
* 搜索

* 文章目录
* 站点概览

![tortorse](/assets/images/avatar.png)

tortorse

一个浸淫互联网行业多年的斜杠中年，通过博客表达自己的所思所想以及所经历的历史进程

[509
日志](/archives/)

[8
分类](/categories/)

[306
标签](/tags/)

[![Creative Commons](https://cdnjs.cloudflare.com/ajax/libs/creativecommons-vocabulary/2020.11.3/assets/license_badges/small/by_nc_sa.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/zh-CN)

相关文章

* [2022-10-01

  MacOS 下 fava 无法打开的问题及解决方案](/archives/996/)
* [2022-10-03

  清除 fava 中 BQL 查询结果](/archives/997/)

# 让 Fava 不再展示 0 余额账户

发表于
2025-12-09

分类于

[技术](/categories/%E6%8A%80%E6%9C%AF/)

评论数：

阅读次数：

本文字数：
501

阅读时长 ≈
1 分钟

用 Beancount 记账时间长了之后，我发现有些账户很久已经没有账目、余额也是 0 了，但在 Fava 的账户列表里依旧会展示，看着有点碍眼。

最近在 Beancount 群里看到有人问同样的问题，群里高手如云，很快给出了关键字：`show-accounts-with-zero-balance`。

我一开始以为，这个设置和 Beancount 自己的配置一样，应该是：

|  |  |
| --- | --- |
| ``` 1 ``` | ``` option "show-accounts-with-zero-balance" "true" ``` |

于是我也试着用类似格式去改，结果发现完全没用，Fava 里的账户列表一点变化都没有。

查了一下才发现，正确的写法其实是：

|  |  |
| --- | --- |
| ``` 1 ``` | ``` 2025-01-01 custom "fava-option" "show-accounts-with-zero-balance" "false" ``` |

也就是说，这其实是 Fava 自己的配置，需要通过 `custom "fava-option"` 来设置，而不是用 Beancount 的 `option`。

加上这一行之后，Fava 就不会再显示 0 余额的账户了，界面一下子清爽了不少。

请我一杯咖啡吧！

赞赏

![tortorse 微信](/assets/images/wechat.png)
微信

* **原作者：** 愆伏
* **本文链接：**
  [https://www.tortorse.com/archives/fava-hide-zero-balance-accounts/](https://www.tortorse.com/archives/fava-hide-zero-balance-accounts/ "让 Fava 不再展示 0 余额账户")
* **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/zh-CN) 许可协议。转载请注明出处！

[# 记账](/tags/%E8%AE%B0%E8%B4%A6/)
[# Beancount](/tags/Beancount/)
[# Fava](/tags/Fava/)

[准备弃用 Logseq](/archives/preparing-to-ditch-logseq/ "准备弃用 Logseq")

© 2003 –
2025

tortorse

254k

7:42

0%

Theme NexT works best with JavaScript enabled