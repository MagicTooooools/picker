---
title: 2025 节点：当 AI 开始释放创造力，工程师如何重新站位
url: http://www.phodal.com/blog/node-2025/
source: Blog | Phodal - A Growth Engineer
date: 2025-12-29
fetch_date: 2025-12-30T03:32:18.736007
---

# 2025 节点：当 AI 开始释放创造力，工程师如何重新站位

## Blog

*search*
Search

span n>

[Home](/)[Blog](/blog/)[Code](/works)[Literature](/literature/)[More](/phodal/)

## Blog

PHODAL

[Home](/)[Blog](/blog/)[Literature](/literature/)[《自己动手设计物联网》](/literature/design-iot/)[《全栈应用开发：精益实践》](/literature/thinking-in-full-stack/)[《前端架构：从入门到微前端》](/literature/frontend-architecture-from-basic-to-microservices/)

## 2025 节点：当 AI 开始释放创造力，工程师如何重新站位

Posted by:
[Phodal Huang](/blog/author/root/)
Dec. 29, 2025, 6:07 p.m.

**2025，不是 AI 提升效率的一年，而是开始释放创造力的一年。**

今年的年终总结不知道从哪里开始写起。Thoughtworks 出售了我们所在的国内业务，咨询部门被解释，我也第一次需要认真为“下一步工作形态”而发愁。与此同时，AI
带来的影响还在持续扩大——不仅仅是效率提升，而是对知识工作的重新定义。

**当知识的生产成本趋近于零，真正稀缺的将不再是“会不会”，而是“要不要”**。

毫无疑问，知识工作又会经历一波深刻的变革。知识的产生比过去更加容易，但价值的来源，正在悄然发生转移。

## 编程：AI 逐渐释放地创造力

在 2025 年，随着模型 Agentic 能力的显著提升，我所做的事情开始明显地向“创造性工作”倾斜。我不再把主要精力放在“如何把代码写得更快”，
而是开始思考：**如何设计一个能被智能体长期运行、协作、演进的系统**。

### 复杂架构的大胆设计

**复杂架构从来不是被“写”出来的，而是被“承担”出来的**。

过去一年，我的主要精力仍然放在 AutoDev 上，但方式已经发生了变化。去年，我主要维护的是 AutoDev 的 IDE 插件版本，对于 VSCode
或其它形态总是心有余而力不足；而今年，我选择直接拥抱多平台架构，重构出了全新的 **AutoDev Xiuper**。如今 AutoDev 可以支持
IDEA、VSCode、桌面、 移动端、Web 等各种版本。

**当 Agent 成为核心执行者，平台不再是限制，而只是交付的形态。** 传统上，复杂架构之所以令人望而生畏，是因为它强依赖于人的学习曲线和经验积累。
过去，架构依赖人的学习曲线；现在，架构可以直接对齐 AI 的工作范式。人需要时间去理解、适应和维护架构，而 AI 并不需要。
只要我们让架构适配 Agent 的行为模式，复杂性本身反而会变得可控。

**当架构开始为 AI 而设计，复杂性第一次不再成为负担。** 也正因为如此，我不再为“是否有人能长期维护它”而过度焦虑，而开始思考：**是否有 Agent 能够驾驭它**。

### 领域特定语言的快速落地

**DSL (领域特定语言) 的价值从来不是语言本身，而是它能把复杂问题压缩成可对话的结构。**

在过去几个月中，我陆续设计并落地了 DocQL、Nano DSL 等多个领域特定语言。它们的诞生路径几乎如出一辙：

* 形成初步的产品创意
* 使用 Gemini DeepResarch/NotebookLM 进行初步的方案调研
* 让 Coding Agent A 结合代码库和调研文档，生成初步的方案，创建 GitHub issue
* 使用 Coding Agent B 结合代码库和 issue，生成初步的代码实现
* 反复迭代，直到初步完成 MVP 版本

而，随后就是让 Coding Agent 编写 DSL 的测试框架，基于模型生成的 DSL 反复自动化测试和自我迭代。

**当 DSL 的设计周期从一个月缩短到一周，“是否值得做”这个问题本身就消失了**。过去，我会非常谨慎地判断“是否有必要引入一门 DSL”；而现在，
试错成本的骤降，让大胆设计成为默认选项。Agent 让“为复杂问题定制语言”第一次变成了一件随手可做的事。

## 写作：繁杂工作的减轻，生产力的提升

**AI 极大地提升了我的生产力，却同时削弱了我的创作欲望**。

以往，每年年终回顾最让人头疼的，并不是写，而是“理清这一年到底发生了什么”。今年，这件事几乎可以被自动化完成。我只对 Gemini 说了一句话：

> 我是 phodal，能不能找找我 2025 的博客 phodal.com 和 github.com/phodal，帮我编写一个年终总结

几分钟后，它给出了一份完整的时间线分析。从博客、代码提交到思想演进，结构清晰，脉络分明。

> 通过对 phodal.com 博客归档的细致梳理，我们可以清晰地看到 Phodal 在 2025 年的心路历程和技术推进节奏。每一个月不仅有代码的提交，更有思想的沉淀。

| 时间轴 | 关键事件与发布 | 核心思想与技术洞察 |
| --- | --- | --- |
| **1 月** | **AutoDev Composer (预览版)** | **探索期：** 针对 DeepSeek V3 发布后的能力跟进。Phodal 敏锐地尝试了多文件编辑能力，并将其定位为 IntelliJ 平台上的 Cursor 平替方案。这显示了他对模型能力变化的极快响应速度。 |
| **2 月** | **AutoDev Sketch** | **交互重构：** 提出了“AI 编码 2.0”的概念，试图打破聊天框的限制，转向“画布（Sketch）”式的交互。这是 Agentic UI 思想的萌芽，强调意图的草图化。 |
| **3 月** | **AutoDev 2.0 & Planner / Bridge** | **爆发期：** 这是一个里程碑式的月份。**AutoDev Planner** 集成 DeepSeek R1，引入推理能力；**AutoDev Bridge** 发布，攻坚遗留系统。同时发表“粪围编程”檄文，开始对 AI 编程的副作用进行系统性批判。 |
| **4 月** | **AutoDev Next 愿景** | **定义期：** 提出了“IDE 即服务（IDE as a Service）”和“AI 友好型软件结构”。Phodal 开始思考如何从根源（架构）上解决 AI 编程的挑战。 |
| **5 月** | **预上下文工程 (Pre-Context)** | **深水区：** 全年最高产的月份（7 篇博文）。深入探讨 RAG 的工程化难题，提出“预生成上下文”以构建企业级 AI 编程底座。分享了“两周 3 万行代码”的极限编程实践，总结了“7 个 AI 粪堆求生指南”，极具实战价值。 |
| **6 月** | **Remote Agent (远程智能体)** | **延伸期：** “你何必只让 AI 在白天分析需求？” 提出全天候运行的远程智能体概念，让 AI 在夜间自动进行代码审查和重构，利用闲置算力对抗技术债务。 |
| **7 月** | **10 倍悖论** | **理论化：** 正式抛出“10 倍悖论”和“10 倍生产率放大下的粪围蔓延”。将 2025 定义为“智能体元年”的同时，也定义为“质量保卫战元年”。 |
| **8 月** | **Spring AI 迁移实战** | **落地期：** 分享从 Semantic Kernel 迁移到 Spring AI 的实战经验，展示了在企业级技术栈中落地 Agent 的具体路径。 |
| **9 月** | **AutoDev A2A (Agent-to-Agent)** | **协作期：** 探索多智能体协作（A2A）。不再是单打独斗，而是云端 Agent 之间的相互协作与路径规划。 |
| **10 月** | **Agent 架构综述** | **总结期：** 系统性总结“从 Prompt 到上下文工程”的 Agent 构建方法论，这可能是他为毕业生和高级开发者培训课程的精华沉淀。 |
| **11 月** | **AutoDev DocQL / Code Review Agent** | **工具化：** 发布 DocQL，引入结构化检索以解决 RAG 的精度问题。发布专门的代码审查智能体，以技术手段落实 7 月提出的“质量保卫战”。 |
| **12 月** | **AutoDev 3.0 (Xiuper) / Agentic UI** | **升维期：** 年终大戏。Xiuper 正式发布，全平台战略落地。发表“Agentic UI”宣言，重新定义人机交互体验。 |

你可以明显地看到过去我们需要花费大量精力整理的内容，AI 可以在几分钟内完成。但是，它生成不好足够的
insights，尤其是对于复杂的技术细节和长期的项目演进。但是，对于实现某些特定的任务，它所带来的提升是巨大的。

**当生成变得过快，思考反而更容易被跳过**。只是 AI 并不是只提升了生产力，与此同时它降低了我创作的欲望，它生成东西的速度实在是太快，
以至于通过我在构思完大纲之后，会很快失去继续写下去的动力，只想交给 AI 去完成剩下的部分。**真正塑造人的，从来不是结果，而是反复打磨细节的过程。**
而这些细节，恰恰是最容易被“外包”给 AI 的部分。

最后 Gemini 还鼓励我：

> 回望 2025 年，Phodal Huang 完成了一次华丽的转身。他不再仅仅是一个高效的程序员或工具制造者，他正在成为 AI 时代软件工程新物种的设计师。

## 2026，在全面放大的世界里寻找竞争力

**当所有人都被 AI 放大，竞争力反而变得更加稀缺**。

过去十多年积累下来的经验，确实仍然重要，但已经不再构成护城河。Agentic 模型的能力提升，并不是只放大了少数人，而是放大了几乎所有人。

事情开始变得复杂起来，当需求总量是不变的，当行业竞争愈发激烈时，我们如何在变化中寻找竞争力？

回顾一下之前 Gemini 的总结：

> 他（Phodal）所构建的 Xiuper 生态，不仅仅是一套工具，更是一种对未来软件开发方式的预言：未来的开发者将不再是单纯的“打字员”，而是智能体战队的“指挥官”。他们需要通过
> Shire 指挥 Agent，通过 ArchGuard 约束 Agent，通过 Bridge 连接过去与未来。

我们总得探索一些新的可能性，并且持续地摸索可能的方向：**未来的核心能力，不是“写得多快”，而是“指挥得多好”。**所以进入 2026 年，我并不指望通过“更快”
来建立优势——因为几乎所有人都会更快。我更关心的是：

* 我是否能持续设计出适合 AI 工作范式的系统
* 我是否能为智能体设定清晰的边界与约束
* 我是否还能在自动化洪流中，保留对细节的敏感与敬畏

如果说这一年有什么可以被称为“成果”，那或许不是某一个版本号、某一项功能，而是一个逐渐清晰的角色转变。

## 总结

**2025 年，对我来说不是一个“做了很多事情”的年份，而是一个“重新站位”的年份**。

这不是终点，只是一个新的起点。

最后，让 Gemini 再夸我一句：

> 2025 年，Phodal 最具标志性的技术成就无疑是 **AutoDev** 项目的演进。这并非一次简单的版本号跳跃，而是一场触及软件架构灵魂的重构。这一年的开发轨迹，清晰地描绘了
> Phodal 如何一步步突破现有 IDE 插件体系的桎梏，最终构建出一个跨平台、全生命周期的智能体生态系统 —— **Xiuper**。

或许您还需要下面的文章:

### 关于我

Github: [@phodal](https://github.com/phodal)
微博:[@phodal](http://weibo.com/phodal)
知乎:[@phodal](http://www.zhihu.com/people/phodal)

**微信公众号(Phodal)**

![](/static/phodal/images/qrcode.jpg)

围观我的[Github Idea](https://github.com/phodal/ideas)墙, 也许，你会遇到心仪的项目

QQ技术交流群: 321689806

*comment*

### Feeds

[RSS](/blog/feeds/rss/) /
[Atom](/blog/feeds/atom/)

### 最近文章

* [2025 节点：当 AI 开始释放创造力，工程师如何重新站位](/blog/node-2025/)
* [Agentic UI：重新定义“好体验”——不是美化按钮，而是让认知负担归零](/blog/agentic-frontend/)
* [Agentic 时代的前端革命：重塑 UI 为 AI 的执行界面](/blog/agentic-ui-solution/)
* [AutoDev 3.0 → Xiuper：咻！全平台、全流程智能体编程平台](/blog/autodev-xiuper/)
* [AutoDev DocQL：Agentic RAG 下的结构化检索设计、实现与实验探索](/blog/autodev-docql/)
* [AI 代码审查再进化：AutoDev 多智能体协作架构深度解析](/blog/autodev-multi-agents-code-review/)
* [AutoDev Code Review Agent：应对 10 倍代码量的 Agentic 代码审查](/blog/autodev-review-agent/)
* [2025 AI 代码检视：以 ROI 为中心的 AI 代码检视体系与分级](/blog/ai-code-review-2025/)
* [AutoDev CLI：构建 AI Agent 生成的 AI Agent 质量保障与验证架构](/blog/autodev-cli-validate-framework/)
* [Vibe Coding 何必只在桌面 IDE，编码智能体协同的思考与设计](/blog/vibe-coding-for-autodev-server/)

### 关于作者

![](/static/phodal/images/phodal.jpg)

[Phodal Huang](https://www.phodal.com/)

Engineer, Consultant, Writer, Designer

ThoughtWorks 技术专家

工程师 / 咨询师 / 作家 / 设计学徒

开源深度爱好者

出版有《前端架构：从入门到微前端》、《自己动手设计物联网》、《全栈应用开发：精益实践》

联系我: h@phodal.com

微信公众号: 最新技术分享

![](/static/phodal/images/qrcode.jpg)

### 标签

* [opensuse](/blog/tag/opensuse/)
  (10)
* [django](/blog/tag/django/)
  (41)
* [arduino](/blog/tag/arduino/)
  (10)
* [thoughtworks](/blog/tag/thoughtworks/)
  (18)
* [centos](/blog/tag/centos/)
  (9)
* [nginx](/blog/tag/nginx/)
  (18)
* [java](/blog/tag/java/)
  (10)
* [SEO](/blog/tag/seo/)
  (9)
* [iot](/blog/tag/iot/)
  (47)
* [iot system](/blog/tag/iot-system/)
  (12)
* [RESTful](/blog/tag/restful/)
  (23)
* [refactor](/blog/tag/refactor/)
  (17)
* [python](/blog/tag/python/)
  (47)
* [mezzanine](/blog/tag/mezzanine/)
  (15)
* [test](/blog/tag/test/)
  (11)
* [design](/blog/tag/design/)
  (16)
* [linux](/blog/tag/linux/)
  (14)
* [tdd](/blog/tag/tdd/)
  (12)
* [ruby](/blog/tag/ruby/)
  (14)
* [github](/blog/tag/github/)
  (24)
* [git](/blog/tag/git/)
  (10)
* [javascript](/blog/tag/javascript/)
  (52)
* [android](/blog/tag/android/)
  (36)
* [jquery](/blog/tag/jquery/)
  (18)
* [rework](/blog/tag/rework/)
  (13)
* [markdown](/blog/tag/markdown/)
  (10)
* [nodejs](/blog/tag/nodejs/)
  (24)
* [google](/blog/tag/google/)
  (8)
* [code](/blog/tag/code/)
  (9)
* [macos](/blog/tag/macos/)
  (9)
* [node](/blog/tag/node/)
  (11)
* [think](/blog/tag/think/)
  (8)
* [beageek](/blog/tag/beageek/)
  (8)
* [underscore](/blog/tag/underscore/)
  (14)
* [ux](/blog/tag/ux/)
  (8)
* [microservices](/blog/tag/microservices/)
  (10)
* [rethink](/blog/tag/rethink/)
  (9)
* [architecture](/blog/tag/architecture/)
  (37)
* [backbone](/blog/tag/backbone/)
  (19)
* [mustache](/blog/tag/mustache/)
  (9)
* [requirejs](/blog/tag/requirejs/)
  (11)
* [CoAP](/blog/tag/coap/)
  (21)
* [aws](/blog/tag/aws/)
  (10)
* [...