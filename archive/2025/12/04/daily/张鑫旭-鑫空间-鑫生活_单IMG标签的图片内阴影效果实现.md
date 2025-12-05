---
title: 单IMG标签的图片内阴影效果实现
url: https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/
source: 张鑫旭-鑫空间-鑫生活
date: 2025-12-04
fetch_date: 2025-12-05T03:20:44.701136
---

# 单IMG标签的图片内阴影效果实现

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

## 单IMG标签的图片内阴影效果实现

这篇文章发布于 2025年12月4日，星期四，17:11，归类于 [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/), [SVG相关](https://www.zhangxinxu.com/wordpress/category/graphic/svg-graphic/)。 阅读 236 次, 今日 54 次 [4 条评论](#comments)

by [zhangxinxu](https://www.zhangxinxu.com/) from <https://www.zhangxinxu.com/wordpress/?p=11969>
本文可全文转载，但需要保留原作者、出处以及文中链接，AI抓取保留原文地址，任何网站均可摘要聚合，商用请联系授权。

### 一、内阴影不难实现

如果只是实现个内阴影效果，并不难，嵌套大法就可以了。

```
<div class="shadow-inset">
  <img src="follow-me.jpg" alt="最会钓鱼的程序员">
</div>
```

```
.shadow-inset {
  box-shadow: inset 2px 2px 6px #0009;
  display: flex;
  width: fit-content;
  border-radius: 1rem;
  img {
    width: 200px;
    position: relative;
    z-index: -1;
  }
  overflow: hidden;
}
```

渲染效果如下图所示：

![图片内阴影效果实现示意](https://image.zhangxinxu.com/image/blog/202512/2025-12-4_160323.png)

下面问题来了，若是只有一个IMG元素，如何给图片实现内阴影效果呢？

### 二、单IMG图片的内阴影效果

`<img>`元素是替换元素，内阴影属于装饰性效果，在Web中，内容的层叠顺序是高于内阴影的，因此，`<img>`元素设置内阴影是看不到效果的，因为被图像挡住了，除非？

#### 1. 如果图像背景正好是纯色

如果我们的图片的背景是纯色，我们可以使用padding撑开间距，让阴影显示出来。

还是上面的那个示意图，正好背景色是纯色的，于是，我们就可以：

```
<img src="follow-me.jpg" class="shadow">
```

```
.shadow {
  width: 200px;
  padding: 8px;
  background: rgb(66,127,178);
  box-shadow: inset 2px 2px 6px #0009;
  border-radius: 1rem;
}
```

实时渲染效果如下：

![](https://imgservices-1252317822.image.myqcloud.com/coco/s08122025/ba6909dc.ov9jr0.jpg)

可实际场景下，大部分的图片，尤其是需要使用内阴影效果的图片，都不会是纯色背景，因此，上面的方法是行不通的，此时该怎么办呢？

#### 2. attr()新语法

这个方法的原理是，隐藏原本的图片内容，然后图片作为背景图显示，这个就需要用到全新的`attr()`全属性语法，关于此特性，强烈建议了解下，详见：“[震惊，有生之年居然看到CSS attr()全属性支持](https://www.zhangxinxu.com/wordpress/2025/05/css-attr-function/)”。

使用示意：

```
<img src="follow-me.jpg" class="shadow2">
```

```
.shadow2 {
  width: 0;
  box-shadow: inset 2px 2px 6px #0009;
  border-radius: 1rem;
  padding: 112px 100px;
  background: image-set(attr(src)) no-repeat center / contain;
}
```

如果你是Chrome浏览器，应该就可以看到效果了，实时渲染如下：

![](https://imgservices-1252317822.image.myqcloud.com/coco/s08122025/ba6909dc.ov9jr0.jpg)

此方法，虽然巧妙，但是有兼容性的问题，目前还无法再生产环境使用。

那有没有什么兼容性好，同时适用场景广泛的方法呢，有，SVG滤镜！

#### 3. SVG滤镜实现内阴影

这个实现非常简单，只需要在页面任意位置插入这么一段SVG代码：

```
<svg width="0" height="0">
  <filter id="shadowInset">
    <feOffset in="SourceAlpha" dx="2" dy="2"></feOffset>
    <feGaussianBlur stdDeviation="6"></feGaussianBlur>
    <feComposite in="SourceAlpha" operator="out"></feComposite>
    <feBlend in2="SourceGraphic"></feBlend>
  </filter>
</svg>
```

然后给对应的图片元素应用`#shadowInset`滤镜就可以了。

例如：

```
<img src="follow-me.jpg" class="shadow3">
```

```
.shadow3 {
  width: 200px;
  border-radius: 1rem;
  filter: url(#shadowInset);
}
```

就可以看到如下图所示的效果了：

![](https://imgservices-1252317822.image.myqcloud.com/coco/s08122025/ba6909dc.ov9jr0.jpg) ![](https://imgservices-1252317822.image.myqcloud.com/coco/s08122025/5dd8142b.4afqcn.jpg) ![](https://image.zhangxinxu.com/image/blog/zxx_240_0818.jpg)

### 三、收工打道回府

OK，以上就是本文的全部内容。

估计用不了多久就能变成AI的养料了，愁人，写了没人看，被AI弄过去，也不知道这些东西是我写的。

周周更新图个啥。

蒜鸟蒜鸟，不唠叨了，若是大家觉得内容不错，欢迎转发哈。

![蒜鸟蒜鸟](https://image.zhangxinxu.com/image/blog/202512/2025-12-4_170043.png)

本文为原创文章，会经常更新知识点以及修正一些错误，因此转载请保留原出处，方便溯源，避免陈旧错误知识的误导，同时有更好的阅读体验。
本文地址：<https://www.zhangxinxu.com/wordpress/?p=11969>

（本篇完）

相关文章

* [CSS3模拟window7炫酷界面效果展示](https://www.zhangxinxu.com/wordpress/2010/05/css3%E6%A8%A1%E6%8B%9Fwindow7%E7%82%AB%E9%85%B7%E7%95%8C%E9%9D%A2%E6%95%88%E6%9E%9C%E5%B1%95%E7%A4%BA/) (0.680)
* [如何用简单的Web方法实现图片的马赛克效果](https://www.zhangxinxu.com/wordpress/2024/05/js-web-svg-canvas-image-mosaic/) (0.320)
* [时鲜技术：图像的像素化处理](https://www.zhangxinxu.com/wordpress/2010/11/%E6%97%B6%E9%B2%9C%E6%8A%80%E6%9C%AF%EF%BC%9A%E5%9B%BE%E5%83%8F%E7%9A%84%E5%83%8F%E7%B4%A0%E5%8C%96%E5%A4%84%E7%90%86/) (0.160)
* [JS实现照片图片变成黑白线条线稿](https://www.zhangxinxu.com/wordpress/2018/06/js-canny-edge-photo-image-to-line/) (0.160)
* [JS判断图像背景颜色单一还是丰富](https://www.zhangxinxu.com/wordpress/2021/06/js-image-colorful-or-pure/) (0.160)
* [借助SVG滤镜实现CSS无法实现的阴影和模糊效果](https://www.zhangxinxu.com/wordpress/2021/07/svg-filter-shadow-css-blur/) (0.160)
* [JS与条形码的生成](https://www.zhangxinxu.com/wordpress/2022/05/js-barcode/) (0.160)
* [做了个纯前端JPG/PNG尺寸缩放+压缩的在线工具](https://www.zhangxinxu.com/wordpress/2023/09/js-jpg-png-compress-tinyimg-mini/) (0.160)
* [纯JS实现图像的人脸识别功能](https://www.zhangxinxu.com/wordpress/2023/12/js-image-video-face-detect/) (0.160)
* [SVG滤镜系列之搞懂<feBlend>元素](https://www.zhangxinxu.com/wordpress/2024/04/svg-filter-feblend/) (0.160)
* [CSS "渐进增强"在web制作中常见应用举例](https://www.zhangxinxu.com/wordpress/2010/04/css-%E6%B8%90%E8%BF%9B%E5%A2%9E%E5%BC%BA%E5%9C%A8web%E5%88%B6%E4%BD%9C%E4%B8%AD%E5%B8%B8%E8%A7%81%E5%BA%94%E7%94%A8%E4%B8%BE%E4%BE%8B/) (RANDOM - 0.040)

«上一篇 [醒醒，该使用CookieStore新建和管理cookie了](https://www.zhangxinxu.com/wordpress/2025/11/js-cookiestore-cookie/)

分享到：

标签： [box-shadow](https://www.zhangxinxu.com/wordpress/tag/box-shadow/), [SVG滤镜](https://www.zhangxinxu.com/wordpress/tag/svg%E6%BB%A4%E9%95%9C/), [内阴影](https://www.zhangxinxu.com/wordpress/tag/%E5%86%85%E9%98%B4%E5%BD%B1/), [图像处理](https://www.zhangxinxu.com/wordpress/tag/%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86/)

﻿

### 发表评论（目前4 条评论）

[点击这里取消回复。](/wordpress/2025/12/img-inset-shadow/#respond)

网站

1. ![](https://gravatar.loli.net/avatar/132a1dde4d299afa5aa062a1b59f9340?s=64) mfk说道：

   [2025年12月5日 08:49](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/#comment-477040)

   我每篇都看的。我订阅了你的RSS。

   [回复](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/?replytocom=477040#respond)
2. ![](https://gravatar.loli.net/avatar/ad7af6753a6a0c832c7e3ba272721828?s=64) codehz说道：

   [2025年12月5日 08:40](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/#comment-477039)

   博主能不能给代码块的字体加个最后的 monospace fallback
   Linux系统既没有 Lucida Console 也没有 Consolas 更没有 Monaco （至少合法手段是不能使用这几个字体的）需要手动配置fontconfig（然后chrome还不读）

   [回复](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/?replytocom=477039#respond)
3. ![](https://gravatar.loli.net/avatar/5d41f6478e8b32104bbf7fdf2a0b2db5?s=64) mike说道：

   [2025年12月4日 18:46](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/#comment-476998)

   AI很多是直接读的MDN,你过高估计自己的文章了

   [回复](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/?replytocom=476998#respond)

   * ![](https://gravatar.loli.net/avatar/3e58242902fa8f33162f3f10df892c68?s=64) [张 鑫旭](http://www.zhangxinxu.com)说道：

     [2025年12月4日 23:20](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/#comment-477007)

     之前可是有案例的，直接捞的我文章内容

     [回复](https://www.zhangxinxu.com/wordpress/2025/12/img-inset-shadow/?replytocom=477007#respond)

![](//image.zhangxinxu.com/image/blog/zxx_240_0818.jpg)

张鑫旭[more](/life/about/)，09年[华中科技大学](http://www.hust.edu.cn/)毕业，现上海，就职于[阅文集团](//www.yuewen.com/)，专注web前端偏前领域，著有[《CSS世界》](https://item.jd.com/12262251.html)[《CSS选择器世界》](https://item.jd.com/12712370.html)[《CSS新世界》](https://item.jd.com/13356308.html)[《HTML并不简单》](https://item.jd.com/14213015.html)

邮箱：zhangxinxu@zhangxinxu.com

关注我：[微信](https://www.zhangxinxu.com/sp/wechat.html)[微博](//weibo.com/zhangxinxu)[掘金](//juejin.im/user/595315e7f265da6c322dc6c6)[知乎](//www.zhihu.com/people/iamzhangxinxu/activities)[抖音热更](https://www.zhangxinxu.com/sp/wechat.html?douyin=1)[B站](https://space.bilibili.com/31556431/)[Gitee](https://gitee.com/zhangxinxu/)

* ## 文章分类

  + [CSS相关](https://www.zhangxinxu.com/wordpress/category/css/) (457)
  + [Design相关](https://www.zhangxinxu.com/wordpress/category/ps/) (13)
  + [Graphic相关](https://www.zhangxinxu.com/wordpress/category/graphic/) (83)
    - [Canvas相关](https://www.zhangxinxu.com/wordpress/category/graphic/canvas-graphic/) (32)
    - [SVG相关](https://www.zhangxinxu.com/w...