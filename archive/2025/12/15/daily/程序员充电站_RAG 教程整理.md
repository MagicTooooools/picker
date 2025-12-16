---
title: RAG 教程整理
url: https://itcharge.cn/tech/llm-dev/rag-tutorial/
source: 程序员充电站
date: 2025-12-15
fetch_date: 2025-12-16T03:27:47.730039
---

# RAG 教程整理

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

# RAG 教程整理

[2025-12-15](https://itcharge.cn/tech/llm-dev/rag-tutorial/)

3.645k 字  |  13 分钟

## 1. RAG 简介

### 1.1 RAG 定义

> **RAG**：**检索增强生成** 的缩写。它是一种将 **信息检索** 与 **大语言模型** 相结合的人工智能框架。简单来说，它的工作原理是：**「先查找，再回答」**。

当用户提出一个问题时，RAG 系统不会直接让 LLM 凭空生成答案，而是会先从外部的知识库（如文档、数据库、网页）中检索出与问题最相关的信息片段，然后将这些信息片段和原始问题一起「喂」给 LLM，让 LLM 基于这些 **最新、最准确、最具体** 的上下文信息来生成最终答案。

RAG 就像给一个博学但记忆有延迟的学者（LLM）配了一个超级图书馆员和一台实时资料检索机。每当学者需要回答问题时，图书馆员立刻从海量、最新的书库中找出最相关的几页资料递给他，学者再基于这些资料给出精准可靠的回答。

### 1.2 RAG 的工作原理

一个典型的 RAG 系统工作流程如下：

![image-20251215211006311](https://qcdn.itcharge.cn/images/202512152110107.png)

**第一步：知识库构建（预处理，离线进行）**

* **文档加载**：收集所有相关的文档（PDF、Word、网页、数据库等）。
* **切分**：将长文档分割成更小的、有意义的「块」。
* **向量化**：使用嵌入模型将每个文本块转换为一个高维数值向量（称为「嵌入」）。这个向量代表了文本的语义。
* **存储**：将这些向量及其对应的原始文本存储到**向量数据库**中。

**第二步：检索与生成（在线响应用户查询）**

1. **检索**：当用户提出问题时，系统使用相同的嵌入模型将问题也转换为一个向量。然后，在向量数据库中搜索与这个问题向量**最相似**的文本块向量（通常使用余弦相似度）。这些被找到的文本块就是最相关的上下文。
2. **增强**：系统将用户的原始问题和检索到的相关文本片段组合成一个结构化的「提示」，交给 LLM。提示通常类似于：
   > 「请基于以下上下文信息回答问题。如果上下文信息不足以回答问题，请直接说‘根据提供的信息，我无法回答这个问题’。
   > **上下文：**
   > [这里插入检索到的相关文本片段1]
   > [这里插入检索到的相关文本片段2]
   > **问题：** [用户的问题]
   > **答案：**」
3. **生成**：LLM 接收到这个富含相关上下文的提示后，生成一个准确、有据可依的答案。系统通常还会要求 LLM 在答案中注明引用的来源。

### 1.3 为什么需要 RAG

大语言模型虽然强大，但存在几个关键局限，RAG 正是为了解决这些问题而生的：

1. **知识过时 / 静态**：LLM 的训练数据有截止日期（例如，GPT-4 的知识截止到 2023年4月）。它无法知道这之后发生的事件或最新的信息。
2. **缺乏领域 / 专有知识**：LLM 对通用知识掌握得很好，但对于企业内部文档、特定领域的非公开资料、个人笔记等「私有知识」一无所知。
3. **容易「幻觉」**：当 LLM 遇到其训练数据中不明确或不存在的信息时，它可能会自信地编造一个看似合理但错误的答案。
4. **无法溯源**：LLM 的答案是一个「黑箱」，你无法知道它的回答是基于哪些具体信息源得出的，难以验证其可信度。

#### 1.3.1 RAG 的主要优势

* **答案准确性高**：基于最新、最相关的具体信息生成，大幅减少「幻觉」。
* **知识可更新**：只需更新向量数据库中的文档，即可让系统获得新知识，无需重新训练昂贵的 LLM。
* **成本效益**：比为了获取新知识而重新训练或微调一个 LLM 要便宜和快速得多。
* **可溯源与可信**：答案可以追溯到具体的源文档，提高了透明度和可信度。
* **易于实现**：有成熟的工具链（如 LangChain、LlamaIndex）和向量数据库（如 Pinecone、Chroma、Weaviate）支持。

#### 1.3.2 RAG 的典型应用场景

* **智能客服/问答系统**：基于产品手册、FAQ文档回答用户问题。
* **企业知识库助手**：员工可以快速查询公司内部的规章制度、项目报告、会议纪要等。
* **学术研究助手**：基于大量的论文库回答专业问题。
* **法律、医疗等专业领域顾问**：基于法律法规、病例数据库提供信息参考。
* **任何需要基于特定、最新文档进行问答的场景**。

#### 1.3.3 与微调的区别

* **RAG**：侧重于为 LLM **提供外部知识**。LLM 本身的能力（如逻辑、文风）不变，但获得了回答问题所需的具体材料。**适合知识密集型任务**。
* **微调**：通过额外的数据训练，**改变 LLM 本身的权重参数**，使其适应特定任务、风格或领域。**适合改变模型行为或风格**。

在选择技术路径时，经常是先尝试提示工程，再选择 RAG，最后才考虑微调。

### 1.4 RAG 系统的评估

一套 RAG 系统的好坏，可以从 **检索质量**（评估检索器是否找到了包含答案所需信息的文档片段）、**生成质量**（在检索到相关上下文的基础上，评估 LLM 生成答案的质量）。

## 2. 简易 RAG 实现

```
import os
# hugging face镜像设置，如果国内环境无法使用启用该设置
# os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'
from dotenv import load_dotenv
from langchain_community.document_loaders import UnstructuredMarkdownLoader
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_core.prompts import ChatPromptTemplate
from langchain_deepseek import ChatDeepSeek

load_dotenv()

markdown_path = "../../data/C1/markdown/easy-rl-chapter1.md"

# 加载本地markdown文件
loader = UnstructuredMarkdownLoader(markdown_path)
docs = loader.load()

# 文本分块
text_splitter = RecursiveCharacterTextSplitter()
chunks = text_splitter.split_documents(docs)

# 中文嵌入模型
embeddings = HuggingFaceEmbeddings(
    model_name="BAAI/bge-small-zh-v1.5",
    model_kwargs={'device': 'cpu'},
    encode_kwargs={'normalize_embeddings': True}
)

# 构建向量存储
vectorstore = InMemoryVectorStore(embeddings)
vectorstore.add_documents(chunks)

# 提示词模板
prompt = ChatPromptTemplate.from_template("""请根据下面提供的上下文信息来回答问题。
请确保你的回答完全基于这些上下文。
如果上下文中没有足够的信息来回答问题，请直接告知：“抱歉，我无法根据提供的上下文找到相关信息来回答此问题。”

上下文:
{context}

问题: {question}

回答:"""
                                          )

# 配置大语言模型
llm = ChatDeepSeek(
    model="deepseek-chat",
    temperature=0.7,
    max_tokens=4096,
    api_key="sk-xxxxxxxxxxxxxxxxxxxxxxxxx"
)

# 用户查询
question = "文中举了哪些例子？"

# 在向量存储中查询相关文档
retrieved_docs = vectorstore.similarity_search(question, k=3)
docs_content = "\n\n".join(doc.page_content for doc in retrieved_docs)

answer = llm.invoke(prompt.format(question=question, context=docs_content))
print(answer)
```

## 3. RAG 系统构建

### 3.1 数据加载

* 当前主流RAG文档加载器：

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

## 3.2 文本分块

赏

* **本文作者：**
  程序员充电站
* **本文链接：**
  https://itcharge.cn/tech/llm-dev/rag-tutorial/
* **版权声明：**
  本站所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。

* [大模型应用开发](https://itcharge.cn/category/tech/llm-dev/)
* [技术](https://itcharge.cn/category/tech/)

# 评论（没有评论）

![](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/head.jpg)

# [程序员充电站](/)

高效率编程，慢节奏生活。

[74
文 章](/archive)

[18
分 类](/category)

[21
标 签](/tag)

# 最新文章

* [2024 年目标清单](https://itcharge.cn/life/learning/2024-flags/)
* [RAG 教程整理](https://itcharge.cn/tech/llm-dev/rag-tutorial/)
* [Docker 万字教程：从入门到掌握](https://itcharge.cn/tech/server/docker-tutorial/)
* [Docker、Kubernetes、KubeSphere 简易使用文档](https://itcharge.cn/tech/server/docker-kubernetes-kubesphere-doc/)
* [2023 年目标清单](https://itcharge.cn/life/learning/2023-flags/)

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

![支付宝](https://itcharge.cn/wp-content/themes/Sapphire/assets/im...