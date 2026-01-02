---
title: LLM的弱点——不能很好的讲笑话？
url: https://blog.est.im/2026/stderr-01
source: est の 输入 输出和出入
date: 2026-01-01
fetch_date: 2026-01-02T03:33:19.576929
---

# LLM的弱点——不能很好的讲笑话？

# [est の 输入 输出和出入](/)

* [Home](/)
* [RSS](https://feeds.feedburner.com/initiative)
* [About](/about)
* [Category](/category)

## LLM的弱点——不能很好的讲笑话？

Posted 2026-01-01 | stderr

元旦节长途旅行，无聊，我问娃，AI的弱点是什么？答：没有嗅觉，这个的确是物理限制，我之前吹的。哈哈哈。AI has no taste 🤣

这一路为了打法无聊时光，下载安装了个豆包闲聊。娃很喜欢猜历史人物。我发现一个规律，豆包似乎在缩小3、4次范围之后，就会进入 “你想的是不是XXX” 这种点菜名儿模式，而不是想办法进一步缩小范围。

这种模式如果是猜常见人物，那么特别准。如果是猜冷门、模糊的的，就容易 miss。

AI这种 hit or miss，让我不禁联系到不知道哪里看到过一个大佬的说法，现阶段LLM有个弱点，训练的时候给 next token “回答正确” 的单一结果给的权重太高，比如让AI讲一个 joke，它便老是翻来覆去讲那一个最擅长最经典的，就像是背题背出幻觉了。

不能很好的讲笑话，难道是LLM的终极弱点？

让真人马上讲个笑话，如果一时半会儿想不到好的joke，会随便找个现成的凑数。AI我试了 gemini，chatgpt，deepseek都很烂。不好笑。或许是有的笑话是英语硬翻译过来丢了东西，也可能是我没接触到更好的AI，或者我 prompt 不会写，甚至SOTA已经解决了这个问题？

如果没解决，我们是不是可以说，LLM往往对于有明确答案的东西能一发入魂；对于有多个能接受答案的东西明显变化度不足，甚至给个很差的答案。

我也不知道这是为什么。但是反之 LLM 可以很到位的解释 joke 的笑点在哪里，它不能搜集这些经典笑话然后下一次需要用到的时候复述一遍。或许 transformer 就不是拿来干这个的，毕竟它是 decoder-only的。是不是应该交给RAG做这个事？

我感觉现在的 reasoning 模型把这个问题变得更严重了。推理的时候想得太多。 Reinforcement Learning from Verifiable Rewards (RLVR) 是不是导致轻松愉快随便讲个笑话变得更痛苦严肃？

或许 joke 这事本来就是逆智的东西。弱智吧的笑话是最让我想笑的。AI聪明过头了找不到好笑的，随便打发个回答。

有研究甚至说 [AI 压根不懂 pun](https://arxiv.org/abs/2509.12158)。比如

> I used to be a comedian, but my life became a joke

把最后的 joke 换成 chaotic，AI依然觉得好笑。哈哈，这是词向量近似了吧 🤣

还有人说AI讲不好笑话是因为，[AI会刻意避免意外、惊喜行为](https://danfabulich.medium.com/llms-tell-bad-jokes-because-they-avoid-surprises-7f111aac4f96)。作者说笑话的本质是一个不对劲的东西，然后回头看有搞怪的地方——a joke is surprising, but inevitable in hindsight。如果回头看没看明白，那么就是个烂梗。如果一个人看了太多梗，那么TA笑点就很高，作者以此推断出，没有“最牛笑话”。笑话总是在没见过的人眼里最好笑。AI的训练就陷入怪圈：训练得越多，什么笑话都见过了，越不好笑，笑点就越奇怪。这个问题不是加大训练力度能解决的。作者同时推断，这也是AI没法编一个好的故事情节的原因。

对于这个说法，[HN有评论](https://news.ycombinator.com/item?id=44932170) 说 Daniel Dennett 有本讲《Inside Jokes》书，幽默是心灵的逆向工程。

> humor depends on errors in reasoning and the punchline causes you to reevaluate your reasoning and discover your error. Humor evolved to be enjoyable to encourage the discovery of errors.

这个说法感觉有点特别倾向老外式的 joke。比如 “程序员为什么老是搞混万圣节和圣诞节？因为 Oct 31 == Dec 25” 这类，我感觉更多的是 幽默。国内的笑话我感觉更无限逼近“地狱笑话”那种段子，要么是看别人倒霉，或者是屎尿系列的 butt joke 。

---

除了讲笑话的问题，还有最老生常谈的LLM幻觉问题。猜成语我想了个冷门的「投杼逾墙」，AI懵逼。btw 这个成语也是 也是卢格杜努姆的奥古斯丁 这个up主之前某一期爆的典。

然后又看到 [Steve Yegge 的访谈](https://mp.weixin.qq.com/s/k147AyWj01W-V7IpkV0g2A)。的确现在 agent 生产会导致人和人的差距越来越大。我在想LLM似乎就很适合编程这种有规律可循，答案又很确定，很多写法都是 there should be one– and preferably only one –obvious way to do it，太适合 LLM 了。

新年第一天，杂七杂八胡思乱想一篇。看了眼已经 01:37 了 😂

## Comments