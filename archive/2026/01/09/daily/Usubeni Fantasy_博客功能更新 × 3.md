---
title: 博客功能更新 × 3
url: https://ssshooter.com/blog-upgrade/
source: Usubeni Fantasy
date: 2026-01-09
fetch_date: 2026-01-10T03:35:37.742631
---

# 博客功能更新 × 3

[skip to content](#main)

[![usubeni fantasy logo](/logo-mobile.png) Usubeni Fantasy](/)    [归档](/archive/)  [标签](/tags/)  [关于](/about/)  [友链](/links/)  [虫洞](https://www.foreverblog.cn/go.html)

Close

     Dark Theme

## 目录

* <#系列文章>
* <#相关文章>
* <#评论组件更新>

# 博客功能更新 × 3

2026/01/09  / 3 分钟阅读

[本站历史](/tag/%E6%9C%AC%E7%AB%99%E5%8E%86%E5%8F%B2/)  ,  [coding](/tag/coding/)

有种好久没更新[本站历史](https://ssshooter.com/tag/%E6%9C%AC%E7%AB%99%E5%8E%86%E5%8F%B2/)的感觉，最近有 3 个新功能还是得记一下。

## 系列文章

![](https://img.ssshooter.com/img/blog-upgrade/post-series.png)

使用方法：Frontmatter 中添加 `series` 和 `seriesOrder`

```
slug: "kitten-large-language-model-1"

publishDate: "2025-11-30T15:01:45.814Z"

title: "小猫都能懂的大模型原理 1 - 深度学习基础"

tags: ["大语言模型", "深度学习", "神经网络", "机器学习"]

description: "用最简单易懂的语言解释大语言模型的基本原理，从深度学习基础到神经网络训练，包含梯度下降、反向传播等核心概念，适合初学者的AI入门教程。"

series: "小猫都能懂的大模型原理"

seriesOrder: 1

useKatex: true
```

## 相关文章

![](https://img.ssshooter.com/img/blog-upgrade/recommend-posts.png)

使用方法：Frontmatter 中添加 `recommendTag`

```
slug: "2025-summary"

publishDate: "2025-12-29T08:49:10.000Z"

title: "2025 年终总结"

tags: ["diary", "年终总结"]

recommendTag: "年终总结"
```

## 评论组件更新

[Twikoo](https://github.com/twikoojs/twikoo) 似乎挺久没更新了，于是 [Fork](https://github.com/SSShooter/twikoo) 了一份。Twikoo 本身用的还是 Vue2，本来想顺便把它升级成 Vue3，但是迁移起来比想象中的麻烦（而且我还发现自己已经把 Vue2 的使用方式**忘掉一大半了**😂），如果再过几年它依然没更新的话再重构吧。

最后就只是改成 Vite 构建，添加了**主题色功能**，顺便做了一点 UI 微调。

![](https://img.ssshooter.com/img/blog-upgrade/twikoo-update.jpg)

如果你也想使用，只要安装依赖：

Terminal window

```
pnpm i tttwikoo
```

然后在 `global.css` 中添加如下代码即可：

```
#twikoo {

--tk-primary-color-rgb: 203, 42, 66;

}

:root[data-theme="dark"] #twikoo {

--tk-primary-color-rgb: 232, 120, 142;

}
```

太好了，UI 终于统一起来了，从 [cactus](https://github.com/chrismwilliams/astro-theme-cactus) Fork 出来也这么久了，更新了不少，晚点也开源一下好了🤗

评论组件加载中……

© SSShooter 2026.[🚀 Usubeni Fantasy](/)

  [归档](/archive/)  [标签](/tags/)  [关于](/about/)  [友链](/links/)  [虫洞](https://www.foreverblog.cn/go.html)