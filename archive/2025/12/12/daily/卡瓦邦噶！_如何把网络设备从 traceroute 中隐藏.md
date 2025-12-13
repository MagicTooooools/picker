---
title: 如何把网络设备从 traceroute 中隐藏
url: https://www.kawabangga.com/posts/7129
source: 卡瓦邦噶！
date: 2025-12-12
fetch_date: 2025-12-13T03:20:58.841989
---

# 如何把网络设备从 traceroute 中隐藏

顶部菜单

* [所有文章](https://www.kawabangga.com/all-posts)
* [演讲](https://www.kawabangga.com/talks)
* [Join Shopee & Work with Me!](https://www.kawabangga.com/join-shopee-work-with-me)

[卡瓦邦噶！](https://www.kawabangga.com/ "卡瓦邦噶！")

无法自制的人得不到自由。

[![](https://www.kawabangga.com/wp-content/themes/heatmap-adaptive/images/twitter.png)](https://twitter.com/laixintao)[![](https://www.kawabangga.com/wp-content/themes/heatmap-adaptive/images/rss-feed.png)](https://www.kawabangga.com/feed)[![](https://www.kawabangga.com/wp-content/themes/heatmap-adaptive/images/rss-comments.png)](https://www.kawabangga.com/comments/feed)

topics

* [Vim系列教程](https://www.kawabangga.com/vim%E7%B3%BB%E5%88%97)
* [如何学Python？](https://www.kawabangga.com/how-to-learn-python)
* [珍藏资料](https://www.kawabangga.com/collection)
* [DB资料集](https://www.kawabangga.com/db)
* [计算机网络实用技术](https://www.kawabangga.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E5%AE%9E%E7%94%A8%E6%8A%80%E6%9C%AF)

# [如何把网络设备从 traceroute 中隐藏](https://www.kawabangga.com/posts/7129 "Permalink to 如何把网络设备从 traceroute 中隐藏")

Posted on [2025年12月12日](https://www.kawabangga.com/posts/7129 "11:44") by [laixintao](https://www.kawabangga.com/posts/author/laixintao "View all posts by laixintao") [7 Comments](https://www.kawabangga.com/posts/7129#comments)

在和朋友一起吃饭的时候，A 提了一个有意思的问题：怎样可以把一个机房内的路由设备从互联网「隐藏」呢？

「隐藏就是没有人可以知道这个设备的 IP 地址，这还不简单，只要禁用 ICMP 就可以了」， B说。

A 说，这样是可以。很多安全团队在实施起来也确实是这么做的，但是这样并不好：机房内所有的 IP 都无法 ping 通了。这样会增加 debug 的难度，得不偿失呀！

C 说，那就依然转发 ICMP 包，但是如果是 TTL=1 的包，就不要回复 `ICMP Time Exceeded` 了。

B 说，人家要的是「隐藏」，要是像你说的这么做，别人还是知道中间有一个设备的存在，没有完全符合要求。

[![](https://www.kawabangga.com/wp-content/uploads/2025/12/traceroute-with-star-1024x333.png)](https://www.kawabangga.com/wp-content/uploads/2025/12/traceroute-with-star.png)

一个 traceroute 的例子：第7跳直接丢弃 TTL=1 的包，不返回错误[1](#718aff14-8a3f-4764-8151-0ca3dc2665c1)

事实是这样的。假设一个简单的物理拓扑是 A -> B -> C，B 不回复 `ICMP Time Exceeded`，那么 traceroute 看起来就是 A ? C，可以猜测得到中间有一个路由器，但是已经禁止回复 `ICMP Time Exceeded`。看起来像下面这样。

C 说，traceroute 的原理是发送 TTL=1, 2, 3, … 的包，不断让路由器回复 `ICMP Time Exceeded` 信息，来得到每一跳的 IP 地址。要想完全隐藏，只需要：

* 自己不回复 `ICMP Time Exceeded`；
* 让下一跳回复，仿佛下一跳就在自己的位置；

这样就可以完全隐藏了。要达到这个目的，只需要：

* 对于 TTL=1 的包，不是丢弃，而是转发给下一跳，并且 TTL 依然保持为 1，即可。`A ---[TTL=1]---> B ---[TTL=1]---> C`， 对于客户端的 traceroute，看起来就像：A → C。

这样（理论上）好像确实可行了。三人对这个结论满意了。

后来我把这个讨论记录在了博客上（你现在正在阅读的一个），[一位读者](https://www.kawabangga.com/posts/7129#comment-64237)马上就发现了问题：可是这样 C 会出现两次吧！

确实是这样，假设在 A -> B -> C 的链路中：

* TTL = 1 从 A 进入的时候，A 会在 ICMP 中回复自己的 IP；
* TTL = 2 从 A 进入的时候，B 会直接转发给 C，C 会在 ICMP 中回复自己的 IP；
* TTL = 3 从 A 进入的时候，B 会 TTL -1 转发给 C，C 会在 ICMP 中回复自己的 IP；

这样 C 就出现了 2 次！

看来，B 必须完全不减 TTL，直接转发，才能隐藏自己。不过这样就有出现环路[2](#7be59f1e-4a79-4af8-b1ad-39c1e24b268b)的风险了。

1. [使用 mtr 检查网络问题，以及注意事项](https://www.kawabangga.com/posts/4275) [↩︎](#718aff14-8a3f-4764-8151-0ca3dc2665c1-link)
2. [网络中的环路和防环技术](https://www.kawabangga.com/posts/6291) [↩︎](#7be59f1e-4a79-4af8-b1ad-39c1e24b268b-link)

---

### 相关文章:

* [探测 TCP 乱序问题](https://www.kawabangga.com/posts/5876)
* [数据是如何转发的](https://www.kawabangga.com/posts/6919)
* [使用 mtr 检查网络问题，以及注意事项](https://www.kawabangga.com/posts/4275)
* [用 LD\_PRELOAD 写魔法程序](https://www.kawabangga.com/posts/6825)
* [数据中心网络高可用技术：ECMP](https://www.kawabangga.com/posts/6732)
* [由 ICMP Redirect 消息引起的丢包问题排查](https://www.kawabangga.com/posts/6751)
* [Linux interface Vlan 和 Bond 配置错误问题排查](https://www.kawabangga.com/posts/6767)
* [真实世界中的 PMTUD](https://www.kawabangga.com/posts/5816)
* [网络抓包的技巧](https://www.kawabangga.com/posts/6950)
* [网络中的环路和防环技术](https://www.kawabangga.com/posts/6291)
* [数据中心网络高可用技术之从服务器到网关：首跳冗余协议 VRRP](https://www.kawabangga.com/posts/6594)
* [有关 MTU 和 MSS 的一切](https://www.kawabangga.com/posts/4983)
* [pwru 工具介绍和案例一则](https://www.kawabangga.com/posts/6879)
* [理解网络的分层模型](https://www.kawabangga.com/posts/6295)
* [MTU 和 UDP (以及基于 UDP 的协议)](https://www.kawabangga.com/posts/5160)

Categories: [网络](https://www.kawabangga.com/posts/category/%E7%BD%91%E7%BB%9C)

Tags: [icmp](https://www.kawabangga.com/posts/tag/icmp), [ICMP Time Exceeded](https://www.kawabangga.com/posts/tag/icmp-time-exceeded), [traceroute](https://www.kawabangga.com/posts/tag/traceroute), [TTL](https://www.kawabangga.com/posts/tag/ttl), [安全策略](https://www.kawabangga.com/posts/tag/%E5%AE%89%E5%85%A8%E7%AD%96%E7%95%A5), [网络测量](https://www.kawabangga.com/posts/tag/%E7%BD%91%E7%BB%9C%E6%B5%8B%E9%87%8F), [调试可观测性](https://www.kawabangga.com/posts/tag/%E8%B0%83%E8%AF%95%E5%8F%AF%E8%A7%82%E6%B5%8B%E6%80%A7), [路由器隐藏](https://www.kawabangga.com/posts/tag/%E8%B7%AF%E7%94%B1%E5%99%A8%E9%9A%90%E8%97%8F), [跳数（hop）](https://www.kawabangga.com/posts/tag/%E8%B7%B3%E6%95%B0%EF%BC%88hop%EF%BC%89), [转发行为](https://www.kawabangga.com/posts/tag/%E8%BD%AC%E5%8F%91%E8%A1%8C%E4%B8%BA)

## “如何把网络设备从 traceroute 中隐藏”已经有7条评论

1. [Reply](https://www.kawabangga.com/posts/7129?replytocom=64237#respond)

   ![](https://secure.gravatar.com/avatar/4dd86ca2947bb6b50120bc497e4be5e9966174d8b77bac93fc6104a4bea27b89?s=68&d=wavatar&r=g)Clicker on [2025年12月12日 at 15:23](https://www.kawabangga.com/posts/7129#comment-64237) said:

   可是这样 C 会出现两次吧

   * [Reply](https://www.kawabangga.com/posts/7129?replytocom=64238#respond)

     ![](https://secure.gravatar.com/avatar/5e100142dcfc57f58c7373f3657c2ac9c35e1ae6ded24fce8c9b0960ad4f75b2?s=39&d=wavatar&r=g)[laixintao](https://www.kawabangga.com) on [2025年12月12日 at 15:30](https://www.kawabangga.com/posts/7129#comment-64238) said:

     说的对哦，那是不是全部都不减 TTL 直接转发，就可以了？

     + [Reply](https://www.kawabangga.com/posts/7129?replytocom=64239#respond)

       ![](https://secure.gravatar.com/avatar/f49d6c941589fb88e0ecb661ac768510838c0cddb918b41ae065e54bb7af3c24?s=39&d=wavatar&r=g)[依云](https://blog.lilydjwg.me/) on [2025年12月12日 at 15:55](https://www.kawabangga.com/posts/7129#comment-64239) said:

       小心打环哦（

       - [Reply](https://www.kawabangga.com/posts/7129?replytocom=64241#respond)

         ![](https://secure.gravatar.com/avatar/5e100142dcfc57f58c7373f3657c2ac9c35e1ae6ded24fce8c9b0960ad4f75b2?s=39&d=wavatar&r=g)[laixintao](https://www.kawabangga.com) on [2025年12月12日 at 15:58](https://www.kawabangga.com/posts/7129#comment-64241) said:

         是有风险，不过严格只有一台设备这么做的话是没有风险的。
     + [Reply](https://www.kawabangga.com/posts/7129?replytocom=64240#respond)

       ![](https://secure.gravatar.com/avatar/4dd86ca2947bb6b50120bc497e4be5e9966174d8b77bac93fc6104a4bea27b89?s=39&d=wavatar&r=g)Clicker on [2025年12月12日 at 15:57](https://www.kawabangga.com/posts/7129#comment-64240) said:

       好像可以在网络都正常的情况，不减 TTL 直接转发好像就可以隐藏了

       - [Reply](https://www.kawabangga.com/posts/7129?replytocom=64242#respond)

         ![](https://secure.gravatar.com/avatar/5e100142dcfc57f58c7373f3657c2ac9c35e1ae6ded24fce8c9b0960ad4f75b2?s=39&d=wavatar&r=g)[laixintao](https://www.kawabangga.com) on [2025年12月12日 at 15:59](https://www.kawabangga.com/posts/7129#comment-64242) said:

         谢谢，我更新一下原文。
       - [Reply](https://www.kawabangga.com/posts/7129?replytocom=64243#respond)

         ![](https://secure.gravatar.com/avatar/5e100142dcfc57f58c7373f3657c2ac9c35e1ae6ded24fce8c9b0960ad4f75b2?s=39&d=wavatar&r=g)[laixintao](https://www.kawabangga.com) on [2025年12月12日 at 16:04](https://www.kawabangga.com/posts/7129#comment-64243) said:

         原文已更新，现在应该没有问题了。

### Leave a comment [取消回复](/posts/7129#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

[ ]  在此浏览器中保存我的显示名称、邮箱地址和网站地址，以便下次评论时使用。

[x] 如果有人回复我的评论，请通过电子邮件通知我。

Δ

[« 博客维护：升级到 Ubuntu 24.04](https://www.kawabangga.com/posts/7124)

[AoC 2025 通关留念 »](https://www.kawabangga.com/posts/7138)

搜索：

#### 我的电邮

![](https://www.kawabangga.com/wp-content/uploads/2019/09/laixintaoo-gmail-address.png)

2025 年 12 月

| 一 | 二 | 三 | 四 | 五 | 六 | 日 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 2 | [3](https://www.kawabangga.com/posts/date/2025/12/03) | 4 | 5 | [6](https://www.kawabangga.com/posts/date/2025/12/06) | 7 |
| 8 | 9 | [10](https://www.kawabangga.com/posts/date/2025/12/10) | 11 | [12](https://www.kawabangga.com/posts/date/2025...