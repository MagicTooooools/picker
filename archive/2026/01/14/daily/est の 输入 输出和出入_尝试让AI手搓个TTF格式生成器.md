---
title: 尝试让AI手搓个TTF格式生成器
url: https://blog.est.im/2026/stdout-03
source: est の 输入 输出和出入
date: 2026-01-14
fetch_date: 2026-01-15T03:34:44.813652
---

# 尝试让AI手搓个TTF格式生成器

# [est の 输入 输出和出入](/)

* [Home](/)
* [RSS](https://feeds.feedburner.com/initiative)
* [About](/about)
* [Category](/category)

## 尝试让AI手搓个TTF格式生成器

Posted 2026-01-14 | stdout

一个奇怪的需求：如何在浏览器判断一个字体是否支持某个字符？

（原始需求是：遇到一些字符渲染错位问题，看起来是字体不支持，fallback 到别的去了。）

想到的方法是：用canvas渲染看宽度。但因为这个 fallback机制，所以更好的办法是拿一个已知的特殊字体去比对，如果fallback了说明不支持。

那么问题来了，这个 fallback font 你不可能下载一个包含所有字符的，那样体积会很大，所以最好是按需生成一个，只包含一个字符，用来比对。那么这个问题就转换成了：如何在浏览器js里动态生成一个 .ttf 格式的字体文件，只包含一个字符？

这里不考虑 woff woff2，因为前者已经过时了后者比 ttf 更复杂。

一开始以为很easy，让 ChatGPT搓，打开浏览器就懵逼

```
OTS parsing error: bad table directory searchRange
bad table directory entrySelector
bad table directory rangeShift
Invalid table tag: 0x66000000
f\x00\x00\x00: invalid table offset
```

感觉事情没那么简单。让Antigravity搓，它把我免费额度烧光了也没搓出个所以然

其中一个小插曲是，css里允许对单独一个字符设置 `font-family`，但是因为这个 .ttf 是动态生成的，所以需要动态生成这个CSS的声明，所以在生成 .ttf 之后需要类似这样的代码：

```
document.head.insertAdjacentHTML("beforeend", `
<style>
@font-face {
  font-family: GhostRaw;
  src: url(${url});
  unicode-range: U+2603;
}
body { font-family: GhostRaw, system-ui; }
</style>
`);
```

注意这个是在 html 里的 js 里的 template string 里的 css。antigravity改这一块出现了好多次 error，我猜是AI输出格式嵌套格式本来就容易出错，这里引用和转义太复杂以至于 agent 直接看不懂output了。哈哈

因为没额度了，所以换国产免费的 Trae。Trae 比antigravity更笨，但是也努力。js搓不动，就开始换写 .py 去验证。搞到最后搞出来一堆测试文件

check\_maxp.py
check\_validation.py
generate\_minimal\_ttf.py
generate\_proper\_ttf.py
generate\_simple\_zero\_width\_font.py
generate\_ttf.js
generate\_zero\_width\_font.py
generated\_font.ttf
generated\_font.ttx
get\_base64.js
minimal\_font\_generator.py
modify\_existing\_font.py
simple\_font\_generator.py
validate\_ttf.py
validate.js
working\_font\_generator.py
zero\_width\_font.ttf
zero-width.ttf

这样几回合下来，直接搓超出上下文了，最后直接罢工，出现

> Output is too long, please enter 'Continue' to get more.

而且你点了 continue 它思索半天还是出现这句话。上下文是彻底爆了。

正向构造一个 .ttf 很难，AI还很聪明的想到了：

> 我将使用一个更简单的方法，直接使用一个现有的基础字体文件，然后修改它的字符映射。让我检查一下是否有任何基础 TTF 文件可用。我将使用 Google Fonts 中的一个非常简单的字体作为基础。

最后还是失败了。于是我就去搜索引擎找到了这个：

<https://pomax.github.io/Minimal-font-generator/>

> dynamically generated bespoke font that encodes that character as a zero-width glyph
> in the PDF.js project, where a PDF file may have fonts embedded for rendering text with, but no way to tell whether an extracted font has actually finished loading.

和我想到一块去了。还是人类老哥牛逼，2012-01-14号就把这个方案搓出来了，距今刚好整整14年。

AI就像一个培训班出身的，对背题能过的任务能很快完成，对于这种考验细节的冷门任务，还是难。

## Comments