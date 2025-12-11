---
title: 博客维护：升级到 Ubuntu 24.04
url: https://www.kawabangga.com/posts/7124
source: 卡瓦邦噶！
date: 2025-12-10
fetch_date: 2025-12-11T03:28:10.338980
---

# 博客维护：升级到 Ubuntu 24.04

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

# [博客维护：升级到 Ubuntu 24.04](https://www.kawabangga.com/posts/7124 "Permalink to 博客维护：升级到 Ubuntu 24.04")

Posted on [2025年12月10日](https://www.kawabangga.com/posts/7124 "23:12") by [laixintao](https://www.kawabangga.com/posts/author/laixintao "View all posts by laixintao") [7 Comments](https://www.kawabangga.com/posts/7124#comments)

这个博客在四年前做了一次迁移[1](#29dc3471-9f20-4c75-8687-95171be845fc)，目前是架设在一台 DigitalOcean 的 VPS 上，前面用 Cloudflare 作 CDN，得益于我选择的这两家公司非常靠谱（但是最近 Cloudflare 的事故[2](#91c1e860-49f2-4cd3-8edf-82a8b54f2c84) [3](#79e8491d-7b64-4a15-afe5-6926ada3f044)让我这个 CF 吹有一些尴尬），自从搭建起来之后，我几乎没有维护过。

[![](https://www.kawabangga.com/wp-content/uploads/2025/12/running-time.png)](https://www.kawabangga.com/wp-content/uploads/2025/12/running-time.png)

最近后台一直提示我 PHP7.4 EOL 了，今天终于打起精神来决定升级一把。

一不做而不休，干脆直接把 4 年半前启动的这台 VPS 全升了吧：

* Ubuntu 升级到 24.04；
* Mysql 升级到 8.0；
* PHP 升级到 8.3；

因为我是专业的 SRE，所以这次升级读者感受不到任何区别。

整体比较顺利，唯一遇到的问题是，我用的 wordpress theme 太老了，一样的代码居然在 PHP8.3 挂了。看提示是函数参数少传了，在 PHP7.4 是 Warning，在 PHP8.4 是直接 Fatal。

本来打算放弃治疗，直接用一个新的 wordpress 官方主题得了，省心。结果一个二〇二五这些主题，都是什么玩意，行距看着都难受。又回来决定修好主题的代码。得亏 ChatGPT，没想到意外地顺利，很快就跑起来了，目前也没发现什么问题。

安全方面上顺便做了一个加强，以前的架构是：域名解析到 Cloudflare，Cloudflare proxy 到我的 Nginx，Nginx 只接受 Cloudflare 的 IP[4](#1bc019b8-c7fc-4583-a35a-1f063a1cb2e7)，其他的一概拒绝。自己以为很安全了，没有人知道我的真实 IP。然后自己一查，居然早已经暴露了。

[![](https://www.kawabangga.com/wp-content/uploads/2025/12/censys-domain-record-1024x298.png)](https://www.kawabangga.com/wp-content/uploads/2025/12/censys-domain-record.png)

[censys](https://search.censys.io/search?resource=hosts&sort=RELEVANCE&per_page=25&virtual_hosts=EXCLUDE&q=kawabangga.com) 的查询结果

我也不知道什么时候暴露的。

这次直接用了 [cloudflared](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/get-started/create-remote-tunnel/)，原理是，我的服务器 Nginx 只 listen localhost 的端口，我的服务器安装一个 cloudflared，cloudflared 会去主动连接 cloudflare，这样，在 cloudflare 收到请求的时候，会通过 –> cloudflared –> nginx 转发到我的机器上。有点像 FRP[5](#9b5549be-b9cd-4c76-bfd5-09d8c56ca94a) 穿透。如此一来，我的 IP 完全没有暴露在公网上了。过段时间再去搜索一下，看暴露了没有。

欢迎读者留言 ; D

1. [博客迁移到 Cloudflare](https://www.kawabangga.com/posts/4331) [↩︎](#29dc3471-9f20-4c75-8687-95171be845fc-link)
2. [Cloudflare outage on November 18, 2025](https://blog.cloudflare.com/18-november-2025-outage/) [↩︎](#91c1e860-49f2-4cd3-8edf-82a8b54f2c84-link)
3. [Cloudflare outage on December 5, 2025](https://blog.cloudflare.com/5-december-2025-outage/) [↩︎](#79e8491d-7b64-4a15-afe5-6926ada3f044-link)
4. Cloudflare 的 IP range：<https://www.cloudflare.com/ips/> [↩︎](#1bc019b8-c7fc-4583-a35a-1f063a1cb2e7-link)
5. <https://github.com/fatedier/frp> [↩︎](#9b5549be-b9cd-4c76-bfd5-09d8c56ca94a-link)

---

### 相关文章:

* [博客迁移到 Cloudflare](https://www.kawabangga.com/posts/4331)
* [博客维护：速度优化，嵌入instagram](https://www.kawabangga.com/posts/2139)

Categories: [博客维护](https://www.kawabangga.com/posts/category/about-this-blog)

## “博客维护：升级到 Ubuntu 24.04”已经有7条评论

1. [Reply](https://www.kawabangga.com/posts/7124?replytocom=64202#respond)

   ![](https://secure.gravatar.com/avatar/77224a4b28f4c79e21bbb50c08857ce18abdc8bcc55e0fd8118466017f5152ff?s=68&d=wavatar&r=g)[Frost](https://frostming.com) on [2025年12月11日 at 09:09](https://www.kawabangga.com/posts/7124#comment-64202) said:

   >因为我是专业的 SRE，所以这次升级读者感受不到任何区别。

   牛逼就完事了。
2. [Reply](https://www.kawabangga.com/posts/7124?replytocom=64203#respond)

   ![](https://secure.gravatar.com/avatar/0fe2d1e3c7530d97ad6fd854725a28b0e1c4d12f6e84db30c00e52f96fa7f1e0?s=68&d=wavatar&r=g)Seunji on [2025年12月11日 at 09:18](https://www.kawabangga.com/posts/7124#comment-64203) said:

   用 cloudflared 会影响 certbot 自动申请证书吗

   * [Reply](https://www.kawabangga.com/posts/7124?replytocom=64205#respond)

     ![](https://secure.gravatar.com/avatar/5e100142dcfc57f58c7373f3657c2ac9c35e1ae6ded24fce8c9b0960ad4f75b2?s=39&d=wavatar&r=g)[laixintao](https://www.kawabangga.com) on [2025年12月11日 at 09:54](https://www.kawabangga.com/posts/7124#comment-64205) said:

     应该不会，cloudflared 只是帮你代理单个服务，其他的东西该怎样还是怎样的。

     不过都用 cloudflare 了，直接让 cloudflare 负责证书不就好了吗？
3. [Reply](https://www.kawabangga.com/posts/7124?replytocom=64207#respond)

   ![](https://secure.gravatar.com/avatar/2945ba681b94c0cd18683cbd6b61853b0dc23abadd27b6d6ce0be893f58710aa?s=68&d=wavatar&r=g)[daya](http://changchen.me) on [2025年12月11日 at 10:12](https://www.kawabangga.com/posts/7124#comment-64207) said:

   都用 tunnel 了，是不是可以直接把博客放家里了。

   * [Reply](https://www.kawabangga.com/posts/7124?replytocom=64211#respond)

     ![](https://secure.gravatar.com/avatar/5e100142dcfc57f58c7373f3657c2ac9c35e1ae6ded24fce8c9b0960ad4f75b2?s=39&d=wavatar&r=g)[laixintao](https://www.kawabangga.com) on [2025年12月11日 at 11:03](https://www.kawabangga.com/posts/7124#comment-64211) said:

     理论上是，但是家庭里没有稳定的网和电，没有连续让我的主机开机 5 年的条件。
4. [Reply](https://www.kawabangga.com/posts/7124?replytocom=64208#respond)

   ![](https://secure.gravatar.com/avatar/a2faa05a25e7ce0fcc073db36d4b12753d924d6b112ed4b2f07cff9385ad129e?s=68&d=wavatar&r=g)[druggo](https://blog.druggo.org) on [2025年12月11日 at 10:16](https://www.kawabangga.com/posts/7124#comment-64208) said:

   可能是IP反向解析记录暴露的：

   * [Reply](https://www.kawabangga.com/posts/7124?replytocom=64210#respond)

     ![](https://secure.gravatar.com/avatar/5e100142dcfc57f58c7373f3657c2ac9c35e1ae6ded24fce8c9b0960ad4f75b2?s=39&d=wavatar&r=g)[laixintao](https://www.kawabangga.com) on [2025年12月11日 at 11:03](https://www.kawabangga.com/posts/7124#comment-64210) said:

     哈哈，还真是，我 trace 了一下是 DO 来解析的。

     87.246.230.157.in-addr.arpa. 1800 IN PTR kawabangga.com.
     ;; Received 84 bytes from 172.64.49.209#53(ns3.digitalocean.com) in 22 ms

     |  |  |
     | --- | --- |
     | 1  2 | 87.246.230.157.in-addr.arpa. 1800 IN    PTR     kawabangga.com.  ;; Received 84 bytes from 172.64.49.209#53(ns3.digitalocean.com) in 22 ms |

     然后看了文档 <https://docs.digitalocean.com/products/networking/dns/how-to/manage-records/#ptr-rdns-records> ：

     > We automatically create PTR records for Droplets based on the name you give that Droplet in the control panel. The name must be a valid FQDN. For example, using example.com as the Droplet name creates a PTR record, but ubuntu-s-4vcpu-8gb-fra1-01 or my-droplet does not. Droplets with IPv6 enabled only have PTR records enabled for the first IPv6 address assigned to it, not to all 16 addresses available.

     ![PTR records](https://www.kawabangga.com/wp-content/uploads/2025/12/PTR-records-kawabangga.png)

### Leave a comment [取消回复](/posts/7124#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

[ ]  在此浏览器中保存我的显示名称、邮箱地址和网站地址，以便下次评论时使用。

[x] 如果有人回复我的评论，请通过电子邮件通知我。

Δ

[« Harbor GC 问题](https://www.kawabangga.com/posts/7057)

搜索：

#### 我的电邮

![](https://www.kawabangga.com/wp-content/uploads/2019/09/laixintaoo-gmail-address.png)

2025 年 12 月

| 一 | 二 | 三 | 四 | 五 | 六 | 日 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 2 | [3](https://www.kawabangga.com/posts/date/2025/12/03) | 4 | 5 | [6](https://www.kawabangga.com/posts/date/2025/12/06) | 7 |
| 8 | 9 | [10](https://www.kawabangga.com/posts/date/2025/12/10) |...