---
title: AutoDev 3.0 → Xiuper：咻！全平台、全流程智能体编程平台
url: http://www.phodal.com/blog/autodev-xiuper/
source: Blog | Phodal - A Growth Engineer
date: 2025-12-16
fetch_date: 2025-12-17T03:23:19.578374
---

# AutoDev 3.0 → Xiuper：咻！全平台、全流程智能体编程平台

## Blog

*search*
Search

span n>

[Home](/)[Blog](/blog/)[Code](/works)[Literature](/literature/)[More](/phodal/)

## Blog

PHODAL

[Home](/)[Blog](/blog/)[Literature](/literature/)[《自己动手设计物联网》](/literature/design-iot/)[《全栈应用开发：精益实践》](/literature/thinking-in-full-stack/)[《前端架构：从入门到微前端》](/literature/frontend-architecture-from-basic-to-microservices/)

## AutoDev 3.0 → Xiuper：咻！全平台、全流程智能体编程平台

Posted by:
[Phodal Huang](/blog/author/root/)
Dec. 16, 2025, 10:18 p.m.

**AutoDev Xiuper 3.0.0-alpha** 正式发布！这是一款基于 Kotlin Multiplatform (KMP) 构建的 **全生命周期 AI 编程助手**
，覆盖从需求分析、代码编写、文档管理到代码审查的完整开发流程，并且支持 Desktop（macOS/Windows/Linux）、CLI、VSCode、JetBrains
IDE、Web、Android 和 iOS 全平台运行。

官网：<https://xiuper.com>
GitHub：https://github.com/phodal/auto-dev

## 下载 AutoDev Xiuper

* **IntelliJ IDEA Plugin**: https://plugins.jetbrains.com/plugin/29223-autodev-experiment
* **VSCode Extension**: https://plugins.jetbrains.com/plugin/29223-autodev-experiment
* **CLI Tool**: `npm install -g @xiuper/cli`
* **Web Version**: https://web.xiuper.com/
* **Desktop & Android**: https://github.com/phodal/auto-dev/releases

过去两年，我们看到了大量 AI 编程工具、AI IDE 的出现。它们大多解决了一个局部问题：**如何更快地写出一段代码**。
但在真实的软件工程中，代码从来不是问题的起点，也不是终点。真正的开发流程是一个完整的 **SDLC（Software Development Life
Cycle，
软件开发生命周期）**：

```
需求 → 设计 → 实现 → 测试 → 文档 → 审查 → 交付 → 演进
```

## Xiuper：一个关于「存在方式」的名字

过去几年，我们在不同客户现场持续构建智能体与 AI 辅助开发能力。从最初的代码生成，到逐步介入设计、审查、文档与交付过程，我们越来越清晰地意识到一件事：
**AI 真正产生价值的方式，并不是被频繁“调用”，而是自然地“存在”于开发流程之中。**

这也是为什么，在重构 AutoDev 到全新的多平台架构之上时，我们开始重新思考这个产品的名字。

### 从 AutoDev 开始，但不止于 AutoDev

AutoDev 这个名字，源于“自动化开发”。在很长一段时间里，它准确地描述了我们正在解决的问题：让 AI
帮助开发者更快地完成编码工作。但随着实践的深入，这个边界逐渐变得狭窄。

在真实的软件工程现场，开发并不止于 Coding。更多的时间消耗在理解上下文、确认影响范围、评估风险、维护知识一致性以及跨角色协作上。当
AI 能力开始覆盖这些环节时，“Auto-Dev” 已经不足以承载我们想要构建的东西。

我们需要一个新的名字，来表达这种变化。

### 「咻」：AI 出现的正确方式

PS：此名字来源于我的同事 Sara 的提议: xiiu per fast。

Xiuper 的第一个音节，来自一个极其直观的拟声词 —— **“咻”**。它代表的是一种瞬间发生、无需准备的状态。不是“打开一个工具”，而是在需要的时候，它已经在那里\*\*。在 IDE 里写代码时，在 Terminal 调试时，在浏览器里检查页面时，甚至在移动设备上回看文档或审查结果时，AI
都不应该要求开发者切换思路、迁移上下文或改变工作方式。

“咻”所表达的，是一种 **无处不在但不打扰的存在感**。这正是我们希望 Xiuper 在 Every Device、Every 场景中呈现的状态。

### 「super」：不是助手，而是能力层

Xiuper 的后半部分来自 “super”，但它并不指向“更强的 AI”，而是指向一种 **能力放大**。在 Xiuper 中，AI
并不是一个只负责回答问题的助手，而是作为工程体系的一部分参与进来。它理解项目结构、理解设计决策、理解历史上下文，并在合适的阶段承担合适的角色。

这种 “super” 并非替代工程师，而是让工程系统本身变得更具弹性、更可扩展、更易演进。

当 AutoDev 被重构为基于 Kotlin Multiplatform 的多端统一平台后，我们希望它不再被绑定在某一个工具形态上。它可以是 CLI、是 IDE
插件、是桌面应用、也是 Web 或移动端能力的一部分。

在这个意义上，Xiuper 不是一个“应用”的名字，而是一个 **平台层的名字**。它承载的是一个明确的方向：**以 SDLC 为主线，以智能体为一等公民，让
AI 成为软件工程中自然存在的能力层。**

## One Platform 架构：跨阶段、跨端的 AI 原生开发

与传统 AI 编程工具不同，它不是单点功能的集合，而是以 **软件生命周期（SDLC）为主线**，将 AI
能力嵌入每一个开发环节。从需求理解、架构设计、代码实现，到测试验证、文档编写、代码审查，乃至 UI 调整和团队协作，Xiuper 的每一个
Agent 都对应真实工程角色，实现完整的 **Plan → Execute → Verify → Reflect** 闭环。

在这个平台中：

**理解与探索**由 Chat 和 Knowledge Agent 负责，帮助开发者精准定位文档、架构和历史决策；
**实现与修改**由 Agentic Agent 执行，支持上下文感知的多轮迭代开发；

* **数据分析与验证**由 ChatDB Agent 完成，自然语言查询数据库并可视化结果；
* **审查与质量控制**由 Code Review Agent 管理，生成可执行修复建议而非仅评论；
* **UI / Web 调整**由 WebEdit Agent 执行，实现 DOM 与源码的双向映射；
* **团队协作与资源共享**通过 Remote Agent 统一管理，支持多端、多人协作。

通过统一的多平台架构（Kotlin Multiplatform + MCP 扩展协议），Xiuper 实现了 **跨设备、跨端、跨阶段的一致体验**：无论是在
IDE、CLI、Desktop、Web 还是移动端，AI 都能**自然存在、即时响应**。这就是 Xiuper 的 One Platform
架构——不只是一个工具，而是整个软件工程中持续可用的智能体能力层。

## All Phases：完整覆盖 SDLC 的智能体能力

Xiuper 的设计目标是 **贯穿整个软件开发生命周期**（SDLC），每一个阶段都由专门的智能体（Agent）支持，从需求理解到运维管理，形成完整的
**AI 原生开发闭环**。

| 阶段 | Agent 名称 | 核心能力 | 状态 |
| --- | --- | --- | --- |
| **需求** | Knowledge | 需求理解与知识构建，AI 原生的文档阅读和分析能力。核心功能包括 DocQL、上下文工程。 | 可用 |
| **开发** | Coding | 自主编码代理，具备完整的文件系统、Shell 和工具访问能力。支持 MCP、SubAgents 以及 DevIns DSL。 | 可用 |
| **代码审查** | Code Review | 专业代码审查，分析代码质量、安全性、性能与最佳实践。提供 Linter、摘要生成及自动修复功能。 | 可用 |
| **测试** | Testing | 自动化测试代理，生成测试用例、执行测试并分析覆盖率。支持端到端测试、自愈测试与覆盖率分析。 | 规划中 |
| **数据** | ChatDB | 数据库对话代理，支持自然语言查询（Text-to-SQL）、多数据库操作与 Schema 链接。 | 可用 |
| **部署** | WebEdit | 网页编辑代理，可浏览网页、选择 DOM 元素并与页面交互，实现源码映射与修改。 | Beta |
| **运维** | Ops | 运维监控代理，提供日志分析、性能监控与告警处理能力。 | 规划中 |

通过这种 **以阶段为中心的 Agent 编排**，Xiuper 能够保证：

* AI 不仅在某一个环节发挥作用，而是贯穿整个开发流程
* 每个 Agent 都有清晰的职责和工具能力，能够在对应阶段独立或协作完成任务
* 用户在不同阶段无感切换上下文，AI 能自然地参与每一步决策和执行

## Every Device：AI 随时随地伴随开发者

在现代软件开发中，工作环境高度碎片化。开发者可能同时：

* 在 IDE 中编写和调试代码
* 在 Terminal 或 CLI 中运行脚本
* 在会议或移动设备上查阅文档
* 在 Web 页面上验证或调整 UI

在这种场景下，AI 的价值不在于“单独工具”，而在于**随时随地可用**。Xiuper 正是为此而生，它的能力覆盖了：

* **Desktop**（macOS / Windows / Linux）
* **CLI / TUI**（交互式终端）
* **IDE 扩展**（VS Code / JetBrains 系列）
* **Web 与移动端**（Web / Android / iOS）

通过统一的多平台架构，Xiuper 确保了**开发者无需迁移上下文**，AI 能自然地出现在你正在工作的地方，无缝支持整个 SDLC 的每一步操作。

这就是 Xiuper 的 **Every Device** 理念：**AI 不追随工具，而追随开发者的工作流**。

### 结语

AutoDev Xiuper 不只是一个 AI 工具，更不是一个单点功能的集合。它是一个**以 SDLC 为核心、全生命周期贯穿的智能体原生开发平台**
，将 AI 自然地嵌入每一个开发环节：从需求理解、架构设计，到代码实现、测试验证、文档管理与代码审查，再到部署与运维。

Xiuper 的价值不在于“能做多少事”，而在于**以正确的方式存在**：在任何设备、任何场景中，随时随地伴随开发者的工作流程，无缝支持每一步决策与执行。它让
AI 不再是工具，而成为**工程系统中天然的能力层**——理解项目、理解上下文、理解团队，主动提供决策支持、自动化执行与质量保障。

**One Platform. All Phases. Every Device.**
这不仅是一句口号，更是我们对未来软件工程的愿景：\*\*AI 与开发者共生，成为工程能力的自然延伸，让每一个软件项目都更高效、更可演进、更有智慧。

官网：<https://xiuper.com>

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

* [AutoDev 3.0 → Xiuper：咻！全平台、全流程智能体编程平台](/blog/autodev-xiuper/)
* [AutoDev DocQL：Agentic RAG 下的结构化检索设计、实现与实验探索](/blog/autodev-docql/)
* [AI 代码审查再进化：AutoDev 多智能体协作架构深度解析](/blog/autodev-multi-agents-code-review/)
* [AutoDev Code Review Agent：应对 10 倍代码量的 Agentic 代码审查](/blog/autodev-review-agent/)
* [2025 AI 代码检视：以 ROI 为中心的 AI 代码检视体系与分级](/blog/ai-code-review-2025/)
* [AutoDev CLI：构建 AI Agent 生成的 AI Agent 质量保障与验证架构](/blog/autodev-cli-validate-framework/)
* [Vibe Coding 何必只在桌面 IDE，编码智能体协同的思考与设计](/blog/vibe-coding-for-autodev-server/)
* [多端编程 Agent（CLI/Desktop/Mobile）：AutoDev 架构升级，欢迎一起参与演进](/blog/autodev-next-poc/)
* [Agent 架构综述：从 Prompt 到上下文工程构建 AI Agent](/blog/prompt-to-agenticg/)
* [AutoDev A2A 新能力下的云端 Agent 路径思考，从扩展到协作](/blog/autodev-a2a/)

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
* [CoAP](/blog/tag/coap...