---
title: Agent 教程笔记（Task03）
url: https://itcharge.cn/tech/llm-dev/agent-tutorial-03/
source: 程序员充电站
date: 2025-12-21
fetch_date: 2025-12-22T03:32:15.432574
---

# Agent 教程笔记（Task03）

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

# Agent 教程笔记（Task03）

[2025-12-21](https://itcharge.cn/tech/llm-dev/agent-tutorial-03/)

2.004k 字  |  7 分钟

## 4. 记忆与检索

### 4.1 为什么需要记忆与 RAG

**LLM 的两大局限**：

1. **无状态导致对话遗忘**：每次请求都是独立的，无法记住之前的对话内容
2. **内置知识局限性**：训练数据有时间截止点，无法获取最新信息，专业领域知识不足

**解决方案**：

* **记忆系统**：让智能体能够记住对话历史、用户偏好和学习经验
* **RAG 系统**：从外部知识库检索相关信息，增强模型生成能力

### 4.2 记忆系统：四层架构

借鉴人类记忆系统，HelloAgents 设计了四种记忆类型：

**（1）工作记忆（Working Memory）**

* **特点**：临时信息，容量有限（默认 50 条），TTL 自动清理
* **存储**：纯内存，访问速度快
* **应用**：存储当前对话的上下文信息
* **评分公式**：`(相似度 × 时间衰减) × (0.8 + 重要性 × 0.4)`

**（2）情景记忆（Episodic Memory）**

* **特点**：具体事件和经历，支持时间序列检索
* **存储**：SQLite + Qdrant 混合存储
* **应用**：记录学习事件、查询历史、重要里程碑
* **评分公式**：`(向量相似度 × 0.8 + 时间近因性 × 0.2) × (0.8 + 重要性 × 0.4)`

**（3）语义记忆（Semantic Memory）**

* **特点**：抽象知识和概念，支持知识图谱推理
* **存储**：Neo4j 图数据库 + Qdrant 向量数据库
* **应用**：存储用户偏好、领域知识、概念理解
* **评分公式**：`(向量相似度 × 0.7 + 图相似度 × 0.3) × (0.8 + 重要性 × 0.4)`

**（4）感知记忆（Perceptual Memory）**

* **特点**：多模态数据（文本、图像、音频），支持跨模态检索
* **存储**：按模态分离的向量存储
* **应用**：处理文档特征、图像理解、音频转录

### 4.3 记忆系统的核心操作

**MemoryTool 提供的操作**：

* `add`：添加记忆（支持 4 种类型）
* `search`：搜索记忆（语义检索 + 关键词匹配）
* `forget`：遗忘记忆（基于重要性/时间/容量）
* `consolidate`：整合记忆（短期 → 长期）
* `summary`：获取记忆摘要
* `stats`：获取统计信息

**记忆生命周期**：

```
编码 → 存储 → 检索 → 整合 → 遗忘
```

### 4.4 RAG 系统：知识检索增强

**RAG 核心思想**：在生成回答前，先从外部知识库检索相关信息，作为上下文提供给模型

**工作流程**：

1. **数据处理阶段**：文档提取 → MarkItDown 转换 → 智能分块 → 向量化 → 存储
2. **查询阶段**：用户提问 → 检索相关文档 → 注入 Prompt → LLM 生成答案

**MarkItDown 统一转换**：

* 支持格式：PDF、Word、Excel、图片、音频等
* 统一输出：Markdown 格式
* 优势：结构清晰，便于后续处理

**智能分块策略**：

* 基于 Markdown 标题层次（#、##、###）进行语义分割
* 根据 Token 数量控制分块大小
* 支持重叠策略保持上下文连续性

### 4.5 高级检索策略

**（1）多查询扩展（MQE）**

* **原理**：生成语义等价的多样化查询
* **优势**：提升召回率 30%-50%，避免用词差异导致遗漏
* **示例**："如何学习 Python" → ["Python 入门教程", "Python 学习方法", "Python 编程指南"]

**（2）假设文档嵌入（HyDE）**

* **原理**：用答案找答案，先生成假设答案段落，再用其检索真实文档
* **优势**：缩小查询和文档的语义鸿沟，提升检索精度
* **适用**：专业领域查询效果显著

**（3）统一扩展检索框架**

* 整合 MQE 和 HyDE 两种策略
* 通过"扩展-检索-合并"三步流程
* 支持灵活配置，根据场景选择策略

### 4.6 实际应用：智能文档问答助手

**案例功能**：

1. **智能文档处理**：PDF → Markdown → 分块 → 向量化
2. **高级检索问答**：MQE + HyDE 提升检索质量
3. **多层次记忆管理**：工作记忆（当前任务）、情景记忆（学习事件）、语义记忆（概念知识）
4. **个性化学习支持**：基于学习历史的推荐、学习报告生成

**核心实现**：

```
# 加载文档
rag_tool.execute("add_document", file_path=pdf_path)

# 智能问答
rag_tool.execute("ask", question=question, enable_mqe=True, enable_hyde=True)

# 记忆管理
memory_tool.execute("add", content=content, memory_type="semantic")
memory_tool.execute("search", query=query, limit=5)
```

### 4.7 设计要点总结

**记忆系统设计**：

* 分层架构：工作记忆（临时）→ 情景记忆（事件）→ 语义记忆（知识）
* 评分机制：综合考虑相似度、时间衰减、重要性权重
* 生命周期管理：自动整合、选择性遗忘、容量控制

**RAG 系统设计**：

* 统一转换：MarkItDown 处理多格式文档
* 智能分块：基于 Markdown 结构的语义分割
* 高级检索：MQE + HyDE 提升召回率和精度
* 模块化设计：各层可独立优化和替换

**核心优势**：

* **记忆系统**：让智能体具备类人的记忆能力，支持个性化服务
* **RAG 系统**：突破模型知识局限，支持最新信息和专业领域知识
* **组合使用**：RAG 检索外部知识，Memory 记录学习经验，形成完整闭环

## 参考资料

* [Hello-Agents - 《从零开始构建智能体》](https://datawhalechina.github.io/hello-agents/#/)

赏

* **本文作者：**
  程序员充电站
* **本文链接：**
  https://itcharge.cn/tech/llm-dev/agent-tutorial-03/
* **版权声明：**
  本站所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。

* [大模型应用开发](https://itcharge.cn/category/tech/llm-dev/)
* [技术](https://itcharge.cn/category/tech/)

# 评论（没有评论）

![](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/head.jpg)

# [程序员充电站](/)

高效率编程，慢节奏生活。

[78
文 章](/archive)

[18
分 类](/category)

[21
标 签](/tag)

# 最新文章

* [2024 年目标清单](https://itcharge.cn/life/learning/2024-flags/)
* [Agent 教程笔记（Task03）](https://itcharge.cn/tech/llm-dev/agent-tutorial-03/)
* [Agent 教程笔记（Task02）](https://itcharge.cn/tech/llm-dev/agent-tutorial-02/)
* [RAG 教程笔记（Task02）](https://itcharge.cn/tech/llm-dev/rag-tutorial-02/)
* [Agent 教程笔记（Task01）](https://itcharge.cn/tech/llm-dev/agent-tutorial/)

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
* [LeetCode](https://itcharge.cn/tag/leetcode/)
* [算法](https://itcharge.cn/tag/%E7%AE%97%E6%B3%95/)
* [读书随笔](https://itcharge.cn/tag/book-notes/)
* [算法 & 数据结构](https://itcharge.cn/tag/%E7%AE%97%E6%B3%95-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/)

# 联系方式

# 目 录

谢谢你请我喝咖啡~

![扫码支持](https://itcharge.cn/wp-content/uploads/wxpay.png "扫一扫")
![扫码支持](https://itcharge.cn/wp-content/uploads/alipay.png "扫一扫")

扫码打赏，支持一下

![微信](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/praise/wechat_pay.svg)
微信支付

![支付宝](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/praise/ali_pay.svg)
支付宝

打开微信扫一扫，即可进行扫码打赏哦

本站总访问量  次
|
本站总访客数  人

©
2016-2025 程序员充电站 | [京ICP备18044298号-2](https://beian.miit.gov.cn/) | Theme [Sapphire](https://itcharge.cn/wordpress-theme-sapphire/) by ITCharge