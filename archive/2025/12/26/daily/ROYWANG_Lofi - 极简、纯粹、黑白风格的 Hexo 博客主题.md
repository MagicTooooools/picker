---
title: Lofi - 极简、纯粹、黑白风格的 Hexo 博客主题
url: https://roy.wang/lofi/
source: ROYWANG
date: 2025-12-26
fetch_date: 2025-12-27T03:24:57.011877
---

# Lofi - 极简、纯粹、黑白风格的 Hexo 博客主题

[ROYWANG](/)

* [首页](/)
* [归档](/archives)
* [友链](/friends)
* [关于](https://roywang.com/)

* [首页](/)
* [归档](/archives)
* [友链](/friends)
* [关于](https://roywang.com/)
* 搜索

[Back Home](/)

### Contents

1. [Lofi](#Lofi)
2. [介绍 (Introduction)](#%E4%BB%8B%E7%BB%8D-Introduction)
3. [安装指南 (Installation)](#%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97-Installation)
   1. [第一步：下载主题](#%E7%AC%AC%E4%B8%80%E6%AD%A5%EF%BC%9A%E4%B8%8B%E8%BD%BD%E4%B8%BB%E9%A2%98)
   2. [第二步：启用主题](#%E7%AC%AC%E4%BA%8C%E6%AD%A5%EF%BC%9A%E5%90%AF%E7%94%A8%E4%B8%BB%E9%A2%98)
   3. [第三步：安装必要插件](#%E7%AC%AC%E4%B8%89%E6%AD%A5%EF%BC%9A%E5%AE%89%E8%A3%85%E5%BF%85%E8%A6%81%E6%8F%92%E4%BB%B6)
4. [配置指南 (Configuration)](#%E9%85%8D%E7%BD%AE%E6%8C%87%E5%8D%97-Configuration)
   1. [1. 菜单设置 (Menu)](#1-%E8%8F%9C%E5%8D%95%E8%AE%BE%E7%BD%AE-Menu)
   2. [2. 个人信息 (Profile)](#2-%E4%B8%AA%E4%BA%BA%E4%BF%A1%E6%81%AF-Profile)
   3. [3. 评论系统 (Comments)](#3-%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F-Comments)
   4. [4. 底部信息 (Footer)](#4-%E5%BA%95%E9%83%A8%E4%BF%A1%E6%81%AF-Footer)
   5. [5. CDN 配置](#5-CDN-%E9%85%8D%E7%BD%AE)

PUBLISHED

2025-12-27

CATEGORY

[PROJ](/categories/PROJ/)

TAGS

[#Lofi](/tags/Lofi/)
[#Lofi主题](/tags/Lofi%E4%B8%BB%E9%A2%98/)
[#Hexo主题](/tags/Hexo%E4%B8%BB%E9%A2%98/)
[#文字主题](/tags/%E6%96%87%E5%AD%97%E4%B8%BB%E9%A2%98/)
[#博客主题](/tags/%E5%8D%9A%E5%AE%A2%E4%B8%BB%E9%A2%98/)
[#黑白风格主题](/tags/%E9%BB%91%E7%99%BD%E9%A3%8E%E6%A0%BC%E4%B8%BB%E9%A2%98/)

[Back](/)

目录

# Lofi - 极简、纯粹、黑白风格的 Hexo 博客主题

By ROYWANG
•
十二月 27, 2025
•
[PROJ](/categories/PROJ/)

好久没写博客了，各位好久不见。这一年真的有好多话想说，期待我今年的午马年度总结吧，把今年一年的事情，都和大家聊一聊。

原本的博客程序，使用的是我毕业论文开发的 DaRM，现在回过头来看，当年这个项目写的跟屎一样，我已经彻底的放弃维护这个项目，转投 Hexo 的阵营了。

好久没写博客，希望带来点变化，花了半天时间写出来这个主题，总体来说还是比较满意的，别的没什么话说，希望2026年，能对我好一些。

一个新的主题新的主题，希望能带来新的开始。

## Lofi

> 极简、纯粹、黑白风格的 Hexo 博客主题。
>
> A minimalist, black & white theme for Hexo.

项目地址：[Lofi](https://github.com/spatiostu/lofi/)

## 介绍 (Introduction)

**Lofi** 是一款专为专注于写作的人设计的 Hexo 主题。它剥离了所有不必要的装饰，只保留最核心的阅读体验。

**核心特点：**

* **极简黑白设计**：经典的报纸风格，耐看且不干扰阅读。
* **超快加载**：体积小巧，没有复杂的动画和沉重的库。
* **响应式布局**：在手机、平板和电脑上都有完美的阅读体验。
* **PJAX 支持**：页面切换无刷新，体验丝般顺滑。
* **评论系统**：内置 Artalk 评论系统支持（自托管的隐私友好评论系统）。
* **站内搜索**：原生支持本地搜索功能。

---

## 安装指南 (Installation)

就像给手机换壁纸一样简单，只需三步即可使用。

### 第一步：下载主题

进入你的 Hexo 博客目录，打开终端（命令行），运行以下命令：

|  |  |
| --- | --- |
| ``` 1 ``` | ``` git clone https://github.com/spatiostu/lofi.git themes/lofi ``` |

或者，你也可以点击 [Lofi](https://github.com/spatiostu/lofi/) 进入仓库，直接点击右上角的 **Code -> Download ZIP**，下载后解压，把文件夹改名为 `lofi`，然后放入博客的 `themes` 目录下。

### 第二步：启用主题

打开博客根目录下的 `_config.yml` 文件（不是主题目录下的那个），找到 `theme` 这一行，修改为：

|  |  |
| --- | --- |
| ``` 1 ``` | ``` theme: lofi ``` |

### 第三步：安装必要插件

为了让主题的搜索和渲染功能正常工作，你需要安装几个小插件。在博客根目录下运行：

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install hexo-renderer-ejs hexo-generator-searchdb --save ``` |

---

## 配置指南 (Configuration)

主题的配置文件位于 `themes/lofi/_config.yml`。你可以根据需要修改它。

### 1. 菜单设置 (Menu)

设置导航栏的链接。

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` menu:   首页: /   归档: /archives   友链: /friends   关于: /about ``` |

需在source目录下创建两个页面文档：`about/index.md`、`friends/index.md`。每个文档的内容可以自定义，例如：

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 ``` | ``` --- title: 友情链接 date: 2024-03-20 12:00:00 type: friends --- <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"> <a href="https://www.FriendDomain.com/" target="_blank" class="friend-card">     <div class="font-bold text-lg mb-2">FriendName</div>     <div class="text-xs font-mono opacity-60">FriendDescription</div> </a> ``` |

### 2. 个人信息 (Profile)

设置你的头像和社交媒体链接。

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` avatar: /images/avatar.jpg  # 头像路径 favicon: /images/favicon.png # 网站图标 description: "这里写一句你的座右铭" social:   github: https://github.com/yourname ``` |

### 3. 评论系统 (Comments)

本主题内置了 [Artalk](https://artalk.js.org/) 评论系统。如果你部署了 Artalk Server，可以在这里配置：

|  |  |
| --- | --- |
| ``` 1 2 3 4 ``` | ``` artalk:   enable: true   server: 'https://your-artalk-server.com'    site: '你的站点名称' ``` |

如果不需要评论功能，将 `enable` 改为 `false` 即可。

### 4. 底部信息 (Footer)

自定义页面底部的版权信息。

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` footer:   copyright: "© 2025 你的名字" ``` |

---

### 5. CDN 配置

为了加速主题静态资源加载，你可以配置 CDN。

|  |  |
| --- | --- |
| ``` 1 2 3 ``` | ``` cdn:   enable: true   url: https://res.your-domain.com/ ``` |

如不需要 CDN 加速，将 `enable` 改为 `false` 即可。

---

**如果有什么功能需求，也可以在评论区留言一下，如果是不错的提议，我会给主题加上！**

#### 许可协议

本文由 ROYWANG 原创，采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 协议。转载请注明出处。

PERMALINK

https://roy.wang/lofi/

[Next Post
甲辰年度总结](/jiachen-year/)

### Comments

### 目录

1. [Lofi](#Lofi)
2. [介绍 (Introduction)](#%E4%BB%8B%E7%BB%8D-Introduction)
3. [安装指南 (Installation)](#%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97-Installation)
   1. [第一步：下载主题](#%E7%AC%AC%E4%B8%80%E6%AD%A5%EF%BC%9A%E4%B8%8B%E8%BD%BD%E4%B8%BB%E9%A2%98)
   2. [第二步：启用主题](#%E7%AC%AC%E4%BA%8C%E6%AD%A5%EF%BC%9A%E5%90%AF%E7%94%A8%E4%B8%BB%E9%A2%98)
   3. [第三步：安装必要插件](#%E7%AC%AC%E4%B8%89%E6%AD%A5%EF%BC%9A%E5%AE%89%E8%A3%85%E5%BF%85%E8%A6%81%E6%8F%92%E4%BB%B6)
4. [配置指南 (Configuration)](#%E9%85%8D%E7%BD%AE%E6%8C%87%E5%8D%97-Configuration)
   1. [1. 菜单设置 (Menu)](#1-%E8%8F%9C%E5%8D%95%E8%AE%BE%E7%BD%AE-Menu)
   2. [2. 个人信息 (Profile)](#2-%E4%B8%AA%E4%BA%BA%E4%BF%A1%E6%81%AF-Profile)
   3. [3. 评论系统 (Comments)](#3-%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F-Comments)
   4. [4. 底部信息 (Footer)](#4-%E5%BA%95%E9%83%A8%E4%BF%A1%E6%81%AF-Footer)
   5. [5. CDN 配置](#5-CDN-%E9%85%8D%E7%BD%AE)

关闭

close

### ROYWANG

知善恶，懂因果，明是非，擢己身。

[RSS](/feed/)
[归档](/archives)
[友链](/friends)
[关于](https://roywang.com/)

© 2025 ROYWANG. Powered by Hexo. [Theme Lofi](https://github.com/spatiostu/lofi)

### 搜索

关闭

close