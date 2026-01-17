---
title: 纯CSS实现折线连接两个任意元素效果
url: https://www.zhangxinxu.com/wordpress/2026/01/css-anchor-position-connect/
source: 张鑫旭-鑫空间-鑫生活
date: 2026-01-16
fetch_date: 2026-01-17T03:30:39.042490
---

# 纯CSS实现折线连接两个任意元素效果

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

## 纯CSS实现折线连接两个任意元素效果

这篇文章发布于 2026年01月16日，星期五，20:58，归类于 [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/)。 阅读 170 次, 今日 42 次 [没有评论](#comments)

by [zhangxinxu](https://www.zhangxinxu.com/) from <https://www.zhangxinxu.com/wordpress/?p=12026>
本文可全文转载，但需要保留原作者、出处以及文中链接，AI抓取保留原文地址，任何网站均可摘要聚合，商用请联系授权。

### 一、事情的起因

之前介绍[CSS锚点定位](https://www.zhangxinxu.com/wordpress/2024/06/css-anchor-positioning-api/)的时候有提到：

> 我们可以利用此特性，轻松实现任意两个点相连的折线效果，在过去，类似这样的效果一定要借助JS才可以。

![折线效果问答](https://image.zhangxinxu.com/image/blog/202601/2026-1-16_194608.png)

然后我就抽空自己试验了自己的想法，发现此事并没有自己想的那么简单。

### 二、先看下效果

先看GIF录屏效果，纯CSS实现的：

![拖拽](https://image.zhangxinxu.com/image/blog/202601/ball-drag2.gif)

#### 演示页面

您可以狠狠地点击这里：[纯CSS实现两个元素之间折线自动相连demo](https://www.zhangxinxu.com/study/202601/two-balls-line-auto-join-demo.php)

#### 实现原理

先从最简单的说起，两个圆和一条线。

```
<div class="circle circle-a"></div>
<div class="circle circle-b"></div>
<i class="line"></i>
```

圆的样式很简单，50%圆角绝对定位就可以了，对于本文需要实现的效果，重要的是定义锚点的名称：

```
.circle-a {
  anchor-name: --a;
}
.circle-b {
  anchor-name: --b;
}
```

此时，我们的线就可以左右两个球定位了：

```
.line {
  position: absolute;
  --posA: calc(anchor(--a inside) + anchor-size(--a) / 2);
  --posB: calc(anchor(--b inside) + anchor-size(--b) / 2);
  top: var(--posA);
  bottom: var(--posB);
  left: var(--posA);
  right: var(--posB);
  outline: dashed;
}
```

此时的效果就会是这样的：

![原理示意步骤1](https://image.zhangxinxu.com/image/blog/202601/2026-1-16_201953.png)

此时，我们就可以使用对角线渐变连接线条了（`clip-path`剪裁也可以）：

```
.line {
  background: linear-gradient(to left bottom, transparent calc(50% - 2px), currentColor calc(50% - 2px) calc(50% + 2px), transparent calc(50% + 2px)) no-repeat;
}
```

效果如下图所示：

![对角线连接2](https://image.zhangxinxu.com/image/blog/202601/2026-1-16_202229.png)

这线都跑到圆球上了，怎么办？

可以遮罩处理下，正好端点弄两个径向渐变遮罩下。

```
.line {
  mask: radial-gradient(circle at 0 0, #000 85px, transparent 85px no-repeat,
	  radial-gradient(circle at right bottom, #000 65px, transparent 65px no-repeat,
	  linear-gradient(#000, #000);
}
```

![最终的效果](https://image.zhangxinxu.com/image/blog/202601/2026-1-16_203340.png)

原理还是很简单的，但是实际上，两个球的位置是不固定的，上下左右都有可能，所以，如果只考虑一个方位，那么两个球的位置变化的时候，直线可能就不显示，因此，最终是使用了4条线。

```
<div class="circle circle-a"></div>
<div class="circle circle-b"></div>
<i class="line line1"></i>
<i class="line line2"></i>
<i class="line line3"></i>
<i class="line line4"></i>
```

完整代码可以参考demo页面。

### 三、其他一些说明

不过上面的实现并不完美，当两个圆的圆心在同一水平线，或者在同一垂直线上的时候，连接线是不显示的。

这个其实也有方法解决，不过麻烦了些。

1. 需要在设置line为container容器元素；
2. 图形效果使用子元素绘制，同时设置最小尺寸；
3. 基于 `100cqw` 和 `100cwh`计算的角度判断子元素是否显示，利用`opacity`属性的边界特性。

有个[codepen案例](https://codepen.io/t_afif/pen/PwNrNvP)就是这么实现的，有兴趣可以研究下。

时间有限，我就不深入了，因为这个实现成本已经超过JS了。

好吧，就说这么多，如果觉得内容不错，欢迎转发，分享。

我们下一篇文章再见👋🏻

![含韵挥手](https://image.zhangxinxu.com/image/blog/202601/2026-1-16_204944.jpeg)

本文为原创文章，会经常更新知识点以及修正一些错误，因此转载请保留原出处，方便溯源，避免陈旧错误知识的误导，同时有更好的阅读体验。
本文地址：<https://www.zhangxinxu.com/wordpress/?p=12026>

（本篇完）

相关文章

* [介绍2022年最期待的CSS container容器查询](https://www.zhangxinxu.com/wordpress/2022/09/css-container-rule/) (0.462)
* [补全不足，CSS锚点定位支持锚定容器回退检测了](https://www.zhangxinxu.com/wordpress/2025/12/css-anchor-container-query/) (0.399)
* [告别JS浮层，全新的CSS Anchor Positioning锚点定位API](https://www.zhangxinxu.com/wordpress/2024/06/css-anchor-positioning-api/) (0.378)
* [好诶，select下拉框元素支持样式完全自定义啦！](https://www.zhangxinxu.com/wordpress/2025/07/css-checkmark-select-customizable/) (0.378)
* [又发现一种无需绝对定位就可以元素重叠的CSS技巧](https://www.zhangxinxu.com/wordpress/2023/03/css-container-rule-overlap/) (0.273)
* [CSS高宽不等图片固定比例布局的三重进化](https://www.zhangxinxu.com/wordpress/2023/07/css-image-aspect-ratio-layout/) (0.273)
* [CSS progress()函数简介](https://www.zhangxinxu.com/wordpress/2025/12/css-progress-function/) (0.189)
* [第五届CSS大会主题分享之CSS创意与视觉表现](https://www.zhangxinxu.com/wordpress/2019/06/cssconf-css-idea/) (0.161)
* [好奇心驱使下试验了chatGPT写CSS代码的能力](https://www.zhangxinxu.com/wordpress/2023/03/chatgpt-write-css/) (0.142)
* [聊聊Top Layer顶层特性的隐患与实践](https://www.zhangxinxu.com/wordpress/2024/06/web-top-layer/) (0.126)
* [您可能不知道的CSS元素隐藏“失效”以其妙用](https://www.zhangxinxu.com/wordpress/2012/02/css-overflow-hidden-visibility-hidden-disabled-use/) (RANDOM - 0.044)

«上一篇 [学会使用CSSStyleSheet构造CSS样式](https://www.zhangxinxu.com/wordpress/2026/01/dom-cssstylesheet/)

分享到：

标签： [@container](https://www.zhangxinxu.com/wordpress/tag/container/), [anchor-size()](https://www.zhangxinxu.com/wordpress/tag/anchor-size/), [container-type](https://www.zhangxinxu.com/wordpress/tag/container-type/), [cqw](https://www.zhangxinxu.com/wordpress/tag/cqw/), [linear-gradient](https://www.zhangxinxu.com/wordpress/tag/linear-gradient/), [mask](https://www.zhangxinxu.com/wordpress/tag/mask/), [opacity](https://www.zhangxinxu.com/wordpress/tag/opacity/), [锚点定位](https://www.zhangxinxu.com/wordpress/tag/%E9%94%9A%E7%82%B9%E5%AE%9A%E4%BD%8D/)

﻿

### 发表评论（目前没有评论）

[点击这里取消回复。](/wordpress/2026/01/css-anchor-position-connect/#respond)

网站

![](//image.zhangxinxu.com/image/blog/zxx_240_0818.jpg)

张鑫旭[more](/life/about/)，09年[华中科技大学](http://www.hust.edu.cn/)毕业，现上海，就职于[阅文集团](//www.yuewen.com/)，专注web前端偏前领域，著有[《CSS世界》](https://item.jd.com/12262251.html)[《CSS选择器世界》](https://item.jd.com/12712370.html)[《CSS新世界》](https://item.jd.com/13356308.html)[《HTML并不简单》](https://item.jd.com/14213015.html)

邮箱：zhangxinxu@zhangxinxu.com

关注我：[微信](https://www.zhangxinxu.com/sp/wechat.html)[微博](//weibo.com/zhangxinxu)[掘金](//juejin.im/user/595315e7f265da6c322dc6c6)[知乎](//www.zhihu.com/people/iamzhangxinxu/activities)[抖音热更](https://www.zhangxinxu.com/sp/wechat.html?douyin=1)[B站](https://space.bilibili.com/31556431/)[Gitee](https://gitee.com/zhangxinxu/)

* ## 文章分类

  + [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/) (462)
  + [Design相关](https://www.zhangxinxu.com/wordpress/category/ps/) (13)
  + [Graphic相关](https://www.zhangxinxu.com/wordpress/category/graphic/) (83)
    - [Canvas相关](https://www.zhangxinxu.com/wordpress/category/graphic/canvas-graphic/) (32)
    - [SVG相关](https://www.zhangxinxu.com/wordpress/category/graphic/svg-graphic/) (49)
  + [HTML相关](https://www.zhangxinxu.com/wordpress/category/html/) (66)
  + [JS相关](https://www.zhangxinxu.com/wordpress/category/js/) (293)
    - [jQuery相关](https://www.zhangxinxu.com/wordpress/category/js/jquery-about/) (46)
    - [JS API](https://www.zhangxinxu.com/wordpress/category/js/js-api/) (81)
    - [JS实例](https://www.zhangxinxu.com/wordpress/category/js/js%E5%AE%9E%E4%BE%8B/) (146)
  + [Mobile相关](https://www.zhangxinxu.com/wordpress/category/mobile/) (15)
  + [Web综合](https://www.zhangxinxu.com/wordpress/category/web/) (56)
  + [外文翻译](https://www.zhangxinxu.com/wordpress/category/%E5%A4%96%E6%96%87%E7%BF%BB%E8%AF%91/) (36)
* ## 文章存档

  文章存档

  选择月份
   2026年一月  (2)
   2025年十二月  (4)
   2025年十一月  (5)
   2025年十月  (3)
   2025年九月  (4)
   2025年八月  (4)
   2025年七月  (5)
   2025年六月  (4)
   2025年五月  (3)
   2025年四月  (5)
   2025年三月  (5)
   2025年二月  (4)
   2025年一月  (4)
   2024年十二月  (5)
   2024年十一月  (5)
   2024年十月  (3)
   2024年九月  (5)
   2024年八月  (2)
   2024年七月  (5)
   2024年六月  (3)
   2024年五月  (3)
   2024年四月  (3)
   2024年三月  (3)
   2024年二月  (1)
   2024年一月  (4)
   2023年十二月  (3)
   2023年十一月  (4)
   2023年十月  (3)
   2023年九月  (4)
   2023年八月  (4)
   2023年七月  (3)
   2023年六月  (4)
   2023年五月  (3)
   2023年四月  (1)
   2023年三月  (5)
   2023年二月  (4)
   2023年一月  (3)
   2022年十二月  (2)
   2022年十一月  (4)
   2022年十月  (4)
   2022年九月  (4)
   2022年八月  (3)
   2022年七月  (2)
   2022年六月  (4)
   2022年五月  (4)
   2022年四月  (3)
   2022年三月  (5)
   2022年二月  (4)
   2022年一月  (3)
   2021年十二月  (4)
   2021年十一月  (3)
   2021年十月  (3)
   2021年九月  (4)
   2021年八月  (5)
   2021年七月  (6)
   2021年六月  (2...