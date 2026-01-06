---
title: 生成式大语言模型（LLM：eg：deepseek）用法技巧
url: https://liupj.top/2026/01/05/AI-learning-1/
source: peijie's wiki
date: 2026-01-05
fetch_date: 2026-01-06T03:32:11.280363
---

# 生成式大语言模型（LLM：eg：deepseek）用法技巧

[peijie's wiki](/)

* [Home](/)
* [Contact](/contact)
* [GitHub](https://github.com/brannua)
* [Travelling](https://travellings.link)
* [WormHole](https://foreverblog.cn/go.html)
* [RSS](/atom.xml)

# 生成式大语言模型（LLM：eg：deepseek）用法技巧

2026-01-05

### 撰写高质量输入的一些技巧

LLM 的本质是一个输入输出程序软件体，这意味着：

结构化（eg：markdown）表达充分而清晰、具备强引导性的输入，能获取更棒的输出（good question can get good answer）

```
结构化（eg：markdown）能够帮助 LLM 精准辨识：对于输入，哪些部分应当被识别为一个完整的意义单元
```

1. 让 LLM 软件体“身临其境”地理解某一具体需求的场景上下文，以便给出更棒的输出

   实操步骤：
    赋予 LLM 特定角色，约束 LLM 的回答范畴（也可理解为规则限制，比如让其只能给出法律相关的回答）
2. 给 LLM 提供高质量输出的例子，让其模仿
3. 分治法：复杂问题可拆解成 n 个小问题，让 LLM 逐个击破

[Back to Top](#top)

© 2026
lpj