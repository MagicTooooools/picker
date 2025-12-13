---
title: CSS progress()函数简介
url: https://www.zhangxinxu.com/wordpress/2025/12/css-progress-function/
source: 张鑫旭-鑫空间-鑫生活
date: 2025-12-12
fetch_date: 2025-12-13T03:20:42.906839
---

# CSS progress()函数简介

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

## CSS progress()函数简介

这篇文章发布于 2025年12月12日，星期五，19:15，归类于 [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/)。 阅读 162 次, 今日 30 次 [没有评论](#comments)

by [zhangxinxu](https://www.zhangxinxu.com/) from <https://www.zhangxinxu.com/wordpress/?p=11969>
本文可全文转载，但需要保留原作者、出处以及文中链接，AI抓取保留原文地址，任何网站均可摘要聚合，商用请联系授权。

### 一、progress()函数语法

最近CSS又出了个新的函数，名叫`progress()`，返回`0-1`的进度值，我研究了下，这个函数还是有一定的实用性的。

我们先看下他的语法：

```
progress(<value>, <start>, <end>)
```

这个函数支持3个参数，分别是：

**value**
:   当前进度值

**start**
:   范围起始值

**end**
:   范围结束值

因此：

* `progress(300, 0, 1000)`的返回值就是`0.3`；
* `progress(50px, 0px, 100px)`的返回值就是`0.5`；
* `progress(50%, 30%, 80%)`的返回值就是……这个要计算下：`(50% - 30%) / 80%`，结果是`0.25`。

需要注意的是，`progress()`的各个参数的计算值类型需要一致，如果一个是时间值，一个是长度值，或者一个是数值，一个是单位值，是不合法的，下面这两个`progress()`语句就是非法的：

```
progress(3s, 0px, 100px)
progress(3em, 0, 100)
```

### 二、progress()场景应用

在实际开发中，`progress()`函数都是与CSS变量，或者是CSS相对单位结合使用的，因为只有此时，其返回值才是动态的，才有使用的意义，否则，类似`progress(300, 0, 1000)`这样的固定值，还不如直接写个`0.3`来得快活呢！

例如：

```
// CSS变量
progress(var(--container-width), 320, 1200)
// 相对单位
progress(100vw, 360px, 1024px)
```

#### 案例

官方案例都是使用`vw`单位，但是这个不太方便看文章的小伙伴体验。

所以，我打算使用`cqw`单位。

HTML如下，一个容器，里面有一个图片：

```
<div class="container">
  <img src="mybook.jpg" alt="CSS新世界">
</div>
```

此时，我们就可以使用`progress()`函数，让图片的宽度基于容器尺寸动态变化，CSS代码如下：

```
.container {
  padding: 10px;
  border: dashed gray;
  container-type: inline-size;
  overflow: hidden;
  resize: both;

  img {
    width: calc(100px + 200px * progress(80cqw, 150px, 800px));
  }
}
```

`80cqw`表示`.container`容器元素80%的宽度，然后这个宽度计算值和`150px-800px`范围计算进度，最终，图片的尺寸会在`100px~300px`变化。

实时渲染效果如下所示（需要Chrome 138+）（拖拽右下角的resize拖拽条）：

![CSS新世界](https://imgservices-1252317822.image.myqcloud.com/coco/s08122025/99e58160.b1b16e.jpg)

### 三、兼容性以及其他

`progress()`使用还挺灵活的，函数参数支持其他常见的CSS运算函数，例如`calc()`计算：

```
progress(calc(20 + 30), 0, 100)
```

也可以是`calc()`函数的参数，例如：

```
calc((progress(var(--container-width), 20%, 80%) / 2) + 0.5)
```

支持和其他CSS函数，例如`random()`、`clamp()`混合使用。

总之，哪里需要他，就使用他。

#### 边界特性

7年前，我写过一篇文章“[CSS文字和背景color自动配色技术简介](https://www.zhangxinxu.com/wordpress/2018/11/css-background-color-font-auto-match/)”，利用的是`opacity`的边界特性实现的，比较取巧。

现在有了`progress()`函数，那就比较正统了，也更容易理解了。

例如：

```
// 亮度大于0.5，颜色黑色
// 亮度小于 0.5，颜色白色
color: hsl(0, 0%, calc(100% * progress(
  calc((0.5 - var(--lightness)) * infinity), 0, 1))
);
```

要是对`infinity`关键字感兴趣，可以访问这篇文章：“[了解infinity、pi等CSS calc()计算关键字](https://www.zhangxinxu.com/wordpress/2024/07/css-calc-keyword-infinity-pi-e/)”

#### 兼容性

兼容性一般，目前Chrome Only！

![progress()兼容性](https://image.zhangxinxu.com/image/blog/202512/2025-12-12_185651.png)

目前还只是个玩具。

#### 结语

`progress()` 是 CSS 中用于表示进度值的特殊函数（属于 CSS 数值函数范畴），核心作用是将「0~1 范围内的进度值」映射为可视化的数值 / 颜色 / 尺寸等，常用于进度条、动态过渡、动画关键帧等场景，是 CSS 原生实现进度关联样式的核心工具。

不过目前受制于兼容性，暂时无法大规模使用，大家了解即可！

最后，我家白衣慕沛灵希望大家多多点赞转发。

![白衣慕沛灵](https://image.zhangxinxu.com/image/blog/202512/2025-12-12_190135.jpeg)

本文为原创文章，会经常更新知识点以及修正一些错误，因此转载请保留原出处，方便溯源，避免陈旧错误知识的误导，同时有更好的阅读体验。
本文地址：<https://www.zhangxinxu.com/wordpress/?p=11969>

（本篇完）

相关文章

* [介绍2022年最期待的CSS container容器查询](https://www.zhangxinxu.com/wordpress/2022/09/css-container-rule/) (0.462)
* [CSS var变量的局部作用域（继承）特性](https://www.zhangxinxu.com/wordpress/2019/01/css-var-%E5%8F%98%E9%87%8F-%E5%B1%80%E9%83%A8/) (0.427)
* [CSS高宽不等图片固定比例布局的三重进化](https://www.zhangxinxu.com/wordpress/2023/07/css-image-aspect-ratio-layout/) (0.336)
* [寥寥数行SVG实现圆环loading或倒计时效果](https://www.zhangxinxu.com/wordpress/2015/07/svg-circle-loading/) (0.252)
* [更好的纯CSS滚动指示器技术实现](https://www.zhangxinxu.com/wordpress/2019/06/better-css-scroll-indicator/) (0.252)
* [小tip:CSS vw让overflow:auto页面滚动条出现时不跳动](https://www.zhangxinxu.com/wordpress/2015/01/css-page-scrollbar-toggle-center-no-jumping/) (0.189)
* [基于vw等viewport视区单位配合rem响应式排版和布局](https://www.zhangxinxu.com/wordpress/2016/08/vw-viewport-responsive-layout-typography/) (0.189)
* [CSS变量对JS交互组件开发带来的提升与变革](https://www.zhangxinxu.com/wordpress/2020/07/css-var-improve-components/) (0.175)
* [纯CSS实现未读消息超过100自动显示为99+](https://www.zhangxinxu.com/wordpress/2022/01/css-show-diff-content-according-var/) (0.175)
* [视区相关单位vw, vh..简介以及可实际应用场景](https://www.zhangxinxu.com/wordpress/2012/09/new-viewport-relative-units-vw-vh-vm-vmin/) (0.126)
* [巧用浏览器CSS属性值的不兼容向下兼容hack技巧](https://www.zhangxinxu.com/wordpress/2016/10/browser-css-property-down-compatible-hack-technology/) (RANDOM - 0.063)

«上一篇 [单IMG标签的图片内阴影效果实现](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/)

分享到：

标签： [calc](https://www.zhangxinxu.com/wordpress/tag/calc/), [cqw](https://www.zhangxinxu.com/wordpress/tag/cqw/), [css var](https://www.zhangxinxu.com/wordpress/tag/css-var/), [infinity](https://www.zhangxinxu.com/wordpress/tag/infinity/), [progress](https://www.zhangxinxu.com/wordpress/tag/progress/), [vw](https://www.zhangxinxu.com/wordpress/tag/vw/), [进度条](https://www.zhangxinxu.com/wordpress/tag/%E8%BF%9B%E5%BA%A6%E6%9D%A1/)

﻿

### 发表评论（目前没有评论）

[点击这里取消回复。](/wordpress/2025/12/css-progress-function/#respond)

网站

![](//image.zhangxinxu.com/image/blog/zxx_240_0818.jpg)

张鑫旭[more](/life/about/)，09年[华中科技大学](http://www.hust.edu.cn/)毕业，现上海，就职于[阅文集团](//www.yuewen.com/)，专注web前端偏前领域，著有[《CSS世界》](https://item.jd.com/12262251.html)[《CSS选择器世界》](https://item.jd.com/12712370.html)[《CSS新世界》](https://item.jd.com/13356308.html)[《HTML并不简单》](https://item.jd.com/14213015.html)

邮箱：zhangxinxu@zhangxinxu.com

关注我：[微信](https://www.zhangxinxu.com/sp/wechat.html)[微博](//weibo.com/zhangxinxu)[掘金](//juejin.im/user/595315e7f265da6c322dc6c6)[知乎](//www.zhihu.com/people/iamzhangxinxu/activities)[抖音热更](https://www.zhangxinxu.com/sp/wechat.html?douyin=1)[B站](https://space.bilibili.com/31556431/)[Gitee](https://gitee.com/zhangxinxu/)

* ## 文章分类

  + [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/) (458)
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
   2025年十二月  (2)
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
 ...