---
title: 上下文工程：会话与记忆
url: https://xugaoxiang.com/2025/12/10/context-engineering/
source: 迷途小书童的Note
date: 2025-12-10
fetch_date: 2025-12-11T03:28:05.421606
---

# 上下文工程：会话与记忆

[迷途小书童的Note — 编程、技术、分享](/)

* [付费专栏](https://xugaoxiang.com/category/paying-column/)
* [人工智能](https://xugaoxiang.com/category/ai/)
  + [LLM](https://xugaoxiang.com/category/ai/llm/)
  + [AI资讯](https://xugaoxiang.com/category/ai/ai-daily/)
  + [AI+自媒体实战](https://xugaoxiang.com/category/ai/ai-project/)
  + [AIGC](https://xugaoxiang.com/category/ai/aigc/)
  + [GAN](https://xugaoxiang.com/category/ai/gan/)
  + [OCR](https://xugaoxiang.com/category/ai/ocr/)
  + [OpenCV](https://xugaoxiang.com/category/ai/opencv/)
  + [PyTorch](https://xugaoxiang.com/category/ai/pytorch/)
  + [TensorFlow](https://xugaoxiang.com/category/ai/tensorflow/)
  + [YOLO](https://xugaoxiang.com/category/ai/yolo/)
  + [算法](https://xugaoxiang.com/category/ai/algorithm/)
  + [边缘AI](https://xugaoxiang.com/category/ai/edgeai/)
* [编程语言](https://xugaoxiang.com/category/programming-languages/)
  + [Python](https://xugaoxiang.com/category/programming-languages/python/)
    - [Python基础](https://xugaoxiang.com/category/programming-languages/python/python-foundation/)
    - [实用模块](https://xugaoxiang.com/category/programming-languages/python/modules/)
    - [Flask Web](https://xugaoxiang.com/category/programming-languages/python/flask/)
    - [PyQt5开发](https://xugaoxiang.com/category/programming-languages/python/pyqt5/)
    - [Matplotlib](https://xugaoxiang.com/category/programming-languages/python/matplotlib/)
  + [C/C++](https://xugaoxiang.com/category/programming-languages/c-cpp/)
  + [Rust](https://xugaoxiang.com/category/programming-languages/rust/)
  + [PHP](https://xugaoxiang.com/category/programming-languages/php/)
* [嵌入式](https://xugaoxiang.com/category/embedded/)
  + [RISC-V](https://xugaoxiang.com/category/embedded/riscv/)
* [操作系统](https://xugaoxiang.com/category/operation-system/)
  + [Android](https://xugaoxiang.com/category/operation-system/android/)
  + [Linux](https://xugaoxiang.com/category/operation-system/linux/)
    - [常用命令](https://xugaoxiang.com/category/operation-system/linux/commands/)
  + [Mac](https://xugaoxiang.com/category/operation-system/mac/)
* [IT技巧](https://xugaoxiang.com/category/it-skills/)
  + [科学上网](https://xugaoxiang.com/category/it-skills/%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91/)
  + [自媒体](https://xugaoxiang.com/category/it-skills/wemedia/)
    - [Camtasia](https://xugaoxiang.com/category/it-skills/wemedia/camtasia/)
* [流媒体](https://xugaoxiang.com/category/streaming/)
  + [ffmpeg](https://xugaoxiang.com/category/streaming/ffmpeg/)
  + [ZLMediaKit](https://xugaoxiang.com/category/streaming/zlmediakit/)
  + [Gstreamer](https://xugaoxiang.com/category/streaming/gstreamer/)
* [下载频道](https://xugaoxiang.com/category/download/)
* [捐助墙](https://xugaoxiang.com/category/donation/)

[登录](https://xugaoxiang.com/wp-login.php?redirect_to=https%3A%2F%2Fxugaoxiang.com%2F2025%2F12%2F10%2Fcontext-engineering)   [注册](https://xugaoxiang.com/wp-login.php?action=register)

欢迎访问我的网站，希望内容对您有用，关注公众号后台领取免费学习资料。

> [人工智能](https://xugaoxiang.com/category/ai/) > [LLM](https://xugaoxiang.com/category/ai/llm/) > 上下文工程：会话与记忆

# [上下文工程：会话与记忆](https://xugaoxiang.com/2025/12/10/context-engineering/)

[LLM](https://xugaoxiang.com/category/ai/llm/)  [迷途小书童](https://xugaoxiang.com/author/xugaoxiang/)
 13小时前
 35次浏览
 [0个评论](https://xugaoxiang.com/2025/12/10/context-engineering/#respond)

文章目录

Toggle

* [为什么“上下文 + 会话 + 记忆”对 AI 很重要](#%E4%B8%BA%E4%BB%80%E4%B9%88%E2%80%9C%E4%B8%8A%E4%B8%8B%E6%96%87_%E4%BC%9A%E8%AF%9D_%E8%AE%B0%E5%BF%86%E2%80%9D%E5%AF%B9_AI_%E5%BE%88%E9%87%8D%E8%A6%81)
* [什么是上下文工程(Context Engineering)？](#%E4%BB%80%E4%B9%88%E6%98%AF%E4%B8%8A%E4%B8%8B%E6%96%87%E5%B7%A5%E7%A8%8BContext_Engineering%EF%BC%9F)
* [会话(Session)：对话的容器](#%E4%BC%9A%E8%AF%9DSession%EF%BC%9A%E5%AF%B9%E8%AF%9D%E7%9A%84%E5%AE%B9%E5%99%A8)
* [记忆 (Memory)：跨会话长久记忆](#%E8%AE%B0%E5%BF%86_Memory%EF%BC%9A%E8%B7%A8%E4%BC%9A%E8%AF%9D%E9%95%BF%E4%B9%85%E8%AE%B0%E5%BF%86)
* [实际系统里是如何工作的 (一个典型流程)](#%E5%AE%9E%E9%99%85%E7%B3%BB%E7%BB%9F%E9%87%8C%E6%98%AF%E5%A6%82%E4%BD%95%E5%B7%A5%E4%BD%9C%E7%9A%84_%E4%B8%80%E4%B8%AA%E5%85%B8%E5%9E%8B%E6%B5%81%E7%A8%8B)
* [记忆的分类与管理](#%E8%AE%B0%E5%BF%86%E7%9A%84%E5%88%86%E7%B1%BB%E4%B8%8E%E7%AE%A1%E7%90%86)
* [总结](#%E6%80%BB%E7%BB%93)

## 为什么“上下文 + 会话 + 记忆”对 AI 很重要

当下很多AI，尤其是基于大型语言模型（LLM）的智能体，本质上是无状态(stateless)的。也就是说，每次你让它回答，都只能看到你这一次发过去的文字(prompt) — 它不会记住你之前说过什么，也不会知道你之前的对话内容。

这对于简单问答还行，但如果你想让AI记住你喜欢什么、帮你进行多轮对话、长期协作、个性化建议。。。，仅靠一次性prompt是不够的。于是，就需要构建状态(state)和记忆(memory)。而这正是本文所强调的 — 要让AI成为有状态、能记忆、能个性化的智能体(agent)，就必须做**上下文工程(Context Engineering)**。

## 什么是上下文工程(Context Engineering)？

简单说，就是动态地组装和管理供LLM使用的信息(context)，确保它在每一次回应时，既有足够相关的信息，也不过载。

这个上下文(context)可能包括：

* 系统指令(System instructions)：告诉AI你是谁，你能做什么，你要遵守什么规则。例如 ”你是一个贴心助理，帮我管理日程”。
* 工具定义(Tool definitions)：如果AI可以调用外部API、函数(比如查天气、查百科、生成图像。。。)，你就把这些功能描述放进context，让AI知道它能做什么。
* 少量示例(Few-shot examples)：给模型几个”问 -> 答”的示例，让它按类似方式响应。
* 事实、证据、外部知识(External data、knowledge)：比如从数据库、文档、网页检索到的信息，提供给模型做依据。
* 对话历史(Conversation history)：当前这轮、或之前几轮对话，让AI知道之前发生了什么。
* 暂存、工作状态(State、scratchpad)：一些临时数据，帮AI追踪对话中间过程、决定下一步。

通过上下文工程，开发者把这些不同来源的信息按需、动态组装，传给LLM — 就像给它“当前任务+背景+工具+历史+数据”。

## 会话(Session)：对话的容器

会话(Session)就是指某一次完整交互(从开始对话到结束)的记录容器。每次你跟AI交谈，就开启一个session，记录你的每一句话、AI的每一句回应(这些是事件、history)，以及AI在对话过程中可能维护的临时状态(state、memory buffer)。

* 对话历史(history)：按时间顺序记录你说了什么、AI 回答什么。
* 工作状态(state、working memory)：记录当前对话中AI临时需要记住的数据，比如“你刚刚选了这本书，要记住书名、偏好、要不要推荐类似书”。

在单次会话里，Session保证AI能记住上下文 -> 这样交谈不会断片。但如果只是依赖session，当对话结束后，这些信息通常也就丢失了(除非特别保存)。

## 记忆 (Memory)：跨会话长久记忆

为了让AI跨会话(多次对话)也“记得你是谁、你喜欢什么、之前做过什么”，需要一个持久化机制 — 这就是“记忆(Memory)” 的作用。

* 记忆 = 从会话、对话、外部数据中提取出的有价值、重要、可复用的信息片段。
* 它可能是结构化(比如JSON、字典) — “喜欢窗边座位”、“生日 1990-01-01”、某次偏好设置等；也可能是非结构化 (自然语言) 的描述：“上次你说你喜欢科幻小说”。

记忆通常通过记忆管理器(Memory Manager)存储和检索。未来当你再与AI对话时，系统可以把相关记忆加载进context，这样AI就知道你是谁、之前讲过什么、喜欢什么、做过什么，从而提供连贯、个性化的服务体验。

## 实际系统里是如何工作的 (一个典型流程)

1. **获取上下文(Fetch Context)**：当用户发起请求、对话，系统先从memory bank(长期记忆存储)、外部数据库 (知识库)、以及当前session的对话历史、状态中，抓取相关信息。
2. **准备上下文(Prepare Context)**：把这些信息(系统指令、工具定义、外部知识、对话历史、用户 prompt。。。) 动态拼凑成一个prompt，送给LLM。
3. **调用 LLM + 工具 (Invoke LLM & Tools)**：LLM根据prompt生成回应；如果需要调用工具(如数据库查询、计算、API等)，也一起执行，然后把输出作为新的信息补进context。
4. **上传、保存上下文(Upload、Persist Context)**：将新的对话、状态、从工具、external数据得到的信息，以及可能抽象、提炼出的记忆存入memory bank。这样下一次对话就可以重用。

这个循环不断进行，使得AI随着交谈积累经验、变得越来越懂你。

## 记忆的分类与管理

记忆系统并不是单一的一个大日志，它可以有不同形式、不同用途。比如：

* **结构化记忆(Structured user profile)**：用户偏好、基本信息、配置 — 便于快速查找与检索。
* **滚动摘要(Rolling summary)**：将长对话压缩为简洁摘要，只保留关键事实、结论、偏好，节省空间、token、检索代价。
* **集合(Collection)**：多个独立记忆条目(facts、events)，适合存储多次交互中分散的小事实、小偏好。
* **多模态记忆 (Multimodal memory)**：不仅是文字，也可能是图像、文件、声音、其他形式 — 让AI记住更丰富的信息(例如你给它的一张图、一个文档、一次语音说明)。

记忆一般存储在专门的系统中(向量数据库、知识图谱、key-value存储、memory bank等)，并带有元数据(来源、时间、置信度、类型)，方便后续检索与信任判断。

## 总结

* 现代 LLM 本身是无状态的，要让它记得你，陪你做长期、多轮、个性化任务，就需要上下文工程，即在每一次对话中智能地、动态地为它装载有用信息(context)。
* Session提供短期上下文，使一次对话连贯；Memory提供跨会话、长期上下文，使AI能记住你的偏好、历史、习惯。
* 两者结合，配合合理的context构建与memory管理机制，AI才能成为真正有状态、有记忆、有个性的智能体 — 更像助手、伙伴、长期协作者。

简单来说：Session是当下，Memory 是过去、历史。

对于希望打造智能对话助手、长期互动系统、个性化服务的人来说，理解这套体系 (上下文工程 + 会话 + 记忆) 是基础，也是关键。

最后附上上月 Google 出品的上下文工程白皮书地址：<https://www.kaggle.com/whitepaper-context-engineering-sessions-and-memory>

喜欢 (0)

[LLM，上下文工程，提示词](https://xugaoxiang.com/tag/llm%EF%BC%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E5%B7%A5%E7%A8%8B%EF%BC%8C%E6%8F%90%E7%A4%BA%E8%AF%8D/)

 [颠覆视频创作！一键替换3D角色，你还不来试试？](https://xugaoxiang.com/2024/09/21/motionshop/)

![支持作者一杯咖啡]()

没有相关文章!

* 暂无相关文章！

### 您必须 [登录](https://xugaoxiang.com/wp-login.php?redirect_to=https%3A%2F%2Fxugaoxiang.com%2F2025%2F12%2F10%2Fcontext-engineering%2F) 才能发表评论！

## 加微信领取免费学习资料

[![](https://xugaoxiang.com/wp-content/uploads/2024/06/2024062702421430-726x1024.jpg "加微信，进AI交流群")](https://image.xugaoxiang.com/imgs/2024/06/c87841fd5922693b.jpg)

## 博主同款VPS

[![](https://xugaoxiang.com/wp-content/uploads/2019/12/images-9.jpg "博主同款VPS")](https://bandwagonhost.com/aff.php?aff=45788)

网站托管VPS

## 科学上网工具

[![](https://xugaoxiang.com/wp-content/uploads/2023/06/2023062413113190.png "科学上网")](https://justmysocks.net/members/aff.php?aff=1725)

博主在用梯子

## 近期文章

* [上下文工程：会话与记忆](https://xu...