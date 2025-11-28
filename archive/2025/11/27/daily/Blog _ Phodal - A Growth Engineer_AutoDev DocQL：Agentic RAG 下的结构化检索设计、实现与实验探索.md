---
title: AutoDev DocQL：Agentic RAG 下的结构化检索设计、实现与实验探索
url: http://www.phodal.com/blog/autodev-docql/
source: Blog | Phodal - A Growth Engineer
date: 2025-11-27
fetch_date: 2025-11-28T03:15:15.233629
---

# AutoDev DocQL：Agentic RAG 下的结构化检索设计、实现与实验探索

## Blog

*search*
Search

span n>

[Home](/)[Blog](/blog/)[Code](/works)[Literature](/literature/)[More](/phodal/)

## Blog

PHODAL

[Home](/)[Blog](/blog/)[Literature](/literature/)[《自己动手设计物联网》](/literature/design-iot/)[《全栈应用开发：精益实践》](/literature/thinking-in-full-stack/)[《前端架构：从入门到微前端》](/literature/frontend-architecture-from-basic-to-microservices/)

## AutoDev DocQL：Agentic RAG 下的结构化检索设计、实现与实验探索

Posted by:
[Phodal Huang](/blog/author/root/)
Nov. 27, 2025, 11:31 p.m.

AutoDev Knowledge Agent 的想法是来源于一次与客户的交流中，说到的 RAG 相关的问题：结构化数据优于非结构化数据，JSON 还可以采用
JSONPath 来使用。

![](images/docql-overview.jpg)

既然，我们在 AutoDev 多端版本中已经构建了多个 Agents，那么再构建一个新的 Agent 难度自然也是不大的，这也就是 AutoDev
Knowledge 的想法，试验用于复杂场景下的知识问答。

## 为什么是 Agentic RAG 下的文档查询

> Agentic RAG 是一种让 LLM 智能体作为主动参与者（Agentic），以可控、多步骤、可反思的方式 使用检索增强生成（RAG）的新范式。

在 Agent 流行的今天，Agentic RAG 并不是一种新的技术， 我们在 Cursor、Claude Code、Antigravity 等中看到的 Agent 在接受用户的输入之后，
做了一系列 read file、grep、codebase 等操作本身，就是 Agentic RAG。

### 层级化查询：像人类一样查找资料

RAG 通常被分为两个阶段：**构建索引** 和 **执行查询**。因此，**查询方式往往会反过来决定我们的索引方式**
——是按段落切分、按章节组织，还是按代码结构解析，都取决于我们希望如何“查”。

在早先设计 Knowledge Agent 的时候，我预期的是一种**像工程师一样查找文档的体验**，而不是传统静态 RAG 那种 “输入一个问题 →
黑箱返回 context” 的单轮检索。

> 一个工程师在找文档时，从来不会只“搜一次”，而是会不断缩小范围、跨文档跳转、比对上下文、确认细节，并根据结果随时调整查询策略。

而在真实的软件工程环境中，这种“人类式查找”往往天然被分成两种路径：代码查询和文档查询。

#### 代码查询

代码里查找信息时，会自然形成一个多层级路径：

1. 找到相关模块或包。例如：user-service、auth、core/domain。
2. 再定位到类或接口。例如：UserController、AuthService、TokenProvider。
3. 再进入具体方法。例如：createUser()、validateToken()。
4. 再分析调用链与依赖。例如：HTTP handler → service → repository → external call。
5. 必要时反查测试、配置、注释

#### 文档查询

查文档时同样遵循层级式缩小范围：

1. 先确认文档类型或版本。例如 Installation / Architecture / Components / API Reference。
2. 再进入相关章节或目录。例如：Authentication、Deployment Guide、Module Design。
3. 再定位到小节或标题。例如：JWT Flow、Config Options、Error Handling。
4. 再阅读具体段落或示例
5. 必要时跳转到 FAQ、设计图、补充文档

所以，当我们的 context 是变成 RAG 化之后，这个问题就变成了：如何把文档构建成一个类似于 codebase 的 RAG 方式。

### Markdown 文档数据层级化索引

在转换为 Markdown 之后，我们就可以通过一级标题、二级标题、三级标题来反复在文件内跳转了。（事实上，在我觉得直接 readline 更简单）

![](images/query-file-example.png)

保持语义的丰富性是文档处理中的关键问题。实践中，将文档先转换为 HTML，再生成 Markdown 是一种比较理想的方式；相比直接从 Word
等格式生成
Markdown，这种流程更容易保留层级结构与格式信息，同时减少 LLM 的额外转换负担：

> 比如 Uber 的 [EAg-RAG](https://www.uber.com/en-HK/blog/enhanced-agentic-rag/) 将知识源迁移到 Google Docs（HTML 格式）。
> HTML 标签天然提供层级结构，再转换为 markdown，使解析精度大幅提升。即便使用 LangChain 的 Markdownify 将 Google 文档转换为
> Markdown，我们仍发现表格和嵌套结构处理存在改进空间，需要 LLM 将表格重写为 Markdown，并生成自然语言摘要，以保证语义和上下文完整性。

当然，资源受限的情况下，直接都转换成 markdown 就会比较简单了。而在 Codebase 的场景下，所有的文档都是 markdown 那么就在这问题上的挑战
会比较小。再根据实现的使用情况，我们需要适当地添加一些 metadata，比如 filename，timestamp 等，方便 LLM 做排序。

另外，多模态依旧是现在处理文档的一个痛点，尽管可能还有几年，所以在这里我们就暂时没有处理了 —— 正经的代码文档不都是
mermaid，plantuml 么。

## AutoDev DocQL 的实现

在 AutoDev Knowledge 的试验中，我们设计了 **DocQL (Document Query Language)** —— 一种面向文档和代码的领域特定语言（DSL）。
它旨在弥合“非结构化文本”与“结构化查询”之间的鸿沟，为 Agent 提供一种确定性的、可编程的检索接口。

我们主要从以下几个维度构建了 DocQL：

### 1. AI 友好的查询语法：为什么是 JSONPath？

LLM 在处理 JSON 结构时表现出了惊人的稳定性。因此，我们选择了一种 **类 JSONPath (JSONPath-like)** 的语法风格。这不仅降低了
Agent 学习工具使用的门槛（Few-shot 即可掌握），更重要的是它天然支持**属性访问**和**过滤条件**。

在 DocQL 中，一切皆对象。无论是目录（TOC）、实体（Entities）还是正文（Content），都可以通过 `$` 根节点进行访问：

* **$.toc**: 访问文档的目录树。
* **$.entities**: 访问提取出的关键实体（如类、方法、API 定义）。
* **$.content**: 访问具体的文档内容块。
* **$.code**: 专门用于访问源代码结构（基于 AST）。

这种设计让 Agent 可以像操作数据库一样操作文档：

```
// 场景 1：Agent 想要了解系统的整体架构，它不需要读取全文，只需查看一级目录
$.toc[? (@.level == 1)]

// 场景 2：Agent 需要查找所有涉及 "User" 的 API 定义
$.entities[? (@.type == "API" && @.name~ = "User")]

// 场景 3：Agent 想要阅读 "认证" 相关的章节内容
$.content.heading("Authentication")

// 场景 4：Agent 需要查看某个具体类的源码实现
$.code.class("AuthService")
```

### 2. 统一的文档对象模型（DOM for RAG）

为了让 DocQL 能够同时查询 Markdown 文档、PDF 甚至源代码文件，我们在底层构建了一个统一的文档对象模型。

* **对于 Markdown/PDF**：我们编写了 `MarkdownDocumentParser` 和 `TikaDocumentParser`。它们不仅提取文本，更重要的是提取**结构**。解析器会将文档转化为一颗包含层级关系的树（Document Tree），每个节点（TOCItem）都准确记录了行号、锚点和父子关系。
* **对于 Codebase**：代码本质上是一种高度结构化的文档。通过集成 **Tree-Sitter**，`CodeDocumentParser` 将 Java/Kotlin/Python
  等代码文件解析为同样的文档树。一个 `Class` 被视为一个高层级章节，而 `Function` 则是子章节。

这使得 Agent 无需关心底层文件格式。对 Agent 而言，查询《架构设计文档》中的“部署章节”与查询 `DeploymentService.java` 中的
`deploy` 方法，使用的是同一套逻辑。

### 3. Agentic 的核心：智能搜索与多级扩展

虽然 DocQL 提供了精确查询的能力，但用户（或 Agent）的输入往往是模糊的。为了解决这个问题，我们在 `DocQLTool` 中实现了一套 Smart Search（智能搜索）\*\* 机制。

当 Agent 发起一个模糊查询（如 "Auth"）时，系统并不会简单地做字符串匹配，而是执行一个 Agentic 的检索流：

1. **多级关键词扩展 (Multi-Level Keyword Expansion)**：
   * **Level 1 (Primary)**：生成精确变体（如 "Auth", "Authentication"）。
   * **Level 2 (Secondary)**：拆解组件词（如将 "AuthService" 拆解为 "Service"）。
   * **Level 3 (Tertiary)**：生成词干和同义词（如 "authorize", "access"）。
   * 系统会根据召回结果的数量，自适应地决定是“继续扩展”还是“收缩过滤”。
2. **多通道并行检索**：
   查询请求会被分发到不同的通道并行执行：
   * `$.code.classes`：查找类名匹配。
   * `$.code.functions`：查找函数名匹配。
   * `$.content.heading`：查找文档标题匹配。
   * `$.entities`：查找术语定义。
3. **RRF 融合与重排序 (Reranking)**：
   既然是 Agentic RAG，排序逻辑就不能只看词频（BM25）。我们引入了 **Composite Scorer**，结合了以下权重：
   * **Type Priority**：代码实体（类/函数）优于普通文本块。
   * **Name Match**：标题/名称匹配优于内容匹配。
   * **RRF (Reciprocal Rank Fusion)**：融合多路召回的结果。

### 4. 结果的结构化与源溯源

传统的 RAG 往往返回一段拼接后的文本（Context Chunk），丢失了文件的上下文信息。而 DocQL 的执行结果是高度结构化的，并且**按源文件分组**。

```
{
  "type": "docql_result",
  "files": {
    "docs/architecture.md": [
      {
        "type": "heading",
        "text": "System Architecture",
        "line": 10,
        "content": "..."
      }
    ],
    "src/main/AuthService.kt": [
      {
        "type": "class",
        "name": "AuthService",
        "line": 25,
        "source": "..."
      }
    ]
  }
}
```

这种结构化返回让 Agent 能够清晰地知道每一段知识来自哪个文件的哪个位置。在生成回答时，Agent 可以准确地引用来源（例如：“根据
`architecture.md` 的第 10 行...”），极大地缓解了幻觉问题。

## 试验结果 && 总结

![](images/autodev-knowledge-failured.png)

毫无意外，我们在 DeepSeek 上的试验结果并没有那么理想，没有达到预期

* AI 在找到几个结果之后，并不会继续（应该是提示词的问题，也有可能是 DS 的 agentic 不理想）
* JSONPath 还是容易犯错，并不会出现我们预期中的多级查询的效果（可能我找的几个 case 比较复杂）

不过，我觉得等模型能力上去之后可能就不是问题了。

AutoDev DocQL 的试验表明，在面向研发的 RAG 场景中，**Query（查询）比 Search（搜索）更有效**。

* **Search** 是概率性的，适合寻找“可能相关”的信息。
* **Query** 是确定性的，适合在已知结构中“精确定位”信息。

通过 DocQL，我们将文档库和代码库转化为了一个可被 Agent 编程查询的“数据库”。Agent 不再是一个只能被动接收 Context
的阅读者，而变成了一个能够主动探索、按图索骥的**研究员**。它能够像资深工程师一样，先看目录，再查定义，最后读源码，完成复杂的知识问答任务。

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

* [AutoDev DocQL：Agentic RAG 下的结构化检索设计、实现与实验探索](/blog/autodev-docql/)
* [AI 代码审查再进化：AutoDev 多智能体协作架构深度解析](/blog/autodev-multi-agents-code-review/)
* [AutoDev Code Review Agent：应对 10 倍代码量的 Agentic 代码审查](/blog/autodev-review-agent/)
* [2025 AI 代码检视：以 ROI 为中心的 AI 代码检视体系与分级](/blog/ai-code-review-2025/)
* [AutoDev CLI：构建 AI Agent 生成的 AI Agent 质量保障与验证架构](/blog/autodev-cli-validate-framework/)
* [Vibe Coding 何必只在桌面 IDE，编码智能体协同的思考与设计](/blog/vibe-coding-for-autodev-server/)
* [多端编程 Agent（CLI/Desktop/Mobile）：AutoDev 架构升级，欢迎一起参与演进](/blog/autodev-next-poc/)
* [Agent 架构综述：从 Prompt 到上下文工程构建 AI Agent](/blog/prompt-to-agenticg/)
* [AutoDev A2A 新能力下的云端 Agent 路径思考，从扩展到协作](/blog/autodev-a2a/)
* [从 Langchain 到 Spring AI，我们究竟需要怎么样一个企业级 AI 开发框架？](/blog/from-langchain-to-spring-ai/)

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
* [...