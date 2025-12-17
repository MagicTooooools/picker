---
title: OPPO 快游戏在浏览器复现运行（四）之游戏入口加载
url: https://www.liesauer.net/blog/post/1021.html
source: LiesAuer's Blog
date: 2025-12-16
fetch_date: 2025-12-17T03:23:28.037807
---

# OPPO 快游戏在浏览器复现运行（四）之游戏入口加载

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

# OPPO 快游戏在浏览器复现运行（四）之游戏入口加载

[开发](https://www.liesauer.net/blog/category/coding/) · [游戏](https://www.liesauer.net/blog/category/gaming/)
 ·
今天

[LiesAuer](https://www.liesauer.net/blog/author/1/)

一直以来，`web.sdk.js`都是通过以下方式加载快游戏的。
```html
<script src="./web.sdk.js"></script>
<script src="./main.js" type="module"></script>
```
在大部分的游戏中也是没有问题的，由于`web.sdk.js`中存在大量的异步加载逻辑，因此将`main.js`定义为模块，并在代码最前面补充加载等待是对游戏原代码侵入较小的一种方案，而且恰恰是模块，因此顶级 await 也就非常顺理成章了。
```js
await window.qg.ready();
```
直到遇到下面某款游戏的入口（代码片段）
<!--more-->
```js
await window.qg.ready();
window['qg'].setIsUnityGame(true);
const xgame = qg;
require("ral.js");
```
初步一看，看似没啥毛病对吧，非常简单的一个代码片段，但问题就出在了那行平平无奇的`const xgame = qg;`，由于`main.js`是模块，自带作用域隔离，因此此时的作用域是模块内部作用域，并非全局作用域，然后加载`ral.js`时就出问题了，`ral.js`是访问不到`xgame`的，会直接报错导致游戏加载中断。
为了解决这个问题，那么就不能将游戏入口定义为模块，不定义为模块，就没法在顶级 await，而且还得同时兼顾执行上下文的问题，在众多动态执行JS代码的方案中，那么很明显就只有动态创建script标签加载是最合适的了。
那么没办法，就只能在`web.sdk.js`中初始化自身后动态加载`main.js`了，由于快游戏的入口是固定的，因此直接硬编码加载也是没得问题的，这里处理后，`main.js`也就不需要`await window.qg.ready();`了，游戏入口代码零侵入。
```
window.qg.\_ready = true;
await requireAsync("main.js");
```

[快游戏](https://www.liesauer.net/blog/tag/%E5%BF%AB%E6%B8%B8%E6%88%8F/) [快应用](https://www.liesauer.net/blog/tag/%E5%BF%AB%E5%BA%94%E7%94%A8/) [OPPO 快游戏在浏览器复现运行](https://www.liesauer.net/blog/tag/OPPO-%E5%BF%AB%E6%B8%B8%E6%88%8F%E5%9C%A8%E6%B5%8F%E8%A7%88%E5%99%A8%E5%A4%8D%E7%8E%B0%E8%BF%90%E8%A1%8C/)

如遇到文件无法下载，可右键复制链接打开标签页粘贴下载！

[如果您觉得文章或项目对您有帮助，戳我请博主喝一杯咖啡叭！](/blog/buy-me-a-coffee.html)

[取消回复](https://www.liesauer.net/blog/post/1021.html#respond-post-1021) 提交评论

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