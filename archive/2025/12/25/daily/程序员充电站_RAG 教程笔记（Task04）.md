---
title: RAG 教程笔记（Task04）
url: https://itcharge.cn/tech/llm-dev/rag-tutorial-04/
source: 程序员充电站
date: 2025-12-25
fetch_date: 2025-12-26T03:27:07.404808
---

# RAG 教程笔记（Task04）

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

# RAG 教程笔记（Task04）

[2025-12-25](https://itcharge.cn/tech/llm-dev/rag-tutorial-04/)

3.543k 字  |  12 分钟

## 6. 检索优化

### 6.1 混合检索

**混合检索（Hybrid Search）**：结合稀疏向量和密集向量的优势，同时利用关键词精确匹配和语义理解能力，提升检索准确性和召回率。

#### 稀疏向量 vs 密集向量

* **稀疏向量（词法向量）**：

  + 基于词频统计（如TF-IDF、BM25），维度高但大部分元素为零
  + 优点：可解释性强（每个维度对应具体词）、无需训练、关键词匹配精确
  + 缺点：无法理解语义（如"汽车"和"轿车"无法识别为同义词），存在词汇鸿沟
  + 表示方式：字典格式 `{索引: 权重}` 或坐标列表 `(维度, [索引], [值])`
* **密集向量（语义向量）**：

  + 通过深度学习模型（BERT、GPT等）学习到的低维稠密浮点数表示
  + 优点：理解同义词和上下文关系，泛化能力强，语义搜索效果好
  + 缺点：可解释性差，需要大量数据和算力训练，对未登录词（OOV）处理困难
  + 表示方式：数组格式 `[0.89, -0.12, 0.77, ...]`

#### 融合方法

1. **倒数排序融合（RRF）**：

   * 不关心原始得分，只关注排名
   * 公式：$RRF*{score}(d) = \sum*{i=1}^{k} \frac{1}{rank\_i(d) + c}$
   * 参数 `c` 通常设为60，用于降低排名靠后文档的权重
2. **加权线性组合**：

   * 先归一化不同检索系统的得分，再线性组合
   * 公式：$Hybrid*{score} = \alpha \cdot Dense*{score} + (1 - \alpha) \cdot Sparse\_{score}$
   * 通过调整 `α` 控制语义和关键词的贡献比例

#### Milvus实现要点

* **Schema定义**：同时定义 `SPARSE_FLOAT_VECTOR` 和 `FLOAT_VECTOR` 两个向量字段
* **向量生成**：使用BGE-M3模型同时生成稀疏和密集向量
* **索引创建**：稀疏向量使用 `SPARSE_INVERTED_INDEX`，密集向量使用 `AUTOINDEX`
* **混合搜索**：使用 `hybrid_search()` 方法，配合 `RRFRanker` 进行结果融合
* **优势**：召回率和准确率高，灵活适应不同场景，容错性好

### 6.2 查询构建

**查询构建（Query Construction）**：利用LLM将自然语言查询转换为结构化查询语言（如SQL、Cypher）或带过滤条件的请求，使RAG系统能够处理结构化、半结构化和图数据。

#### 文本到元数据过滤器

**自查询检索器（SelfQueryRetriever）**：将自然语言查询分解为查询字符串和元数据过滤器两部分。

**工作流程**：

1. **定义元数据结构**：使用 `AttributeInfo` 描述每个元数据字段的名称、类型和含义
2. **查询解析**：LLM将查询分解为：
   * **查询字符串**：用于语义搜索的部分
   * **元数据过滤器**：结构化过滤条件（如 `year == 2022`）
3. **执行查询**：向量数据库同时执行语义搜索和元数据过滤

**关键配置**：

* `document_contents`：描述文档内容
* `metadata_field_info`：元数据字段信息列表
* `temperature=0`：确保查询转换的稳定性和可复现性

**示例**：查询"2022年发布的机器学习论文" → 查询字符串："机器学习论文" + 过滤器：`year == 2022`

#### 文本到Cypher

**Cypher**：图数据库（如Neo4j）的查询语言，用于匹配图中的模式和关系。

**原理**：LLM根据图谱模式（Schema）将自然语言问题转换为Cypher查询语句，在图数据库执行后返回结构化数据。

**优势**：降低图数据查询门槛，用户可用自然语言与复杂图结构交互。

**应用场景**：知识图谱查询、关系网络分析、复杂结构化数据检索

### 6.3 文本到 SQL

**文本到SQL（Text-to-SQL）**：利用LLM将自然语言问题转换为可执行的SQL查询语句，打破人与结构化数据之间的语言障碍。

#### 业务挑战

* **"幻觉"问题**：LLM可能生成不存在的表或字段
* **数据库结构理解不足**：需要准确理解表结构、字段含义和关联关系
* **用户输入模糊性**：需要处理拼写错误和不规范表达

#### 优化策略

1. **提供精确的数据库模式**：向LLM提供表的DDL语句，包括表名、列名、数据类型和外键关系
2. **提供少量高质量示例**：在提示中加入"问题-SQL"示例对，提升生成准确性
3. **利用RAG增强上下文**：构建知识库包含：
   * 表和字段的详细描述
   * 同义词和业务术语映射
   * 复杂查询示例（JOIN、GROUP BY等）
4. **错误修正与反思**：执行失败时将错误信息反馈给LLM，迭代修正SQL语句

#### 实现框架核心模块

**知识库模块**：

* 统一存储DDL定义、Q-SQL示例和表描述三种知识类型
* 使用BGE-M3进行语义检索，支持中英文混合搜索

**SQL生成模块**：

* 基于知识库检索结果构建上下文
* 使用结构化提示确保SQL语法正确
* 设置temperature=0保证输出确定性
* 具备错误修复机制

**代理模块**：

* 协调知识库检索、SQL生成和执行的完整流程
* 实现重试机制和结果行数限制
* 结构化返回查询结果

#### 工作流程

1. **知识库检索**：根据用户问题检索相关的表结构、查询示例和字段描述
2. **SQL生成**：LLM基于检索到的上下文信息生成SQL语句
3. **SQL执行**：在数据库上执行SQL，失败时进行错误修复和重试
4. **结果处理**：将查询结果转换为结构化格式返回

### 6.4 查询重构与分发

**查询重构与分发**：在检索前对用户查询进行预处理，包括查询翻译（将问题转换为更适合检索的形式）和查询路由（智能分发到最合适的数据源）。

#### 查询翻译技术

**1. 提示工程**：

* 通过精心设计的提示词引导LLM改写查询
* 高级技巧：让LLM生成结构化指令（如JSON格式），直接指导代码执行排序、过滤等操作
* 适用于需要复杂逻辑处理的查询（如"时间最短的视频"）

**2. 多查询分解（Multi-query）**：

* 将复杂问题拆分成多个简单子问题
* 分别检索后合并去重，形成更全面的上下文
* LangChain提供 `MultiQueryRetriever` 实现

**3. 退步提示（Step-Back Prompting）**：

* 两步流程：先抽象化生成高层"退步问题"，再基于通用原理推理具体答案
* 适用于细节繁多的问题，通过先获取通用知识再推理提升准确性

**4. 假设性文档嵌入（HyDE）**：

* 核心思想：生成一个假设性的理想答案文档，用其向量检索真实文档
* 将"查询到文档"匹配转化为"文档到文档"匹配，提升检索准确率
* 工作流程：生成假设文档 → 编码为向量 → 检索相似文档

#### 查询路由

**应用场景**：

* **数据源路由**：根据查询意图路由到不同知识库（向量数据库、SQL数据库、知识图谱）
* **组件路由**：简单问题用向量检索，复杂任务调用Agent
* **提示模板路由**：为不同类型任务选择最优提示模板

**实现方法**：

1. **基于LLM的意图识别**：

   * LLM分析查询并输出路由标签
   * 使用 `RunnableBranch` 根据标签选择处理链
   * 灵活但延迟较高
2. **嵌入相似性路由**：

   * 计算查询与预设路由描述的向量相似度
   * 选择最相似的路由并调用对应处理链
   * 延迟低，无需LLM分类

### 6.5 检索进阶技术

**检索进阶技术**：在基础向量检索后引入重排序、压缩和校正技术，提升RAG系统的检索精度和答案质量。

#### 重排序（Re-ranking）

**1. RRF（Reciprocal Rank Fusion）**：

* 零样本方法，基于文档在多个检索结果中的排名计算分数
* 计算成本低，但忽略原始相似度分数

**2. RankLLM / LLM-based Reranker**：

* 直接利用LLM判断文档相关性并排序
* 通过精心设计的提示词要求LLM输出排序列表和相关性分数
* 适用于高价值语义理解场景

**3. Cross-Encoder（交叉编码器）**：

* 将查询和文档拼接后输入Transformer模型，输出单一相关性分数
* 精度高但延迟高（需要N次独立推理）
* 适用于Top-K精排场景

**4. ColBERT（Contextualized Late Interaction）**：

* 独立编码查询和文档的每个Token，通过后期交互（MaxSim）计算相似度
* 在精度和效率间取得平衡，支持Token级细粒度交互

#### 压缩（Compression）

**目标**：从检索到的文档中提取与查询最相关的信息，去除噪音文本。

**LangChain实现**：

* `ContextualCompressionRetriever`：包装基础检索器，使用`DocumentCompressor`处理文档
* `LLMChainExtractor`：内容提取，提取相关句子或段落
* `LLMChainFilter`：文档过滤，丢弃不相关文档
* `EmbeddingsFilter`：基于嵌入相似度的快速过滤

**自定义重排器**：

* 继承`BaseDocumentCompressor`基类实现`compress_documents`方法
* 可与LangChain组件组合成处理管道（如ColBERT重排 + LLM压缩）

#### 校正（Correcting）

**校正检索（C-RAG）**：引入"检索-评估-行动"循环，在生成答案前评估文档质量。

**工作流程**：

1. **检索**：从知识库检索文档
2. **评估**：判断文档与查询的相关性（正确/不正确/模糊）
3. **行动**：
   * **正确**：知识精炼，分解文档并过滤无关部分
   * **不正确/模糊**：知识搜索，进行查询重写并触发Web搜索

**优势**：增强系统鲁棒性，减少幻觉，在检索失败时主动寻求外部帮助。

赏

* **本文作者：**
  程序员充电站
* **本文链接：**
  https://itcharge.cn/tech/llm-dev/rag-tutorial-04/
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

![支付宝](https://itcharge.cn/wp-content/themes/S...