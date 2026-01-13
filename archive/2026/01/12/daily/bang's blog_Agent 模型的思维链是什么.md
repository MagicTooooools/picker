---
title: Agent 模型的思维链是什么
url: https://blog.cnbang.net/uncategorized/4279/
source: bang's blog
date: 2026-01-12
fetch_date: 2026-01-13T03:34:13.371618
---

# Agent 模型的思维链是什么

[bang's blog](/)

[APPs](http://app.cnbang.net)
[存档](/archives)
[作品](/works)
[关于](/about)

# Agent 模型的思维链是什么 2026-1-12

关于 Agent 模型的思维链，之前被几个高大上的词绕晕了，claude 提出 Interleaved Thinking（交错思维链），MiniMax M2 追随和推动标准化，K2 叫 Thinking-in-Tools，Deepseek V3.2 写的是 Thinking in Tool-Use，gemini 则是 Thought Signature（思考签名）。了解了下，概念上比较简单，基本是一个东西，就是定义了模型思考的内容怎样在 Agent 长上下文里传递。

## 是什么

在25年年初 deepseek 的轰炸下，思考模型大家都很熟悉了，在 Chatbot 单轮对话中，模型会先输出思考的内容，再输出正文。再早的 GPT-o1 也一样，只不过 o1 不把完整的思考内容输出。

在 Chatbot 进行多轮对话时，每一次思考的内容是不会再带入上下文的。每次到下一轮时，思考的内容都会被丢弃，只有用户 prompt 和模型回答的正式内容会加到上下文。因为在普通对话的场景下没必要，更倾向于单轮对话解决问题，长上下文会干扰模型，也会增加 token 消耗。

[![](https://blog.cnbang.net/wp-content/uploads/2026/01/image-2.png)](https://blog.cnbang.net/wp-content/uploads/2026/01/image-2.png)

这些思考模型用到 Agent 上，就是下图这样，每次模型输出工具调用，同时都会输出 thinking 内容，思考应该调什么工具，为什么调，但下次这个思考内容会被丢弃，不会带入上下文：

[![](https://blog.cnbang.net/wp-content/uploads/2026/01/image-3.png)](https://blog.cnbang.net/wp-content/uploads/2026/01/image-3.png)
> Agent 的 loop 是：用户输入 → 模型输出工具调用 → 调用工具得出结果 → 模型输入下一步工具调用 → 调用工具得出结果 → …. 直到任务完成或需要用户新的输入。

这不利于模型进行多轮长链路的推理，于是 claude 4 sonnet 提出把 thinking 内容带入上下文这个事内化到模型，以提升 Agent 性能，上下文的组织变成了这样：

[![](https://blog.cnbang.net/wp-content/uploads/2026/01/image-4.png)](https://blog.cnbang.net/wp-content/uploads/2026/01/image-4.png)

就这样一个事，称为 Interleaved Thinking，其他的叫法也都是一样的原理。

## 为什么要带 thinking

面向 Chatbot 的模型，倾向于一次性解决问题，尽量在一次 thinking 一次输出解决问题。

Agent 相反，倾向于多步不断跟环境( tool 和 user )交互解决问题。

Agent 解决一个复杂问题可能要长达几十轮工具调用，如果模型对每次调用工具的思考内容都抛弃，只留下结果，模型每次都要重新思考每一轮为什么要调这个工具，接下来应该调什么工具。这里每一次的重新思考如果跟原来的思考推理有偏移，最终的结果就会有很大的出入和不稳定，这种偏移在多轮下几乎一定会发生。

如果每一轮调用的思考内容都放回上下文里，每次为什么调工具的推理逻辑上下文都有，思维链完整，就大大减少了模型对整个规划的理解难度和对下一步的调用计划的偏差。

有没有带 thinking 内容，对效果有多大差别？MiniMax-M2 提供了他们的数据，在像 Tau 这种机票预订和电商零售场景的任务 benchmark 提升非常明显，这类任务我理解需要操作的步数更多（比如搜索机票→筛选过滤→看详情→下单→支付），模型在每一步对齐前面的思路很重要，同一个工具调用可能的理由随机性更大，每一步的思考逻辑带上后更稳定。

[![](https://blog.cnbang.net/wp-content/uploads/2026/01/image-5.png)](https://blog.cnbang.net/wp-content/uploads/2026/01/image-5.png)

## 工程也能做？

这么一个简单的事，不用模型支持，直接工程上拼一下给模型是不是也一样？比如手动把思考内容包在一个标签(<think>)里，伪装成 User Message 或 ToolResult 的一部分放在里面，也能达到保留思考的效果。

很多人应该这样做过，但跟模型原生支持还是有较大差别。

工程手动拼接，模型只会认为这部分仍是用户输入，而且模型的训练数据和流程没有这种类型的用户输入和拼接，效果只靠模型通用智能随意发挥。

模型原生支持，训练时就可以针对这样规范的上下文训练，有标注大量的包含思考过程的 trajectory 轨迹数据训练，响应的稳定性必然会提升，这也是 Agent 模型的重点优化点之一。

## 签名

上述工具调用的 thinking 内容带到下一轮上下文，不同的模型做了不同额外的处理，主要是加了不同程度的签名，有两种：

### thinking 内容原文，带签名校验

claude 和 gemini 都为 thinking 的内容加了签名校验，带到下一轮时，模型会前置判断思考内容有没有被篡改。

为什么要防 thinking 内容被篡改？毕竟 prompt 也可以随便改，同样是上下文的 thinking 内容改下也没什么。

主要应该是篡改了模型的 thinking 内容会打乱模型的思路，让效果变差，这也是需要避免的。

另外模型在训练和对齐时，已经默认 thinking 是模型自己的输出，不是用户随意的输入，这是两个不同类型的数据，如果实际使用时变成跟Prompt一样可随意篡改，可能有未知的安全问题。

不过国内模型目前没看到有加这个签名校验的。

### thinking 内容加密

claude 在一些情况下不会输出自然语言的 thinking 内容，而是包在`[redacted_thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking#thinking-redaction)`里，是一串加密后的数据。

而 gemini 2.5/3.0 的 Agent 思维链没有明文的 thinking 字段，而是 `[thought_signature](https://ai.google.dev/gemini-api/docs/thought-signatures)`，也是一串加密后的数据。

用这种加密的非自然语言数据，一个好处是，它可以是对模型内部更友好、压缩率更大的数据表述方式，也可以在一些涉及安审的场景下内容不泄露给用户。

更重要的还是防泄漏，这就跟最开始 GPT o1 不输出所有思考内容一样，主要是为了不暴露思考过程，模型发布后不会太轻易被蒸馏。

## 最后

目前 claude 4 sonnet、gemini 3 在 Agent 工具调用的场景下，都强制要求带工具调用的思考内容和签名，这个链路正常是能很大程度提升整体的推理执行效果，是 Agent 多步骤推理的必需品。

但目前 Agent 模型的稳定性还是个问题，例如在某些场景下，业务逻辑明确需要下一步应该调工具 A，但模型思考后可能就是会概率性的调工具B，在以前是可以直接 hack 替换调工具调用，或手动插入其他工具调用，没有副作用。

但在思维链这套机制下比较麻烦，因为没法替模型输出这个工具调用的思考内容，一旦打破这个链，对后续推理的效果和稳定性都会有影响。

可能模型厂商后续可以出个允许上层纠错的机制，例如可以在某个实际告诉函数工具选择错误，重新思考，原生支持，弥补模型难以保障稳定的不足。

分类:[懒得分类](https://blog.cnbang.net/category/uncategorized/)

[上一篇：2025](https://blog.cnbang.net/living/4271/)

评论

Name:

Email:

Δ

#### 分类目录

* [生活](https://blog.cnbang.net/category/living/) (223)
* [技术文章](https://blog.cnbang.net/category/tech/) (98)
* [随记](https://blog.cnbang.net/category/writting/) (86)
* [作品](https://blog.cnbang.net/category/works/) (50)
* [互联网](https://blog.cnbang.net/category/internet/) (36)
* [懒得分类](https://blog.cnbang.net/category/uncategorized/) (12)

#### 标签云

[AI](https://blog.cnbang.net/tag/ai/)
[as3](https://blog.cnbang.net/tag/as3/)
[flash](https://blog.cnbang.net/tag/flash/)
[ios](https://blog.cnbang.net/tag/ios/)
[iphone](https://blog.cnbang.net/tag/iphone/)
[javascript](https://blog.cnbang.net/tag/javascript/)
[jquery](https://blog.cnbang.net/tag/jquery/)
[JSPatch](https://blog.cnbang.net/tag/jspatch/)
[NBA](https://blog.cnbang.net/tag/nba/)
[node.js](https://blog.cnbang.net/tag/nodejs/)
[twitese](https://blog.cnbang.net/tag/twitese/)
[twitter](https://blog.cnbang.net/tag/twitter/)
[互联网](https://blog.cnbang.net/tag/internet/)
[历程](https://blog.cnbang.net/tag/journey/)
[回忆](https://blog.cnbang.net/tag/memory/)
[大学](https://blog.cnbang.net/tag/university/)
[大学](https://blog.cnbang.net/tag/%E5%A4%A7%E5%AD%A6/)
[学习](https://blog.cnbang.net/tag/study/)
[实习](https://blog.cnbang.net/tag/practice/)
[家](https://blog.cnbang.net/tag/home/)
[微博](https://blog.cnbang.net/tag/%E5%BE%AE%E5%8D%9A/)
[总结](https://blog.cnbang.net/tag/summarize/)
[成长](https://blog.cnbang.net/tag/growing/)
[所想](https://blog.cnbang.net/tag/thinking/)
[旅游](https://blog.cnbang.net/tag/%E6%97%85%E6%B8%B8/)
[游戏](https://blog.cnbang.net/tag/game/)
[源码解析](https://blog.cnbang.net/tag/%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/)
[牙](https://blog.cnbang.net/tag/tooth/)
[现象](https://blog.cnbang.net/tag/phenomenon/)
[病](https://blog.cnbang.net/tag/illness/)
[社团](https://blog.cnbang.net/tag/organization/)
[编程](https://blog.cnbang.net/tag/coding/)
[腾讯](https://blog.cnbang.net/tag/tencent/)
[观点](https://blog.cnbang.net/tag/view/)
[设计](https://blog.cnbang.net/tag/design/)
[评论](https://blog.cnbang.net/tag/comment/)
[音乐](https://blog.cnbang.net/tag/music/)
[饭否](https://blog.cnbang.net/tag/fanfou/)
[高中](https://blog.cnbang.net/tag/hight-school/)
[高考](https://blog.cnbang.net/tag/gaokao/)

Powered by Wordpress | Copyright bang 2006-2015