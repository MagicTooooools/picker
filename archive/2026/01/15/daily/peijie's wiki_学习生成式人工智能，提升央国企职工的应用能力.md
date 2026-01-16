---
title: 学习生成式人工智能，提升央国企职工的应用能力
url: https://liupj.top/2026/01/15/AI-learning-of-state-owned-enterprise/
source: peijie's wiki
date: 2026-01-15
fetch_date: 2026-01-16T03:35:00.983246
---

# 学习生成式人工智能，提升央国企职工的应用能力

[peijie's wiki](/)

* [Home](/)
* [Contact](/contact)
* [GitHub](https://github.com/brannua)
* [Travelling](https://travellings.link)
* [WormHole](https://foreverblog.cn/go.html)
* [RSS](/atom.xml)

# 学习生成式人工智能，提升央国企职工的应用能力

2026-01-15

# 生成式-大语言模型(LLM：eg：deepseek)的用法技巧

## 撰写高质量输入的技巧

* LLM 的本质是一个输入输出程序，这意味着：

  + 【结构化的(eg: markdown)、表达清晰充分的、引导性强的】输入，能获得更棒的输出(good question can get good answer)
  + 结构化能帮助 LLM 精准辨识：对于输入，哪些部分应当被识别为一个完整的意义单元
* 赋予 LLM 特定的角色 & 约束其回答范畴(即：规则限制，比如让其只能给出法律相关的回答)

  + 这能让 LLM 身临其境地充分理解当前需求的场景上下文，以便给出更棒的输出
* 可给 LLM 提供高质量输出的例子，让其模仿
* 分治法：复杂问题可拆解成 n 个小问题，让 LLM 逐个击破

## 既然如此，编写高质量输入能不能总结出来套路/范式/模板/框架？这不就省事了么，也便于引导AI，产出更准确、更符合预期的内容

本质就是【表达的方法论】

1. 5W1H

   * Who
   * What
   * When
   * Where
   * Why
   * How
2. BROKE

   * Background 说明背景
   * Role 定义角色
   * Objectives 实现什么
   * Keyresult 具体效果
   * Evolve 改进(eg: 可以改进输入，可以改进输出，可以让AI不断重新回答优中选优…)

+markdown

## 这也太复杂了，既然我不太会写高质量的输入(提示词)，那就让AI来写！

Kimi-提示词专家(AI智能体)

## DeepSeek 的深度思考模式是什么鬼？

深度思考模式调用的是【推理型模型】，与【指令型模型】不同

* 指令型模型，对提示词的依赖程度高，好的提示词，效果立竿见影，eg: DeepSeek V3
  + 聪明又听话，高效又便捷，适合大多数任务，即规范性任务
* 推理型模型，对提示词的依赖程度低，eg: DeepSeek R1, Kimi1.5 …
  + 聪明但没有那么听话的分析推理官，适用于创意思考、分析推理、…

## 深度思考模式下，输入(提示词)的模板

1. 背景信息：AI需要了解上下文
2. 直接需求：结构化表达清楚
3. 约束条件

# AI智能体(AI-Agent)

* <https://www.coze.cn/>

1. 赋予 LLM 特定角色(编写提示词)
2. 插件(第三方工具集)
3. 工作流
4. 记忆系统
5. 知识库

# AI-PPT

## 全套快速制作

* kimi: PPT助手(AI智能体)
* <https://www.aippt.cn/> 提供了自定义PPT模板的功能

## 单页快速美化

[Back to Top](#top)

© 2026
lpj