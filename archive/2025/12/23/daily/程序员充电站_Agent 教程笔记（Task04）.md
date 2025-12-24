---
title: Agent 教程笔记（Task04）
url: https://itcharge.cn/tech/llm-dev/agent-tutorial-04/
source: 程序员充电站
date: 2025-12-23
fetch_date: 2025-12-24T03:27:00.881865
---

# Agent 教程笔记（Task04）

# [![](https://itcharge.cn/wp-content/uploads/header-logo.png)](https://itcharge.cn/)

[Menu](#0)

* [技术](https://itcharge.cn/category/tech/)
  + [iOS 开发](https://itcharge.cn/category/tech/ios-dev/)
  + [算法 & 数据结构](https://itcharge.cn/category/tech/algorithm/)
  + [Hexo 博客](https://itcharge.cn/category/tech/hexo-blog/)
  + [WordPress 博客](https://itcharge.cn/category/tech/wordpress-blog/)
* [高效率](https://itcharge.cn/category/efficient/)
  + [macOS 技巧](https://itcharge.cn/category/efficient/macos-tip/)
  + [工具思维](https://itcharge.cn/category/efficient/tool-mind/)
* [慢生活](https://itcharge.cn/category/life/)
  + [读书笔记](https://itcharge.cn/category/life/book-notes/)
  + [学习成长](https://itcharge.cn/category/life/learning/)
  + [厨艺美食](https://itcharge.cn/category/life/cooking/)
  + [诗意人生](https://itcharge.cn/category/life/brilliant-life/)
  + [考研相关](https://itcharge.cn/category/life/graduate-test/)
* [关于我](https://itcharge.cn/life/about-me/)

# Agent 教程笔记（Task04）

[2025-12-24](https://itcharge.cn/tech/llm-dev/agent-tutorial-04/)

3.897k 字  |  13 分钟

## 5. 上下文工程

### 5.1 核心认知：从 Prompt 到 Context

* **提示工程 vs 上下文工程**

  + 提示工程：关注“怎么写好一段指令”。
  + 上下文工程：关注“这一次调用前，窗口里应该装哪些 token，怎样组织最好”。
  + 现在的智能体往往是**长时程、多轮、多工具**调用，仅优化单条 Prompt 远远不够，需要工程化管理整个上下文状态。
* **上下文是稀缺资源**

  + 模型有固定上下文窗口，同时存在 **注意力预算**：token 越多，平均分到每个 token 的注意力越少。
  + 现实现象：needle-in-a-haystack 基准显示，窗口变长后，模型从上下文中精确检索信息的能力会下降，出现 **context rot（上下文腐蚀）**。
  + 结论：即使 100K / 200K 窗口，也要把“哪些信息必须进窗口、以什么形式进”当成一门工程。
* **高质量上下文的目标**

  + 用**尽量少、信号密度尽量高**的 tokens，最大化得到正确行为的概率。
  + 关键构件：
  + System Prompt：角色、边界、风格、输出格式。
  + Tools / MCP：让模型“按需取数”，而不是“一股脑塞进去”。
  + Few-shot：用少量、典型、结构化示例直观“画出”期望行为。
  + 原则：**信息充分但紧致**，避免过度硬编码逻辑，也避免空洞口号。

### 5.2 长时程任务的三种关键手段

* **压缩整合（Compaction）**

  + 当对话逼近上下文上限时，对历史进行高保真总结，用摘要 + 最近若干关键片段重启新窗口。
  + 压缩优先“清理噪声”：重复工具输出、无关闲聊；保留决策、未解决问题、关键中间结果。
  + 调参思路：先保证召回（不丢关键信息），再逐步提高精确度（删冗余）。
* **结构化笔记 / 外部记忆（Note + Memory）**

  + 把跨会话、跨窗口要长期保留的信息写到**上下文外**：文件、向量库、图数据库等。
  + 适合记录：项目进度、重要结论、TODO / 阻塞、长期偏好等。
  + 好处：
  + 对上下文窗口几乎零占用；
  + 人类可读、可审计、可版本控制；
  + 和 RAG / MemoryTool 结合，可以在新任务前检索“相关经验”注入 Prompt。
* **子代理架构（Sub-agents）**

  + 主代理负责规划与综合，子代理各自拿一块“干净窗口”+ 专属工具做深入探索，然后只回传**压缩摘要**。
  + 适合：大规模调研、代码库多子模块并行分析等。
  + 优点：分而治之，主代理保持上下文干净，不被局部细节淹没。

### 5.3 Hello-Agents 中的实践：ContextBuilder（GSSC）

#### 5.3.1 设计目标

* **统一入口**：把 Gather-Select-Structure-Compress 四步封装进 `ContextBuilder.build()`，Agent 不再手写杂乱的拼 Prompt 代码。
* **稳定形态**：输出固定骨架（[Role & Policies] / [Task] / [Evidence] / [Context] / [Output]），便于调试和 A/B 测试。
* **预算守护**：显式管理 token 预算（`max_tokens` + `reserve_ratio`），避免系统指令被“挤爆”。
* **最小规则**：评分只看 **相关性 + 新近性**，不引入复杂优先级层级，先把 80% 场景做稳。

#### 5.3.2 核心数据结构

* **`ContextPacket`：信息原子单元**

  + 字段：`content`、`timestamp`、`token_count`、`relevance_score`、`metadata`。
  + 所有候选信息统一包成 Packet，方便后面统一打分、过滤、分组。
* **`ContextConfig`：构建策略配置**

  + 关键参数：
  + `max_tokens`: 上下文总预算。
  + `reserve_ratio`: 为系统指令等保留百分比。
  + `min_relevance`: 忽略低于阈值的信息。
  + `recency_weight` + `relevance_weight`: 相关性 vs 时间新近性的权重（要求和为 1）。

#### 5.3.3 GSSC 四阶段

* **Gather：多源汇集**

  + 来源：System Prompt、MemoryTool 检索结果、RAG 检索结果、最近对话历史、自定义 packets。
  + 特点：对单一来源失败要容错（try/except），系统指令标记为高优先级类型。
* **Select：打分筛选**

  + 先把系统指令单独拿出，优先“占坑”。
  + 剩余候选按综合评分排序：
  + `score = relevance_weight * relevance + recency_weight * recency`。
  + 相关性可从简单词重叠起步，工程上可换成向量相似度。
  + 然后按分数从高到低贪心装填，直到 token 上限；过滤 `relevance_score < min_relevance`。
* **Structure：结构化模板**

  + 按 `metadata.type` 分到不同区块：`system_instruction` → [Role & Policies]，`rag_result/knowledge` → [Evidence]，其余 → [Context]。
  + 固定模板：
  + [Role & Policies]：系统指令 / 角色与约束；
  + [Task]：本轮用户问题；
  + [Evidence]：检索到的知识；
  + [Context]：对话历史、记忆、笔记等；
  + [Output]：输出要求。
  + 优点：人类和模型都易读，方便诊断“哪一层出了问题”。
* **Compress：兜底压缩**

  + 若整体 token 超限，则按分区逐段压缩，优先保证结构不被打乱。
  + 简化实现：按分段截断 + 标注“内容已压缩”；真实生产可以接 LLM 摘要。
  + 核心原则：**宁可删细节，也要保框架；宁可删重复，也要保决策链路**。

#### 5.3.4 与 Agent 的集成模式

* Agent 的 `run()` 流程可以统一成：
  1. 收集对话历史 + 系统指令 + 自定义上下文（如项目配置、RAG 结果、笔记）；
  2. 调用 `ContextBuilder.build(...)` 得到结构化上下文字符串；
  3. 把该字符串作为 system 或单轮 prompt，交给 LLM；
  4. 更新对话历史 & 记忆 & 笔记。
* 好处：所有 Agent 共享同一套上下文治理策略，便于横向调优和监控。

### 5.4 NoteTool：结构化长期笔记

* **定位**：介于“对话记忆（MemoryTool）”和“外部文档”之间的**轻量项目式记忆**。

  + 载体：Markdown + YAML Front Matter。
  + 元数据（YAML）：`id/title/type/tags/created_at/updated_at/file_path`。
  + 正文（MD）：状态、结论、阻塞、行动项等，可直接被人类查看和编辑。
* **核心操作**

  + `create/read/update/delete`：完整的笔记生命周期管理。
  + `search`：按关键词 + 类型 + 标签过滤检索，支持按更新时间排序。
  + `list`：浏览某类型/某标签下的近期笔记。
  + `summary`：统计总数、类型分布、最近更新时间等，用于全局把握项目状态。
* **典型用法**

  + 长期项目：以 `task_state/conclusion/blocker/action/reference` 类型管理项目进展。
  + 与 ContextBuilder 协同：在每轮构建上下文前按语义检索笔记，把少量高价值笔记转成 `ContextPacket` 注入 [Context]/[Evidence]。
  + 与人协作：工程师可直接在仓库中编辑这些 `.md` 文件，Agent 与人共享同一信息源。

### 5.5 TerminalTool：即时文件系统上下文

* **目标**：让 Agent 通过“受控 shell”即时探索代码库 / 日志 / 数据文件，而不是预先索引一切。

  + 典型命令：`ls/tree/cat/head/tail/grep/find/wc/sort/uniq/cut/...`，全部只读。
  + 适用场景：
  + 代码库理解：看结构、查类/函数定义、搜 TODO。
  + 日志分析：tail + grep 错误、统计错误类型。
  + 数据预览：head CSV、wc 行数、抽取列名。
* **安全设计**

  + 命令白名单：禁止 `rm/mv/echo >` 等写操作，只允许只读工具。
  + 工作目录沙箱：所有命令限制在 `workspace` 及其子目录，阻止路径逃逸（`..` 检查）。
  + 超时 + 输出大小限制：防止死循环、超大输出拖垮系统。
  + 特殊处理 `cd`：更新内部 `current_dir`，配合后续命令连续导航。
* **与上下文工程的关系**

  + 将“文件全量塞上下文”改为“按需用命令拉取片段”，是典型的 **JIT 上下文**。
  + Terminal 输出再通过 `ContextPacket` 注入构建器，可作为 [Evidence] 或 [Context] 的一部分。
  + 与 NoteTool/MemoryTool 结合：把关键发现固化为笔记或记忆，供后续检索，而非每次重新 grep。

### 5.6 综合案例：代码库维护助手的上下文策略

* **三层记忆 / 上下文分工**

  + 即时层：`TerminalTool` 探索当前代码、日志、数据。
  + 会话层：`conversation_history` + `MemoryTool` 记录本轮任务过程中的关键对话与执行轨迹。
  + 持久层：`NoteTool` 存项目级状态、长期任务、重构计划与阻塞。
* **典型一次 `run()` 的上下文构建流程**

  1. 根据模式（explore/analyze/plan/auto）可选调用 Terminal / 笔记列表，先拉一批“前情”形成 `ContextPacket`。
  2. 用 NoteTool 检索与当前 query 相关的 blockers / actions / task\_state。
  3. 将上述信息 + 最近对话一起交给 `ContextBuilder`，得到结构化上下文。
  4. 调用 LLM 得到回答，并按规则决定是否自动写入新的 blocker/action 笔记。
  5. 更新统计信息与历史，必要时生成阶段性报告。
* **从上下文工程角度看该案例的要点**

  + 不追求“把代码库全塞进上下文”，而是通过工具 + 笔记 + 记忆进行 **分层、按需、可追溯** 的信息管理。
  + 每一次调用前，系统性地回答两个问题：
    1. 这次推理真正需要哪些信息？
    2. 哪些信息可以放在窗口外，通过工具或检索随用随取？
  + GSSC + NoteTool + TerminalTool 正是围绕这两个问题给出的工程解。

## 参考资料

* [Hello-Agents - 《从零开始构建智能体》](https://datawhalechina.github.io/hello-agents/#/)

赏

* **本文作者：**
  程序员充电站
* **本文链接：**
  https://itcharge.cn/tech/llm-dev/agent-tutorial-04/
* **版权声明：**
  本站所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。

* [大模型应用开发](https://itcharge.cn/category/tech/llm-dev/)
* [技术](https://itcharge.cn/category/tech/)

# 评论（没有评论）

![](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/head.jpg)

# [程序员充电站](/)

高效率编程，慢节奏生活。

[79
文 章](/archive)

[18
分 类](/category)

[21
标 签](/tag)

# 最新文章

* [2024 年目标清单](https://itcharge.cn/life/learning/2024-flags/)
* [Agent 教程笔记（Task04）](https://itcharge.cn/tech/llm-dev/agent-tutorial-04/)
* [Agent 教程笔记（Task03）](https://itcharge.cn/tech/llm-dev/agent-tutorial-03/)
* [Agent 教程笔记（Task02）](https://itcharge.cn/tech/llm-dev/agent-tutorial-02/)
* [RAG 教程笔记（Task02）](https://itcharge.cn/tech/llm-dev/rag-tutorial-02/)

# 热门标签

* [技术](https://itcharge.cn/tag/%E6%8A%80%E6%9C%AF/)
* [iOS 开发](https://itcharge.cn/tag/ios-dev/)
* [北航考研](https://itcharge.cn/tag/%E5%8C%97%E8%88%AA%E8%80%83%E7%A0%94/)
* [生活](https://itcharge.cn/tag/life/)
* [数据结构](https://itcharge.cn/tag/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/)
* [C 语言](https://itcharge.cn/tag/c-%E8%AF%AD%E8%A8%80/)
* [学习成长](https://itcharge.cn/tag/learning/)
* [高效率](https://itcharge.cn/tag/%E9%AB%98%E6%95%88%E7%8E%87/)
* [macOS 技巧](https://itcharge.cn/tag/macos-%E6%8A%80%E5%B7%A7/)
* [读书笔记](https://itcharge.cn/tag/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/)
* [考研](https://itcharge.cn/tag/%E8%80%83%E7%A0%94/)
* [工具思维](https://itcharge.cn/tag/%E5%B7%A5%E5%85%B7%E6%80%9D%E7%BB%B4/)
* [慢生活](https://itcharge.cn/tag/%E6%85%A2%E7%94%9F%E6%B4%BB/)
* [Hexo 博客](https://itcharge.cn/tag/hexo-%E5%8D%9A%E5%AE%A2/)
* [WordPress 博客](https://itcharge.cn/tag/wordpress-%E5%8D%9A%E5%AE%A2/)
* [诗意人生](https://itcharge.cn/tag/romantic-life/)
* [厨艺美食](https://itcharge.cn/tag/cooking/)
* [LeetCode](https://itcharge.cn/t...