---
title: 学会使用CSSStyleSheet构造CSS样式
url: https://www.zhangxinxu.com/wordpress/2026/01/dom-cssstylesheet/
source: 张鑫旭-鑫空间-鑫生活
date: 2026-01-05
fetch_date: 2026-01-06T03:31:57.302383
---

# 学会使用CSSStyleSheet构造CSS样式

[张鑫旭-鑫空间-鑫生活](/)

it's my whole life!

* [网站首页](/)
  [我的微码](/php/microCode)
  [建议反馈](/php/advise)
* [前端技术](/wordpress/)
  [CSS](/wordpress/?cat=3)
  [JavaScript](/wordpress/?cat=5)
  [HTML](/wordpress/category/html/)
* [生活与创作](/life/)
  [生活](/life/?cat=3)
  [散文](/life/?cat=5)
  [日紫烟](http://riziyan.com/)
* 前端在线资源
  [HTML并不简单](https://www.htmlapi.cn/)
  [Canvas中文API](//www.canvasapi.cn/)

Search for:

## 学会使用CSSStyleSheet构造CSS样式

这篇文章发布于 2026年01月5日，星期一，16:32，归类于 [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/), [JS API](https://www.zhangxinxu.com/wordpress/category/js/js-api/)。 阅读 242 次, 今日 64 次 [2 条评论](#comments)

by [zhangxinxu](https://www.zhangxinxu.com/) from <https://www.zhangxinxu.com/wordpress/?p=12014>
本文可全文转载，但需要保留原作者、出处以及文中链接，AI抓取保留原文地址，任何网站均可摘要聚合，商用请联系授权。

### 一、创建style元素的问题

如果想要在页面中插入一段全新的CSS样式，大多数的前端开发人员都是通过创建 `<style>` 元素，然后插入字符串CSS代码实现的，示意：

```
const styleEl = document.createElement('style');
styleEl.innerHTML = `.my-class { color: red; font-size: 16px; }`;
document.head.appendChild(styleEl);
```

这种方法的不足非常明显：

1. **难以进行细粒度操作：**要修改、删除或查询某一条具体的CSS规则，必须手动解析整个庞大的CSS字符串，这既繁琐又容易出错。

   例如，我们想要在添加一段CSS样式，代码需要类似这样处理：

   ```
   const styleEl = document.querySelector('style');
   const oldText = styleEl.innerHTML;
   styleEl.innerHTML = oldText + '\n.new-rule { background: blue; }';
   ```
2. **性能低下：**每次修改都意味着要替换整个 `<style>` 标签的内容，浏览器需要重新解析整个样式字符串，对于频繁的样式更新，这会带来不必要的性能消耗。
3. **容易引发错误：**在字符串拼接或替换过程中，一个微小的语法错误（如缺少分号、括号不匹配）就可能导致整个样式表失效，且错误难以定位。
4. **无法利用已解析的结构：**浏览器在加载页面时，已经将CSS解析成了结构化的规则对象（CSSOM）。而字符串操作完全绕过了这个高效的结构，迫使开发者自己处理原始文本。

有不足就有需求，于是`CSSStyleSheet()`构造函数应运而生，专门用来创建CSS样式。

### 二、CSSStyleSheet使用指南

案例是最快速的学习方式，例如，给当前文章的所有 `<h3>` 标题添加阴影，则可以：

```
// 构造空白的样式表
const sheet = new CSSStyleSheet();
// 给样式表添加对应的CSS规则
sheet.replaceSync("article h3 { text-shadow: 2px 2px 4px #0006; }");
// 添加到页面中
document.adoptedStyleSheets.push(sheet);
```

这段代码是事实运行的，大家可以仔细观察文章的标题文字，看看是不是有投影。

#### 语法

语法如下：

```
const sheet = new CSSStyleSheet(options)
```

其中，`options`是可选参数，包括下面这些参数值：

**baseURL**
:   样式表中的URL地址的根地址

**media**
:   媒体查询规则。可以是单个字符串规则，也可以是MediaList。
:   使用示意：

    ```
    const stylesheet = new CSSStyleSheet({ media: "print" });
    console.log(stylesheet.media);
    ```

**disabled**
:   是否禁用当前的样式表。默认值是 `false`.

然后，返回值`sheet`是个CSSStyleSheet对象，包含以下一些属性和CSS规则处理方法：

### 三、CSSStyleSheet对象的属性和方法

两个属性，用来返回当前样式表的样式：

**cssRules**
:   只读属性，返回一个实时CSSRuleList。

**ownerRule**
:   如果使用`@import`规则将此样式表导入文档，`ownerRule`属性将返回相应的CSSImportRule；否则，此属性的值为`null`。

`ownerRule`属性很少使用，我们看下`cssRules`属性的细节。

#### 1. 嵌套语法的输出

比方说我们看下CSS嵌套语法返回的内容：

```
const sheet = new CSSStyleSheet();
sheet.replaceSync(`.project-x {
  display: flex;
  &.many {
    font-size: calc(1em - 2px);
    .project-item {
      padding: .5em .75em;
    }
    max-height: calc(100vh - var(--rem) * 20);
    overflow: hidden;
  }
}`);
console.log(sheet.cssRules);
```

最终的输出结果出乎意料，只有一个CSS规则，如下截图所示：

![CSS嵌套语句的输出](https://image.zhangxinxu.com/image/blog/202601/2026-1-5_151529.png)

#### 2. 如果语句铺平

如果嵌套语句打平，就像这样：

```
const sheet = new CSSStyleSheet();
sheet.replaceSync(`.project-x {
  display: flex;
}
.project-x.many { font-size: calc(1em - 2px); }
.project-x .project-item {
  padding: .5em .75em;
}`);
console.log(sheet.cssRules);
```

那么输出的规则就返回预期了：

![多个规则示意](https://image.zhangxinxu.com/image/blog/202601/2026-1-5_152624.png)

嗯……🤔……

没有预想的强大啊，嵌套语句也应该帮忙解析成一个一个规则才是。

#### 3. 如果使用AT规则

测试代码为：

```
const sheet = new CSSStyleSheet();
sheet.replaceSync(`@media (width < 480px) {
   body { font-size: 16px; }
}
@scope(.container) {
  .list {
    color: red;
  }
}`);
console.log(sheet.cssRules);
```

结果是两条规则，还有不同的type，嗯……🤔……看来，这里面水还挺深的。

![AT规则遍历结果](https://image.zhangxinxu.com/image/blog/202601/2026-1-5_153454.png)

---

再来看下比属性更加常用的方法。

**deleteRule(index)**
:   删除规则，参数是规则索引值。

**insertRule(rule, index)**
:   插入CSS规则，`rule`是CSS规则字符串，`index`参数可选，表示新CSS规则插入的位置，默认是`0`。

**replace()**
:   样式完整替换，返回的是Promise。

**replaceSync()**
:   同步样式替换，平时我们使用这个更多一些，属于后来支持的新特性。

相比传统的`innerHTML` DOM元素替换，CSSStyleSheet要更加规范。

### 四、优先级、兼容性等特性

CSSStyleSheet构造样式的优先级和常规的CSS样式优先级规则一致。

并且在页面中是没有对应的DOM元素的，该样式会有对应的`constructed stylesheet`标识，如下截图所示：

![构造样式标识](https://image.zhangxinxu.com/image/blog/202601/2026-1-5_154623.png)

#### Shadow DOM

CSSStyleSheet构造的样式也可以再shadow中创建，例如：

```
const node = document.createElement("div");
const shadow = node.attachShadow({ mode: "open" });
// 添加样式表到 shadow DOM 中
shadow.adoptedStyleSheets = [sheet];
```

#### 兼容性

CSSStyleSheet的大部分特性浏览器很早就支持了，但是其中一个方法，也就是 `replaceSync()` 方法属于新特性，最近一两年才支持的，兼容性如下图所示：

![replaceSync()兼容性](https://image.zhangxinxu.com/image/blog/202601/2026-1-5_161637.png)

基本上大家是可以放心使用的。

### 五、结语

传统的字符串操作样式的方式，本质上是在“模拟”浏览器的工作，笨重且不灵活。

而 CSSStyleSheet API 提供了一套浏览器原生的、面向对象的接口，让开发者能够以浏览器“理解”的方式去直接操作样式规则本身。
它带来的改变是根本性的：

* 从“全文替换”到“精准手术”。
* 从“容易出错”到“稳定可控”。
* 从“性能消耗大”到“高效更新”。

因此，当您需要动态、精细、高性能地管理页面样式时，尤其是在开发复杂的交互应用、浏览器扩展、主题系统或组件库时，CSSStyleSheet API 是远比操作字符串更现代、更专业的解决方案。

😉😊😇
🥰😍😘

本文为原创文章，会经常更新知识点以及修正一些错误，因此转载请保留原出处，方便溯源，避免陈旧错误知识的误导，同时有更好的阅读体验。
本文地址：<https://www.zhangxinxu.com/wordpress/?p=12014>

（本篇完）

相关文章

* [Web Components中引入外部CSS的3种方法](https://www.zhangxinxu.com/wordpress/2021/02/web-components-import-css/) (0.440)
* [光速了解HTML shadowrootmode等属性的作用](https://www.zhangxinxu.com/wordpress/2025/04/html-shadowrootmode-shadowrootserializable/) (0.236)
* [巧用DOM API实现HTML字符的转义和反转义](https://www.zhangxinxu.com/wordpress/2021/01/dom-api-html-encode-decode/) (0.204)
* [DOM元素querySelectorAll可能让你意外的特性表现](https://www.zhangxinxu.com/wordpress/2015/11/know-dom-queryselectorall/) (0.113)
* [CSS @scope他来了](https://www.zhangxinxu.com/wordpress/2024/01/css-at-scope/) (0.106)
* [CSS Nesting嵌套与@scope规则也太雷同了吧？](https://www.zhangxinxu.com/wordpress/2024/03/css-nesting-scope-rules/) (0.106)
* [巧用:is()或:where()伪类让scoped的style依然全局匹配](https://www.zhangxinxu.com/wordpress/2022/09/css-is-where-scoped-style/) (0.082)
* [让IE6/IE7/IE8浏览器支持CSS3属性](https://www.zhangxinxu.com/wordpress/2010/04/%E8%AE%A9ie6ie7ie8%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81css3%E5%B1%9E%E6%80%A7/) (0.045)
* [CSS "渐进增强"在web制作中常见应用举例](https://www.zhangxinxu.com/wordpress/2010/04/css-%E6%B8%90%E8%BF%9B%E5%A2%9E%E5%BC%BA%E5%9C%A8web%E5%88%B6%E4%BD%9C%E4%B8%AD%E5%B8%B8%E8%A7%81%E5%BA%94%E7%94%A8%E4%B8%BE%E4%BE%8B/) (0.045)
* [CSS3模拟window7炫酷界面效果展示](https://www.zhangxinxu.com/wordpress/2010/05/css3%E6%A8%A1%E6%8B%9Fwindow7%E7%82%AB%E9%85%B7%E7%95%8C%E9%9D%A2%E6%95%88%E6%9E%9C%E5%B1%95%E7%A4%BA/) (0.045)
* [详解日后定会大规模使用的CSS @layer 规则](https://www.zhangxinxu.com/wordpress/2022/05/css-layer-rule/) (RANDOM - 0.024)

«上一篇 [今日学习CSS style()样式查询及其range范围语法](https://www.zhangxinxu.com/wordpress/2025/12/css-style-container-range-syntax/)

分享到：

标签： [:scope](https://www.zhangxinxu.com/wordpress/tag/scope/), [@media](https://www.zhangxinxu.com/wordpress/tag/media/), [adoptedStyleSheets](https://www.zhangxinxu.com/wordpress/tag/adoptedstylesheets/), [attachShadow](https://www.zhangxinxu.com/wordpress/tag/attachshadow/), [createElement](https://www.zhangxinxu.com/wordpress/tag/createelement/), [CSSStyleSheet](https://www.zhangxinxu.com/wordpress/tag/cssstylesheet/), [replaceSync()](https://www.zhangxinxu.com/wordpress/tag/replacesync/), [Shadow DOM](https://www.zhangxinxu.com/wordpress/tag/shadow-dom/), [text-shadow](https://www.zhangxinxu.com/wordpress/tag/text-shadow/)

﻿

### 发表评论（目前2 条评论）

[点击这里取消回复。](/wordpress/2026/01/dom-cssstylesheet/#respond)

网站

1. ![](https://gravatar.loli.net/avatar/199e2d1f8f5bbb0a657d5be3bf6812ff?s=64) 鸵鸟说道：

   [2026年01月6日 06:10](https://www.zhangxinxu.com/wordpress/2026/01/dom-cssstylesheet/#comment-479883)

   无、结语
   ———-
   打错字了

   [回复](https://www.zhangxinxu.com/wordpress/2026/01/dom-cssstylesheet/?replytocom=479883#respond)

   * ![](https://gravatar.loli.net/avatar/3e58242902fa8f33162f3f10df892c68?s=64) [张 鑫旭](http://www.zhangxinxu.com)说道：

     [2026年01月6日 10:06](https://www.zhangxinxu.com/wordpress/2026/01/dom-cssstylesheet/#comment-479893)

     感谢反馈~

     [回复](https://www.zhangxinxu.com/wordpress/20...