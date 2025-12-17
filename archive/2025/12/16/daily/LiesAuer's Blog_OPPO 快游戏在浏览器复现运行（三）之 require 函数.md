---
title: OPPO 快游戏在浏览器复现运行（三）之 require 函数
url: https://www.liesauer.net/blog/post/1018.html
source: LiesAuer's Blog
date: 2025-12-16
fetch_date: 2025-12-17T03:23:29.501037
---

# OPPO 快游戏在浏览器复现运行（三）之 require 函数

* 关于
* Buy Me a Coffee
* 归档
* 友链
* 猫咪
* RSS
* 后台

* 切换模式
* 返回顶部

* [首页](https://www.liesauer.net/blog/ "首页")
* [说说](https://www.liesauer.net/blog/category/shuoshuo/ "说说")
* [开发](https://www.liesauer.net/blog/category/coding/ "开发")
* [AI](https://www.liesauer.net/blog/category/ai/ "AI")
* [游戏](https://www.liesauer.net/blog/category/gaming/ "游戏")
* [资源](https://www.liesauer.net/blog/category/resources/ "资源")
* [虚拟货币](https://www.liesauer.net/blog/category/cryptocurrency/ "虚拟货币")
* [杂七杂八](https://www.liesauer.net/blog/category/default/ "杂七杂八")

* [首页](https://www.liesauer.net/blog/ "首页")
* [说说](https://www.liesauer.net/blog/category/shuoshuo/ "说说")
* [开发](https://www.liesauer.net/blog/category/coding/ "开发")
* [AI](https://www.liesauer.net/blog/category/ai/ "AI")
* [游戏](https://www.liesauer.net/blog/category/gaming/ "游戏")
* [资源](https://www.liesauer.net/blog/category/resources/ "资源")
* [虚拟货币](https://www.liesauer.net/blog/category/cryptocurrency/ "虚拟货币")
* [杂七杂八](https://www.liesauer.net/blog/category/default/ "杂七杂八")

* [关于](/blog/about.html "关于")
* [Buy Me a Coffee](/blog/buy-me-a-coffee.html "Buy Me a Coffee")
* [归档](/blog/archive.html "归档")
* [友链](/blog/links.html "友链")
* [猫咪](/blog/cat.html "猫咪")
* [RSS](https://www.liesauer.net/blog/feed/ "RSS")
* [后台](/blog/admin "后台")

# OPPO 快游戏在浏览器复现运行（三）之 require 函数

[开发](https://www.liesauer.net/blog/category/coding/) · [游戏](https://www.liesauer.net/blog/category/gaming/)
 ·
昨天
 ·
更新于 今天

[LiesAuer](https://www.liesauer.net/blog/author/1/)

本质上，快游戏框架是基于V8引擎魔改出来的，因此天然支持`require`函数，但在 Web 中实现这个功能就不简单了，第一版的方案是想着将`main.js`作为模块在最顶级加载，然后通过将所有的`require`调用都改为`await`。
```js
await require("./libs/min/laya.core.min.js");
await require("./libs/min/laya.ani.min.js");
await require("./libs/min/laya.ui.min.js");
```
```js
async function require(url) {
const script = document.createElement("script");
script.type = "text/javascript";
script.async = false;
script.defer = false;
script.src = url;
await new Promise((resolve, reject) => {
script.onload = () => {
script.remove();
resolve();
};
script.onerror = () => {
script.remove();
reject();
};
document.body.constructor.prototype.appendChild.apply(document.body, [script]);
});
}
```
一开始，这个方案也是走得通的，对于一些简单的游戏改的方便，加载也是没啥问题的，但是随着接触的快游戏越来越多，这个方案就越发难走下去，主要有两个问题：
<!--more-->
1. 对原代码侵入性太大，特别是对于加密混淆过的代码来说，改起来比登天难
2. 游戏初始化复杂性导致异步加载的方案始终会面临运行顺序差异导致的运行异常
特别是第二点，直接影响了`web.sdk.js`能否继续搞下去，因此异步加载的方案就毙了，必须走同步的形式，而同步加载第一个难点就是如何在浏览器中同步获取到压根没加载的JS代码呢？难不成在`web.sdk.js`之前先把所有的JS代码以字符串形式打包进去吗？不需要！！！不知道你们是否还记得上一篇文章中提到过得文件系统，回顾请看 → [OPPO 快游戏在浏览器复现运行（二）之文件系统](https://www.liesauer.net/blog/post/1007.html "OPPO 快游戏在浏览器复现运行（二）之文件系统")，而这个文件系统是带了同步 + 异步两套 API 的，那么就让我想到了一个非常有意思的方案。
在`web.sdk.js`初始化阶段，我们可以先通过异步 API 将整个游戏“安装”到浏览器中，而底层存储自然就通过 IndexedDB 来做了，让整个启动流程像打开 APP 一样，第一次先安装再打开的过程。“安装”完过后，我们就可以通过同步 API 将游戏的JS代码以同步的形式读取出来了。
读取问题解决了，那就剩下加载问题了，这里我们就需要在 Web 端把 NodeJS 中的`require`函数复刻出来了，这个的话有太多的现成库作参考了，这里我就使用了阮一峰老师的 [tiny-browser-require](https://github.com/ruanyf/tiny-browser-require/blob/master/require.js "tiny-browser-require") 作为基础并针对快游戏做了一定的适配。
至此，`require`也算是比较“完美”的实现了，当然和原版`require`还是差很远的，比如寻路规则、循环依赖、加载缓存等等问题都没处理，但对于快游戏来说目前是够用了。
这也是这个项目最迷人的地方，所有的东西都是环环相扣的，少一样都不行，一旦把他们联通起来，那种茅塞顿开的感觉太妙了。

[快游戏](https://www.liesauer.net/blog/tag/%E5%BF%AB%E6%B8%B8%E6%88%8F/) [快应用](https://www.liesauer.net/blog/tag/%E5%BF%AB%E5%BA%94%E7%94%A8/) [OPPO 快游戏在浏览器复现运行](https://www.liesauer.net/blog/tag/OPPO-%E5%BF%AB%E6%B8%B8%E6%88%8F%E5%9C%A8%E6%B5%8F%E8%A7%88%E5%99%A8%E5%A4%8D%E7%8E%B0%E8%BF%90%E8%A1%8C/)

如遇到文件无法下载，可右键复制链接打开标签页粘贴下载！

[如果您觉得文章或项目对您有帮助，戳我请博主喝一杯咖啡叭！](/blog/buy-me-a-coffee.html)

[取消回复](https://www.liesauer.net/blog/post/1018.html#respond-post-1018) 提交评论

瞅一瞅叭

[前端逆向接活](https://www.liesauer.net/blog/post/991.html)

[收一个 Reqable 永久版订阅](https://www.liesauer.net/blog/post/1017.html)

最新评论

* [Dog: hello,Can you please confirm if ...](https://www.liesauer.net/blog/post/binance-red-envelope-scavenger.html/comment-page-1#comment-1630 "hello,Can you please confirm if ...")
* [追梦人: 大佬请问开纸飞机会员速度能提升吗？能的话我开一个，资源太多了，速...](https://www.liesauer.net/blog/post/telemediaspider.html/comment-page-1#comment-1478 "大佬请问开纸飞机会员速度能提升吗？能的话我开一个，资源太多了，速...")
* [追梦人: 要不内置个 sqlite-web？](https://www.liesauer.net/blog/post/telemediaspider.html/comment-page-1#comment-1477 "要不内置个 sqlite-web？")
* [cookee77: 好了 去github下载了 谢谢](https://www.liesauer.net/blog/post/wechat-pic2meme.html/comment-page-1#comment-1440 "好了 去github下载了 谢谢")
* [cookee77: 请问为什么下载不了](https://www.liesauer.net/blog/post/wechat-pic2meme.html/comment-page-1#comment-1439 "请问为什么下载不了")
* [哈哈: 大佬，这个能离线保存文本内容吗，自己想收集点吃瓜内容，没事可以看](https://www.liesauer.net/blog/post/telemediaspider.html/comment-page-1#comment-1390 "大佬，这个能离线保存文本内容吗，自己想收集点吃瓜内容，没事可以看")
* [我是军爸: 养两只猫，那够你忙的了吧](https://www.liesauer.net/blog/about.html/comment-page-1#comment-1293 "养两只猫，那够你忙的了吧")

关于站长

* 广东 佛山
* liesauer#liesauer.net
* [LiesAuer](https://github.com/liesauer)
* [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh-hans)
* [粤ICP备16094588号-1](https://beian.miit.gov.cn/)
* [萌ICP备20245567号](https://icp.gov.moe/?keyword=20245567)
* 茶
  [茶ICP备2025080027号](https://icp.redcha.cn/beian/ICP-2025080027.html)

[![Not By AI]()](https://notbyai.fyi/)[![十年之约](https://storage.liesauer.net/foreverblog/logo_en_default.png)](https://www.foreverblog.cn/)

2014 - 2025 LiesAuer's Blog. All Rights Reserved.

Theme [Jasmine](https://github.com/liaocp666/Jasmine "Jasmine") by [Kent Liao](https://www.liaocp.cn/ "Kent Liao")