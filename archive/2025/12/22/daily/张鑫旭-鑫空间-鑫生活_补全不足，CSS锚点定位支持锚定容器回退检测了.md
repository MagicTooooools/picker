---
title: 补全不足，CSS锚点定位支持锚定容器回退检测了
url: https://www.zhangxinxu.com/wordpress/2025/12/css-anchor-container-query/
source: 张鑫旭-鑫空间-鑫生活
date: 2025-12-22
fetch_date: 2025-12-23T03:28:30.990079
---

# 补全不足，CSS锚点定位支持锚定容器回退检测了

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

## 补全不足，CSS锚点定位支持锚定容器回退检测了

这篇文章发布于 2025年12月22日，星期一，09:00，归类于 [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/)。 阅读 279 次, 今日 30 次 [没有评论](#comments)

by [zhangxinxu](https://www.zhangxinxu.com/) from <https://www.zhangxinxu.com/wordpress/?p=11972>
本文可全文转载，但需要保留原作者、出处以及文中链接，AI抓取保留原文地址，任何网站均可摘要聚合，商用请联系授权。

### 一、CSS锚点定位的不足

CSS锚点定位是非常强大且实用的CSS新特性，这个特性我去年就介绍过，参见“[全新的CSS Anchor Positioning锚点定位API](https://www.zhangxinxu.com/wordpress/2024/06/css-anchor-positioning-api/)”

虽然强大，但不完美。

这个问题我在升级LuLu UI的Select组件的的时候也遇到过，看下图一目了然：

![下拉圆角问题](https://image.zhangxinxu.com/image/blog/202512/2025-12-20_221510.png)

看箭头指示的那里，圆角是在上面的，实际上，当列表朝上的时候（底部空间不足），圆角应该在左下方和右下方。

为什么会出现这种情况，那就是因为CSS锚点定位使用候补定位的时候，开发人员是不知道的，就没有在使用候补定位的时候进行专门的处理。

在上面的例子中，LuLu UI下拉框朝上的样式处理最后还是放弃了锚点定位的候补位置方法。

不过，现在，Chrome 143+新支持了一个特性，可以实现锚定容器回退检测了。

### 二、回退检测语法的使用

CSS锚点定位的回退检测使用很简单。

第一步，设置容器类型为`anchored`，同时指定自动回退的方位类型，这个对应的CSS属性是`position-try-fallbacks`属性，例如设置边界自动垂直翻转：

```
.float-element {
  position-try-fallbacks: flip-block;
  container-type: anchored;
}
```

第二步，使用`@container` `anchored()`函数进行匹配，示意：

```
@container anchored(fallback: flip-block) {
  .float-element {
    /* 如果垂直定位方向改变，如何如何…… */
  }
}
```

就可以了！

### 三、fallback案例

最好的学习方法还是案例，您可以狠狠地点击这里：[CSS锚点定位回退检测与tooltip效果实现demo](https://www.zhangxinxu.com/study/202512/css-container-anchored-fallback-demo.php)

在默认状态下，黑色提示框是在上面的，箭头朝下。

随着滚动进行，当提示框触碰到浏览器上边缘的时候，提示框的位置就会使用候补位置，垂直翻转，此时，大家可以看到箭头位置朝上了，这就是用的新的语法实现的。

以上效果需要Chrome 143+浏览器支持，如果您的浏览器不满足条件，也可以查看下面的GIF录屏效果，体会我说的意思。

![边缘自动翻转GIF](https://image.zhangxinxu.com/image/blog/202512/tips-fallback.gif)

#### 实现代码

HTML代码如下：

```
<span class="tooltip">我是提示内容</span>
<button class="anchor">我是按钮</button>
```

CSS完整代码：

```
.anchor {
  anchor-name: --my-anchor;
}

/* 提示浮层元素 */
.tooltip {
  position: fixed;
  margin-top: 1rem;
  position-anchor: --my-anchor;
  position-area: bottom;
  /* 垂直方向翻转 */
  position-try-fallbacks: flip-block;
  /* 设置为锚点查询容器类型 */
  container-type: anchored;

  /* 向上小三角效果 */
  &::before {
    content: '';
    position: absolute;
    bottom: 100%;
    inset-inline: 0;
    width: 1em;
    margin-inline: auto;
    aspect-ratio: 3/2;
    clip-path: polygon(50% 0, 100% 100%, 0% 100%);
    background: inherit;
  }
  background: #121212;
  color: #fff;
  padding: 4px 8px;
}

/* 如果触发了垂直翻转 */
@container anchored(fallback: flip-block) {
  .tooltip::before {
    /* 小三角翻转，定位调整 */
    scale: 1 -1;
    bottom: auto;
    top: 100%;
  }
}
```

### 四、点到为止、结语

关于CSS锚点定位容器查询，本文就先说这么多，不做进一步展开，为什么呢？

很简单，兼容性还很一般，目前Chrome 143才支持，而Chrome 143就是最近的正式版本。

![锚点位置查询兼容性](https://image.zhangxinxu.com/image/blog/202512/2025-12-22_084338.png)

考虑到我现在的年纪，不知道有生之年，有没有可能在正式项目中使用这个特性。

嘿嘿！

![嘿嘿一笑](https://image.zhangxinxu.com/image/blog/202512/2025-12-22_085135.jpg)

本文为原创文章，会经常更新知识点以及修正一些错误，因此转载请保留原出处，方便溯源，避免陈旧错误知识的误导，同时有更好的阅读体验。
本文地址：<https://www.zhangxinxu.com/wordpress/?p=11972>

（本篇完）

相关文章

* [介绍2022年最期待的CSS container容器查询](https://www.zhangxinxu.com/wordpress/2022/09/css-container-rule/) (0.438)
* [又发现一种无需绝对定位就可以元素重叠的CSS技巧](https://www.zhangxinxu.com/wordpress/2023/03/css-container-rule-overlap/) (0.438)
* [jQuery powerFloat万能浮动层下拉层插件](https://www.zhangxinxu.com/wordpress/2010/12/jquery-powerfloat%E4%B8%87%E8%83%BD%E6%B5%AE%E5%8A%A8%E5%B1%82%E4%B8%8B%E6%8B%89%E5%B1%82%E6%8F%92%E4%BB%B6/) (0.307)
* [js页面文字选中后分享到新浪微博实现](https://www.zhangxinxu.com/wordpress/2011/02/js%E9%A1%B5%E9%9D%A2%E6%96%87%E5%AD%97%E9%80%89%E4%B8%AD%E5%90%8E%E5%88%86%E4%BA%AB%E5%88%B0%E6%96%B0%E6%B5%AA%E5%BE%AE%E5%8D%9A%E5%AE%9E%E7%8E%B0/) (0.307)
* [告别JS浮层，全新的CSS Anchor Positioning锚点定位API](https://www.zhangxinxu.com/wordpress/2024/06/css-anchor-positioning-api/) (0.255)
* [聊聊Top Layer顶层特性的隐患与实践](https://www.zhangxinxu.com/wordpress/2024/06/web-top-layer/) (0.184)
* [好诶，select下拉框元素支持样式完全自定义啦！](https://www.zhangxinxu.com/wordpress/2025/07/css-checkmark-select-customizable/) (0.184)
* [CSS锚点定位实战-鼠标跟随交互效果](https://www.zhangxinxu.com/wordpress/2025/11/css-anchor-position-mouse-follow/) (0.184)
* [好奇心驱使下试验了chatGPT写CSS代码的能力](https://www.zhangxinxu.com/wordpress/2023/03/chatgpt-write-css/) (0.131)
* [CSS高宽不等图片固定比例布局的三重进化](https://www.zhangxinxu.com/wordpress/2023/07/css-image-aspect-ratio-layout/) (0.131)
* [大侠，请留步，要不过来了解下CSS Scroll Snap？](https://www.zhangxinxu.com/wordpress/2018/11/know-css-scroll-snap/) (RANDOM - 0.071)

«上一篇 [CSS progress()函数简介](https://www.zhangxinxu.com/wordpress/2025/12/css-progress-function/)

分享到：

标签： [@container](https://www.zhangxinxu.com/wordpress/tag/container/), [container-type](https://www.zhangxinxu.com/wordpress/tag/container-type/), [position-try-fallbacks](https://www.zhangxinxu.com/wordpress/tag/position-try-fallbacks/), [浮动层](https://www.zhangxinxu.com/wordpress/tag/%E6%B5%AE%E5%8A%A8%E5%B1%82/), [锚点](https://www.zhangxinxu.com/wordpress/tag/%E9%94%9A%E7%82%B9/), [锚点定位](https://www.zhangxinxu.com/wordpress/tag/%E9%94%9A%E7%82%B9%E5%AE%9A%E4%BD%8D/)

﻿

### 发表评论（目前没有评论）

[点击这里取消回复。](/wordpress/2025/12/css-anchor-container-query/#respond)

网站

![](//image.zhangxinxu.com/image/blog/zxx_240_0818.jpg)

张鑫旭[more](/life/about/)，09年[华中科技大学](http://www.hust.edu.cn/)毕业，现上海，就职于[阅文集团](//www.yuewen.com/)，专注web前端偏前领域，著有[《CSS世界》](https://item.jd.com/12262251.html)[《CSS选择器世界》](https://item.jd.com/12712370.html)[《CSS新世界》](https://item.jd.com/13356308.html)[《HTML并不简单》](https://item.jd.com/14213015.html)

邮箱：zhangxinxu@zhangxinxu.com

关注我：[微信](https://www.zhangxinxu.com/sp/wechat.html)[微博](//weibo.com/zhangxinxu)[掘金](//juejin.im/user/595315e7f265da6c322dc6c6)[知乎](//www.zhihu.com/people/iamzhangxinxu/activities)[抖音热更](https://www.zhangxinxu.com/sp/wechat.html?douyin=1)[B站](https://space.bilibili.com/31556431/)[Gitee](https://gitee.com/zhangxinxu/)

* ## 文章分类

  + [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/) (459)
  + [Design相关](https://www.zhangxinxu.com/wordpress/category/ps/) (13)
  + [Graphic相关](https://www.zhangxinxu.com/wordpress/category/graphic/) (83)
    - [Canvas相关](https://www.zhangxinxu.com/wordpress/category/graphic/canvas-graphic/) (32)
    - [SVG相关](https://www.zhangxinxu.com/wordpress/category/graphic/svg-graphic/) (49)
  + [HTML相关](https://www.zhangxinxu.com/wordpress/category/html/) (66)
  + [JS相关](https://www.zhangxinxu.com/wordpress/category/js/) (292)
    - [jQuery相关](https://www.zhangxinxu.com/wordpress/category/js/jquery-about/) (46)
    - [JS API](https://www.zhangxinxu.com/wordpress/category/js/js-api/) (80)
    - [JS实例](https://www.zhangxinxu.com/wordpress/category/js/js%E5%AE%9E%E4%BE%8B/) (146)
  + [Mobile相关](https://www.zhangxinxu.com/wordpress/category/mobile/) (15)
  + [Web综合](https://www.zhangxinxu.com/wordpress/category/web/) (56)
  + [外文翻译](https://www.zhangxinxu.com/wordpress/category/%E5%A4%96%E6%96%87%E7%BF%BB%E8%AF%91/) (36)
* ## 文章存档

  文章存档

  选择月份
   2025年十二月  (3)
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
   ...