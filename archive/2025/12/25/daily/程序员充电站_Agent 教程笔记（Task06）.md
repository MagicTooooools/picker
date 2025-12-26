---
title: Agent 教程笔记（Task06）
url: https://itcharge.cn/tech/llm-dev/agent-tutorial-06/
source: 程序员充电站
date: 2025-12-25
fetch_date: 2025-12-26T03:27:03.500747
---

# Agent 教程笔记（Task06）

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

# Agent 教程笔记（Task06）

[2025-12-25](https://itcharge.cn/tech/llm-dev/agent-tutorial-06/)

1.459k 字  |  5 分钟

## 7. 自动化深度研究智能体系统

---

### 7.1 TODO 驱动的研究范式

**核心思想**：将复杂研究主题分解为可执行的子任务，通过"规划→执行→整合"流程完成。

**三阶段流程**：

* **规划阶段**：将研究主题分解为 3-5 个子任务，每个包含 title、intent、query
* **执行阶段**：对每个子任务执行搜索和总结，生成结构化知识
* **报告阶段**：整合所有子任务总结，生成最终研究报告

**优势**：可控性强、质量可靠、易于调试、可扩展性好。

### 7.2 三 Agent 协作系统

**Agent 职责划分**：

* **TODO Planner（研究规划专家）**：将研究主题分解为子任务，输出 JSON 格式
* **Task Summarizer（任务总结专家）**：总结搜索结果，提取关键信息，添加来源引用
* **Report Writer（报告撰写专家）**：整合所有子任务总结，生成结构化 Markdown 报告

**协作模式**：顺序协作，线性流程，每个 Agent 的输入来自上一个 Agent 的输出。

**设计要点**：

* 每个 Agent 专注单一职责
* 为每个 Agent 定制专门的 Prompt
* 通过清晰的接口协作，易于维护和扩展

### 7.3 ToolAwareSimpleAgent 扩展

**设计动机**：`SimpleAgent`不支持工具调用监听，需要扩展以记录工具调用情况。

**核心功能**：

* 通过`tool_call_listener`回调函数监听每次工具调用
* 记录 Agent 名称、工具名称、参数、结果等信息
* 用于调试、日志、进度展示等场景

**实现方式**：继承`SimpleAgent`，重写`_execute_tool_call`方法，在执行工具后通知监听器。

### 7.4 工具系统集成

**SearchTool 扩展**：

* 支持多种搜索引擎：Tavily、DuckDuckGo、Perplexity、SearXNG 等
* 提供统一的搜索接口，通过配置选择搜索引擎
* 实现结果去重、Token 限制、错误降级等处理

**NoteTool 使用**：

* 持久化研究进度，每个子任务的总结保存为 Markdown 笔记
* 支持研究中断后恢复，方便审计和分析
* 笔记结构：任务信息、搜索结果、总结、来源引用

**ToolRegistry 管理**：

* 统一管理所有工具的注册和调用
* Agent 通过工具调用指令使用工具
* 支持工具的动态注册和扩展

### 7.5 服务层实现

**四个核心服务**：

* **PlanningService**：调用规划 Agent，解析 JSON，验证子任务格式
* **SummarizationService**：调用总结 Agent，格式化搜索结果，提取来源引用
* **ReportingService**：调用报告 Agent，整合总结，生成最终报告
* **SearchService**：调度搜索引擎，处理结果（去重、Token 限制），支持缓存

**设计原则**：服务层连接 Agent 和工具，各司其职，通过清晰接口协作。

### 7.6 前端交互设计

**全屏模态对话框 UI**：

* 沉浸式体验，清晰的层次结构
* 包含顶部栏、进度区域、内容区域、底部栏
* 响应式设计，适配不同屏幕尺寸

**SSE 实时进度展示**：

* 使用 Server-Sent Events 实现服务器主动推送
* 实时展示研究进度（规划、执行、报告）
* 推送任务列表、任务总结、最终报告等事件

**Markdown 结果可视化**：

* 使用`marked`库将 Markdown 转换为 HTML
* 自定义样式，美观展示研究报告
* 特殊处理来源引用，便于查看和验证

### 7.7 关键要点总结

* **研究范式**：TODO 驱动，三阶段流程，系统化研究
* **Agent 设计**：职责清晰，Prompt 优化，易于维护
* **工具集成**：SearchTool 多引擎支持，NoteTool 持久化，ToolRegistry 统一管理
* **服务架构**：四层服务，连接 Agent 和工具，清晰接口
* **用户体验**：全屏模态、SSE 实时进度、Markdown 可视化

赏

* **本文作者：**
  程序员充电站
* **本文链接：**
  https://itcharge.cn/tech/llm-dev/agent-tutorial-06/
* **版权声明：**
  本站所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。

* [大模型应用开发](https://itcharge.cn/category/tech/llm-dev/)
* [技术](https://itcharge.cn/category/tech/)

# 评论（没有评论）

![](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/head.jpg)

# [程序员充电站](/)

高效率编程，慢节奏生活。

[83
文 章](/archive)

[18
分 类](/category)

[21
标 签](/tag)

# 最新文章

* [2024 年目标清单](https://itcharge.cn/life/learning/2024-flags/)
* [Agent 教程笔记（Task06）](https://itcharge.cn/tech/llm-dev/agent-tutorial-06/)
* [Agent 教程笔记（Task05）](https://itcharge.cn/tech/llm-dev/agent-tutorial-05/)
* [RAG 教程笔记（Task04）](https://itcharge.cn/tech/llm-dev/rag-tutorial-04/)
* [RAG 教程笔记（Task03）](https://itcharge.cn/tech/llm-dev/rag-tutorial-03/)

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