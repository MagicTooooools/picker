---
title: RAG 教程笔记（Task02）
url: https://itcharge.cn/tech/llm-dev/rag-tutorial-02/
source: 程序员充电站
date: 2025-12-19
fetch_date: 2025-12-20T03:20:01.144354
---

# RAG 教程笔记（Task02）

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

# RAG 教程笔记（Task02）

[2025-12-19](https://itcharge.cn/tech/llm-dev/rag-tutorial-02/)

3.769k 字  |  13 分钟

## 3. 数据加载

* 当前主流 RAG 文档加载器：

| 工具名称 | 特点 | 适用场景 | 性能表现 |
| --- | --- | --- | --- |
| **PyMuPDF4LLM** | PDF → Markdown 转换，OCR + 表格识别 | 科研文献、技术手册 | 开源免费，GPU 加速 |
| **TextLoader** | 基础文本文件加载 | 纯文本处理 | 轻量高效 |
| **DirectoryLoader** | 批量目录文件处理 | 混合格式文档库 | 支持多格式扩展 |
| **Unstructured** | 多格式文档解析 | PDF、Word、HTML 等 | 统一接口，智能解析 |
| **FireCrawlLoader** | 网页内容抓取 | 在线文档、新闻 | 实时内容获取 |
| **LlamaParse** | 深度PDF结构解析 | 法律合同、学术论文 | 解析精度高，商业 API |
| **Docling** | 模块化企业级解析 | 企业合同、报告 | IBM 生态兼容 |
| **Marker** | PDF→Markdown，GPU加速 | 科研文献、书籍 | 专注 PDF 转换 |
| **MinerU** | 多模态集成解析 | 学术文献、财务报表 | 集成 LayoutLMv3+YOLOv8 |

## 4. 文本分块

文本分块（Text Chunking）是构建 RAG 流程的关键步骤，将长篇文档切分成更小、更易于处理的单元，作为后续向量检索和模型处理的**基本单位**。

### 4.1 文本分块重要性

#### 4.1.1 满足模型上下文限制

* **嵌入模型**: 有严格的输入长度上限（如 `bge-base-zh-v1.5` 为512个token），超出限制会被截断，导致信息丢失。
* **大语言模型**: 虽然上下文窗口较大，但检索到的所有文本块、用户问题和提示词都必须能放入窗口中。块过大会限制可参考的信息广度。

#### 4.1.2 为何"块"不是越大越好

**块的大小并非越大越好**，过大的块会严重影响RAG系统的性能：

1. **嵌入过程中的信息损失**: 嵌入模型通过池化将所有token向量压缩成单一向量。文本块越长，语义信息越稀释，检索精度下降。
2. **生成过程的"大海捞针" (Lost in the Middle)**: LLM处理长上下文时倾向于记住开头和结尾，忽略中间部分。大块文本会让关键信息被淹没。
3. **主题稀释导致检索失败**: 好的文本块应聚焦单一主题。包含多个不相关主题的块会导致语义稀释，检索时无法精确匹配。

### 4.2 基础分块策略

#### 4.2.1 固定大小分块

最简单直接的分块方法，`CharacterTextSplitter` 的工作原理：

1. **按段落分割**：使用默认分隔符 `"\n\n"` 按段落分割
2. **智能合并**：监控累积长度，超过 `chunk_size` 时形成新块，通过 `chunk_overlap` 保持上下文连续性

**特点**：优先保持段落完整性，实际是"段落感知的自适应分块"。

```
from langchain.text_splitter import CharacterTextSplitter
from langchain_community.document_loaders import TextLoader

loader = TextLoader("../../data/C2/txt/蜂医.txt")
docs = loader.load()

text_splitter = CharacterTextSplitter(
    chunk_size=200,    # 每个块的目标大小
    chunk_overlap=10   # 块之间重叠字符数
)

chunks = text_splitter.split_documents(docs)
```

**优势**：实现简单、处理速度快、计算开销小
**劣势**：可能在语义边界处切断文本

#### 4.2.2 递归字符分块

`RecursiveCharacterTextSplitter` 通过分隔符层级递归处理，改善超长文本的处理效果。

**算法流程**：

1. 寻找有效分隔符：从分隔符列表中找到第一个存在的分隔符
2. 切分与分类：使用分隔符切分文本
   * 片段不超过块大小 → 暂存准备合并
   * 片段超过块大小 → 先合并已暂存片段，再递归分割或保留为超长块
3. 最终处理：合并剩余暂存片段

**关键差异**：固定大小分块遇到超长段落只能警告并保留；递归分块会继续使用更细粒度分隔符（段落→句子→单词→字符）直到满足大小要求。

```
from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    separators=["\n\n", "\n", "。", "，", " ", ""],  # 分隔符优先级
    chunk_size=200,
    chunk_overlap=10,
)

# 多语言支持（中文、日文、泰文）
separators=["\n\n", "\n", " ", ".", ",", "\u200b", "\uff0c", "\u3001", "\uff0e", "\u3002", ""]

# 编程语言特化支持
from langchain.text_splitter import Language
splitter = RecursiveCharacterTextSplitter.from_language(
    language=Language.PYTHON,  # 支持Python、Java、C++等
    chunk_size=500,
    chunk_overlap=50
)
```

#### 4.2.3 语义分块

语义分块（Semantic Chunking）不依赖固定字符数或预设分隔符，而是**在语义主题发生显著变化的地方进行切分**，使每个分块具有高度的内部语义一致性。

**实现原理**（`SemanticChunker`）：

1. **句子分割**：将文本拆分成句子列表
2. **上下文感知嵌入**：通过 `buffer_size` 参数，将每个句子与前后句子组合后进行嵌入，融入上下文语义
3. **计算语义距离**：计算相邻句子的嵌入向量余弦距离，量化语义差异
4. **识别断点**：根据统计方法（默认 `percentile`）确定动态阈值，识别语义断点
5. **合并成块**：根据断点位置切分并合并句子

**断点识别方法**：

* `percentile`（百分位法，默认）：第95百分位作为阈值
* `standard_deviation`（标准差法）：平均值 + 3倍标准差
* `interquartile`（四分位距法）：Q3 + 1.5倍IQR
* `gradient`（梯度法）：计算差异值变化率，适合法律、医疗文档

```
from langchain_experimental.text_splitter import SemanticChunker
from langchain_community.embeddings import HuggingFaceEmbeddings

embeddings = HuggingFaceEmbeddings(
    model_name="BAAI/bge-small-zh-v1.5",
    model_kwargs={'device': 'cpu'},
    encode_kwargs={'normalize_embeddings': True}
)

text_splitter = SemanticChunker(
    embeddings,
    breakpoint_threshold_type="percentile"
)
```

#### 4.2.4 基于文档结构的分块

对于具有明确结构标记的文档格式（如Markdown、HTML、LaTex），可利用这些标记实现更智能的分割。

**Markdown 结构分块**（`MarkdownHeaderTextSplitter`）：

* **实现原理**：先按标题分组，再按需细分

  1. 定义标题层级映射：`[ ("#", "Header 1"), ("##", "Header 2") ]`
  2. 内容聚合：将每个标题下的内容聚合，并赋予包含完整标题路径的元数据
* **元数据注入优势**：为每个块提供精确"地址"，增强上下文准确性
* **组合使用**：可与 `RecursiveCharacterTextSplitter` 组合

  1. 先用 `MarkdownHeaderTextSplitter` 按标题分割成带元数据的逻辑块
  2. 再对逻辑块应用 `RecursiveCharacterTextSplitter` 切分为符合大小要求的小块
  3. 小块会继承标题元数据

**优势**：既保留文档宏观逻辑结构，又确保块大小适中

### 4.3 其他开源框架中的分块策略

#### 4.3.1 Unstructured：基于文档元素的智能分块

* **分区 (Partitioning)**：将文档解析成结构化元素（`Title`、`NarrativeText`、`ListItem` 等）
* **分块方法**：
* `basic`：连续组合元素直到达到 `max_characters` 上限
* `by_title`：在 `basic` 基础上，将 `Title` 元素视为新章节开始，强制在此处切分

**优势**："先理解、后分割"策略，最大程度保留文档原始语义结构

#### 4.3.2 LlamaIndex：面向节点的解析与转换

将数据处理抽象为对"节点（Node）"的操作，分块是节点转换的一环。

**节点解析器类型**：

* **结构感知型**：`MarkdownNodeParser`、`JSONNodeParser`、`CodeSplitter` 等
* **语义感知型**：
  + `SemanticSplitterNodeParser`：类似 LangChain 的 `SemanticChunker`
  + `SentenceWindowNodeParser`：将文档切分成单个句子，在元数据中存储前后N个句子作为"窗口"
* **常规型**：`TokenTextSplitter`、`SentenceSplitter` 等

**特点**：灵活的转换流水线、丰富的元数据、良好的互操作性（`LangchainNodeParser`）

#### 4.3.3 ChunkViz：可视化分块工具

可视化工具，用不同颜色块展示每个 chunk 的边界和重叠部分，方便理解分块逻辑。

赏

* **本文作者：**
  程序员充电站
* **本文链接：**
  https://itcharge.cn/tech/llm-dev/rag-tutorial-02/
* **版权声明：**
  本站所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。

* [大模型应用开发](https://itcharge.cn/category/tech/llm-dev/)
* [技术](https://itcharge.cn/category/tech/)

# 评论（没有评论）

![](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/head.jpg)

# [程序员充电站](/)

高效率编程，慢节奏生活。

[77
文 章](/archive)

[18
分 类](/category)

[21
标 签](/tag)

# 最新文章

* [2024 年目标清单](https://itcharge.cn/life/learning/2024-flags/)
* [Agent 教程笔记（Task02）](https://itcharge.cn/tech/llm-dev/agent-tutorial-02/)
* [RAG 教程笔记（Task02）](https://itcharge.cn/tech/llm-dev/rag-tutorial-02/)
* [Agent 教程笔记（Task01）](https://itcharge.cn/tech/llm-dev/agent-tutorial/)
* [RAG 教程笔记（Task01）](https://itcharge.cn/tech/llm-dev/rag-tutorial/)

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
![扫码支持](https://itcharge.cn/wp-content/uploads/alipay.pn...