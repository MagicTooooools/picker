---
title: 博客音乐播放器 + 1
url: https://ssshooter.com/blog-music-player/
source: Usubeni Fantasy
date: 2026-01-06
fetch_date: 2026-01-07T03:33:40.257644
---

# 博客音乐播放器 + 1

[skip to content](#main)

[![usubeni fantasy logo](/logo-mobile.png) Usubeni Fantasy](/)    [归档](/archive/)  [标签](/tags/)  [关于](/about/)  [友链](/links/)  [虫洞](https://www.foreverblog.cn/go.html)

Close

     Dark Theme

# 博客音乐播放器 + 1

2026/01/06  / 4 分钟阅读

[项目](/tag/%E9%A1%B9%E7%9B%AE/)

尽管我本来不是想做博客播放器，而是做一个歌词解释器，但是做都做了，突然发现做成大杂烩也不行，于是开干呗~

最后出来结果还不错，接下来简单介绍一下这个开源项目：

* Github 地址：[Elixia Player](https://github.com/SSShooter/elixia-player)
* 直接来试用：<https://elixia-player.koyeb.app/search>

特别鸣谢 [Meting](https://github.com/metowolf/Meting)，没有 Meting 就没有这个项目！

搜歌、看歌词、AI 功能就不在这多说了，下面主要介绍其作为博客音乐播放器的能力，主要是 3 个功能：

* 插入播放卡片
* 插入外链卡片
* 生成图片分享

首先是播放卡片，可以播放插入的歌曲，但前提是**必须提供对应音乐平台的 cookie**。虽然体验非常好，不用跳转直接播放，但对维护者来说就非常麻烦了，需要在部署服务时设置 cookie，并且 cookie 过期的时候需要及时更换，否则无法播放。

举个 QQ 音乐的例子，在登陆 QQ 音乐之后按 F12，打开 Network 找到这个 cookie 在发布时填写，或直接在网页配置页填写都可以~

![](https://img.ssshooter.com/img/elixia-player/qqmusic-cookie.png)

```
<iframe

loading="lazy"

height="80px"

width="100%"

style="border-radius: 15px;"

src="https://elixia-player.koyeb.app/embed/tencent/003cI52o4daJJL"

frameborder="0"

></iframe>
```

填好了再嵌入以上代码，无意外就能看到这样的播放器：

![](https://img.ssshooter.com/img/elixia-player/%E8%8A%B1%E6%B5%B7.png)

接着**外链卡片**，是一种比较折中的方式，也**最推荐的方式**：

![](https://img.ssshooter.com/img/elixia-player/%E6%AC%A7%E8%8B%A5%E6%8B%89.png)

```
<iframe

loading="lazy"

height="80px"

width="100%"

style="border-radius: 15px;"

src="https://elixia-player.koyeb.app/card/tencent/001t1qJd0DaKOs"

frameborder="0"

></iframe>
```

最后是完全丢弃 HTML 的**图片格式**，完全固定的内容。很多 UGC 平台都不能插入 `iframe`，这时候就可以直接用生成的 PNG 图片：

```
![](https://elixia-player.koyeb.app/card/tencent/002POzud0db9lK/image)
```

![Pretender](https://img.ssshooter.com/img/elixia-player/Pretender-card.png)

当然咯还是建议大家再套一层链接，让用户能直接点击跳转，所以完整版如下：

[![昔涟](https://img.ssshooter.com/img/elixia-player/%E6%98%94%E6%B6%9F-card.png)](https://y.qq.com/n/ryqq_v2/songDetail/002rhFKO3EjKAg)

```
[![昔涟](https://elixia-player.koyeb.app/card/tencent/002rhFKO3EjKAg/image)](https://y.qq.com/n/ryqq_v2/songDetail/002rhFKO3EjKAg)
```

**注意**，我这个白嫖服务生成图片非常慢，建议还是保存一份放服务器😂

其他官方选择：

[**Spotify**](https://open.spotify.com/) 从设计和加载速度上都不失为一个好选择，但最致命的是需要一些魔法才能访问，而且你总不能要求你的读者都会用魔法😂

```
<iframe

style="border-radius:12px"

src="https://open.spotify.com/embed/track/2leJWl7tBdFVj5Imag5T8J?utm_source=generator"

width="100%"

height="152"

frameborder="0"

allowfullscreen=""

allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"

loading="lazy"

></iframe>
```

[**网易云**](http://music.163.com/) 也不是不行，就是加载贼慢，以前写[二次元音乐推荐](https://ssshooter.com/2019-03-27-music-2-natsukoi/)的时候插入了一堆网易云 `iframe`，感觉加载速度也不怎么样，而且有点嫌弃它的颜值……

```
<iframe

frameborder="0"

border="0"

width="100%"

height="100"

src="//music.163.com/outchain/player?type=2&id=28915185&auto=0&height=66"

>

</iframe>
```

至于 [**QQ 音乐**](https://y.qq.com/)，好像没在官网找到可嵌入播放器。

大概就是这么个事，欢迎大家直接使用我已经[部署好的服务](https://elixia-player.koyeb.app)，不过因为是白嫖服务所以打开可能会有点慢……所以也欢迎大家自己在 [koyeb](https://app.koyeb.com/) 部署 [Elixia Player](https://github.com/SSShooter/elixia-player)，这样服务也不拥挤，体验好那么一点。

评论组件加载中……

© SSShooter 2026.[🚀 Usubeni Fantasy](/)

  [归档](/archive/)  [标签](/tags/)  [关于](/about/)  [友链](/links/)  [虫洞](https://www.foreverblog.cn/go.html)