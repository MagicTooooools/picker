---
title: Harbor GC 问题
url: https://www.kawabangga.com/posts/7057
source: 卡瓦邦噶！
date: 2025-12-06
fetch_date: 2025-12-07T03:28:25.302789
---

# Harbor GC 问题

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

# [Harbor GC 问题](https://www.kawabangga.com/posts/7057 "Permalink to Harbor GC 问题")

Posted on [2025年12月6日](https://www.kawabangga.com/posts/7057 "16:01") by [laixintao](https://www.kawabangga.com/posts/author/laixintao "View all posts by laixintao") [3 Comments](https://www.kawabangga.com/posts/7057#comments)

最近的工作比较忙，以至于网络技术的系列文章[1](#c61cff25-a29c-49cf-946d-3a1982884477)许久不更新了。这几天在解决的问题是镜像存储服务 Harbor[2](#010810f1-f122-4354-b067-8fd76cdc8e0d)，存储的 docker image 太多了。

虽然我之前在博客里面分享了一些 Docker image 构建的技巧[3](#b99aceba-0f4d-443b-b301-d1d41d454561)，以及炫耀了构建一个最小的 Redis Docker 镜像才不到 2MiB[4](#a8e50e14-7f6b-4467-ba06-9f9a702b796e)，但是无奈，我的博客基本没有人看，所以同事上传的 image 都非常可怕，动辄就上 G，20+ GiB 的都有。现在我们的 Harbor 存储已经是 PiB 级别了。

多余的 image 就删除就好了，问题就在于，删除 image 比较复杂。分成几个步骤：

1. 删除 image 的 Tag[5](#9d7b8485-e126-4273-ba4e-73a886c0f911)；
2. 扫描整个数据库，找到没有被任何其他 image 和 tag 引用的 blob；
3. 删除这些未被引用的 blob；

第二步尤其重要，简单来说，image 是分层的，一层就是一个 blob，一个 image 可以引用多个 blob。比如服务 A 的 Dockerfile 开头是 `From: ubuntu:24.04`，另一个服务 B 的 Dockerfile 开头也是 `From: ubuntu:24.04`，那么这两个 image 都是引用了 ubuntu 的 blob。删除服务 A 的 image 的时候，不能把 A 的 blob 都删除，因为这样的话 ubuntu 的 base image 就连带被删除了。所以我们在删除一个 image 的时候，其实并没有释放任何空间，而只是**删除了 image 对 blob 的引用**。这时候还不知道哪些 blob 是可以释放的，要知道哪些 blob 可以删除，就必须扫描全部的数据库，找到没有任何引用的 blob，才可以删除。难题就在扫全表这里。

这个问题就和编程语言的 GC 问题很像，不过更加简单一些，因为引用只存在于 tag 到 blob，tag 之间和 blob 之间不存在引用，也就没有环的问题。

## 引用计数

引用计数比较合适这个场景，因为没有环路，所以引用计数到 0 就可以直接删除，不需要扫表找孤零零的环。但是 Harbor 本身没有用这种方案，估计是因为引用记录维护起来比较难，必须准确并且处理好并发，处理不当很容易有数据误删或者出现永久的垃圾。

## Mark and Sweep

这是官方的代码采用的方案，基本思路是，扫描所有的 image，对它们引用的 blob 标记为在使用中。扫描完成之后，所有从未被标记过的 blob 直接删除。

## 问题

如果直接用 Harbor 的 GC 方案，那么运行一次 GC 需要超过一个月的运行时间（不知道具体需要多久，因为从来没有成功跑完过）。之前的负责人设计了一个很聪明的方案，基本思路是，找到系统性能低的瓶颈，然后针对性地处理这些瓶颈。

对于前面的 3 个步骤：

1. 删除 image 的 Tag：直接用 SQL 从数据库查询出来 image，判断是否需要保留（规则是每一个 image 只保留最近的 3 个版本），如果不需要保留，通过 API 删除；
2. 扫描整个数据库，找到没有被任何其他 image 和 tag 引用的 blob：这一步因为是 Harbor 代码的 GC 逻辑，比较负载，还是通过 web UI 来触发的；
3. 删除这些未被引用的 blob：Harbor 本身 sweep 的过程很慢，原因是没有并发，一个一个删除的，改进是直接通过并发删除。

这样，整体运行一次只需要一个月。

目前还是存在很多问题。我接受之后又做了一些改进：

1. 之前的 PIC 显然是一个脚本大师，所有的工作都是通过 bash，awk，curl 这些工具完成的，每一步都需要人工操作 -> 等待完成 -> 人工操作下一步，比如到 mark and sweep 的这一步，需要人工去页面上触发 GC，然后关注执行的进度，在执行到 sweep 阶段的时候手动结束，开始运行下一步的脚本；我写了一个 300 多行的 Python 脚本，把所有的步骤串起来，这样就有了 crontab 定期执行的条件。
2. 在第一步删除 image 的时候还是很慢，30s 只能删除一个 image，我们有千万个 image。解决办法是读了 harbor 的代码，发现 `blobMgr.CleanupAssociationsForProject` 这一步其实是最费时间且多余的，后面执行 GC mark 的时候一定会运行一遍。删除这个逻辑之后只需要 0.1s 就可以删除一个 image；
3. 最后一步通过 API 删除 S3 上的数据，之前还是脚本用 curl 触发，速度太慢。使用 Python 之后就可以用 connection pool 并发删除了；
4. 还做了其他的功能，比如支持不同的 project 自定义删除逻辑，「删除最近1年没有 pull 记录的 image」这种。

本质上是用最少的改动自动化原来的 GC 逻辑，目前运行一次的时间是 3 天。已经足够满足需求了，因为不需要人工执行，所以 3天和 3 个小时区别不大。

上一个负责人留下的文档详细记录的 Harbor GC 的逻辑以及改进点，比 Harbor 官方的文档还要详细。有了这些我半重写 GC 的逻辑就简单很多。

在他之前，是另一个负责 Harbor 的同事。阅读代码并找到瓶颈是需要很大的勇气的，且不一定行得通，可能花了很大的力气，最后发现这个事情做起来就只能这么慢。

但是问题还是要解决。所以他那时用了另一个有意思的方案：

1. 搭建另一套一模一样的 Harbor 集群，复制以前的用户名，权限，project 等数据，但是把 blob 和 image 数据删除；
2. 搭建一套 Nginx 代理，Nginx 转发逻辑是：
   * 对于 push，转发到新集群；
   * 对于 pull，先 pull 新集群，如果得到 404，就转发到老的集群，这样以前的数据都可以读；
3. 在 1年之后，完全删除老的集群；

这是一个很有意思的「用运维手段解决技术问题」的例子，在 SRE 的工作中，迫于没有对软件的实现的控制力，我们经常需要用运维手段来解决代码实现上的问题。

1. [计算机网络实用技术](https://www.kawabangga.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E5%AE%9E%E7%94%A8%E6%8A%80%E6%9C%AF) [↩︎](#c61cff25-a29c-49cf-946d-3a1982884477-link)
2. <https://goharbor.io/> [↩︎](#010810f1-f122-4354-b067-8fd76cdc8e0d-link)
3. [Docker 镜像构建的一些技巧](https://www.kawabangga.com/posts/4676) [↩︎](#b99aceba-0f4d-443b-b301-d1d41d454561-link)
4. [Build 一个最小的 Redis Docker Image](https://www.kawabangga.com/posts/4411) [↩︎](#a8e50e14-7f6b-4467-ba06-9f9a702b796e-link)
5. <https://docs.docker.com/reference/cli/docker/image/tag/> [↩︎](#9d7b8485-e126-4273-ba4e-73a886c0f911-link)

---

### 相关文章:

* [SRE&Devops 每周分享 Issue #5](https://www.kawabangga.com/posts/3334)
* [Build 一个最小的 Redis Docker Image](https://www.kawabangga.com/posts/4411)
* [Spegel 镜像分发介绍](https://www.kawabangga.com/posts/6889)
* [SRE&Devops 每周分享 Issue #4 AWS Layer](https://www.kawabangga.com/posts/3311)
* [SRE 线上操作指南](https://www.kawabangga.com/posts/5452)
* [SRE 书单推荐](https://www.kawabangga.com/posts/6269)
* [玩了一下 Github 个人首页的 Profile （使用 Action 自动更新）](https://www.kawabangga.com/posts/4117)
* [没来的请举手](https://www.kawabangga.com/posts/4847)
* [连接池中的连接失效的几种处理方案](https://www.kawabangga.com/posts/3445)
* [最近的工作感悟](https://www.kawabangga.com/posts/4294)
* [用 Nginx 在公网上搭建加密数据通道](https://www.kawabangga.com/posts/4649)

Categories: [SRE](https://www.kawabangga.com/posts/category/sre)

Tags: [blob](https://www.kawabangga.com/posts/tag/blob), [CleanupAssociationsForProject](https://www.kawabangga.com/posts/tag/cleanupassociationsforproject), [crontab](https://www.kawabangga.com/posts/tag/crontab), [docker image](https://www.kawabangga.com/posts/tag/docker-image), [GC 性能瓶颈](https://www.kawabangga.com/posts/tag/gc-%E6%80%A7%E8%83%BD%E7%93%B6%E9%A2%88), [Harbor GC](https://www.kawabangga.com/posts/tag/harbor-gc), [Harbor 代码优化](https://www.kawabangga.com/posts/tag/harbor-%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%96), [Mark and Sweep](https://www.kawabangga.com/posts/tag/mark-and-sweep), [Nginx 代理转发](https://www.kawabangga.com/posts/tag/nginx-%E4%BB%A3%E7%90%86%E8%BD%AC%E5%8F%91), [project 清理策略](https://www.kawabangga.com/posts/tag/project-%E6%B8%85%E7%90%86%E7%AD%96%E7%95%A5), [push/pull 双集群策略](https://www.kawabangga.com/posts/tag/push-pull-%E5%8F%8C%E9%9B%86%E7%BE%A4%E7%AD%96%E7%95%A5), [Python 自动化脚本](https://www.kawabangga.com/posts/tag/python-%E8%87%AA%E5%8A%A8%E5%8C%96%E8%84%9A%E6%9C%AC), [S3 并发删除](https://www.kawabangga.com/posts/tag/s3-%E5%B9%B6%E5%8F%91%E5%88%A0%E9%99%A4), [全表扫描](https://www.kawabangga.com/posts/tag/%E5%85%A8%E8%A1%A8%E6%89%AB%E6%8F%8F), [删除 Tag](https://www.kawabangga.com/posts/tag/%E5%88%A0%E9%99%A4-tag), [存储清理](https://www.kawabangga.com/posts/tag/%E5%AD%98%E5%82%A8%E6%B8%85%E7%90%86), [并发删除](https://www.kawabangga.com/posts/tag/%E5%B9%B6%E5%8F%91%E5%88%A0%E9%99%A4), [引用计数](https://www.kawabangga.com/posts/tag/%E5%BC%95%E7%94%A8%E8%AE%A1%E6%95%B0), [运维迁移方案](https://www.kawabangga.com/posts/tag/%E8%BF%90%E7%BB%B4%E8%BF%81%E7%A7%BB%E6%96%B9%E6%A1%88)

## “Harbor GC 问题”已经有3条评论

1. [Reply](https://www.kawabangga.com/posts/7057?replytocom=64028#respond)

   ![](https://secure.gravatar.com/avatar/f21602ec5041ebc298c6f9d688b2e50bbaf3672a846c963cbdff2aa8224d39ef?s=68&d=wavatar&r=g)[苏三州](https://sanzhou.live) on [2025年12月6日 at 16:35](https://www.kawabangga.com/posts/7057#comment-64028) said:

   好奇是什么公司

   * [Reply](https://www.kawabangga.com/posts/7057?replytocom=64046#respond)

     ![](https://secure.gravatar.com/avatar/8155512368f18e3dbdc5be4a03299e6c41d645c70da42d84bf383879786b2aa6?s=39&d=wavatar&r=g)richman on [2025年12月6日 at 23:56](https://www.kawabangga.com/posts/7057#comment-64046) said:

     shopee
2. [Reply](https://www.kawabangga.com/posts/7057?replytocom=64031#respond)

   ![](https://secure.gravatar.com/avatar/360b3def6a204e1e7315f226a336f0a7ad5122e1b847c9ac414320184fc66120?s=68&d=wavatar&r=g)zhiqli on [2025年12月6日 at 18:05](https://www.kawabangga.com/posts/705...