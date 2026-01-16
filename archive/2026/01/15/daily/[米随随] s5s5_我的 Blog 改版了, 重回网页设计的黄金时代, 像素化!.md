---
title: 我的 Blog 改版了, 重回网页设计的黄金时代, 像素化!
url: https://s5s5.me/4300
source: [米随随] s5s5
date: 2026-01-15
fetch_date: 2026-01-16T03:35:00.042469
---

# 我的 Blog 改版了, 重回网页设计的黄金时代, 像素化!

[# [米随随] s5s5

// 本站是基于人类交互的，您的想象决定了我们的应用！](/)   [Home](/)   [First Category](/category/first-category)  [Other](/category/other)   [About](/about)

### :: MUSIC

Song: Auld Lang Syne - Ultimate Year End - Nhạc Giáng Sinh 2024 - AudioBay\_BGM

MENU

▷

|<

>|

### :: BADGES

♒

♂

⇒

Ω

中

☃

⌕

⛓

### :: SETTINGS

THEME: AUTO

### :: STATS

LAST UPDATED

20260115

DAYS ONLINE: -

POSTS: 1557

COMMENTS:  -

SINCE: 2003

多云 PARTLY CLOUDY ⛅

## // 我的 Blog 改版了, 重回网页设计的黄金时代, 像素化!

POSTED ON
 [2026-01-15 17:24](/4300)
// BY s5s5 // IN
 [First Category](/category/first-category)
//
 [-
评论](/4300#comments)

Web Design Museum 说 2001 - 2004 年是 [网页设计的黄金时代 Golden Age of Web Design](https://www.webdesignmuseum.org/golden-age-of-web-design) , 里面有很多令人惊叹的像素化网站设计, 我的 Blog 也是为了像素化让 Google Gemini 动用了几个亿的 token 来设计, 完美的重现当年流行的像素化设计风格, 还重现了日历, 天气, 播放器等当年流行的 Blog 元素, 真可谓是重回 2002 啊.

OK, 吹了这么多, 大家多多指点指点, 下面大家请访问 ---->>> <https://s5s5.me>

## 从 WordPress 到 Astro: 一场说走就走的迁移

### 数据导出

[WordPress](https://wordpress.org/) 用了十几年, 里面有一千多篇文章, 几千条评论. 还好 WordPress 自带导出功能, 直接导出 XML 格式, 文章和评论都在里面, 这一步倒是没费什么劲.

图片等静态资源就直接从 VPS 的网站目录下载下来, [rsync](https://rsync.samba.org/) 一把梭, 搞定.

### 为什么选 Astro

AI 建议的, 说实话一开始我也不太了解, 但试了一下发现真香:

* 天生适合博客这种内容型网站
* 所有文章用 Markdown 写, 版本管理友好
* 构建出来的是静态 HTML (SSG), 速度飞快
* 但又支持 SSR, 评论这种动态功能也能搞

最关键的是, [Astro](https://astro.build/) 没有自带评论系统, 刚好给我一个机会和 AI 一起从零搭建.

### 文章数据转换

用了 [wordpress-export-to-markdown](https://github.com/lonekorean/wordpress-export-to-markdown) 这个工具, 把 WordPress 导出的 XML 转成 Markdown 文件. 但转换后的 MD 并不完美, 各种问题:

* 表情图片没转成 Emoji
* 格式混乱, 有些 HTML 标签还在
* 错别字 (原来就有的…)
* 音频视频播放有问题

后面用了 AI 帮写小脚本来处理, 还用了 [豆包大模型](https://www.volcengine.com/product/doubao) 做错别字修复和格式优化, 效果不错. 最后再人工过了一遍所有文章, 毕竟是二十几年的记忆, 过一遍也挺有意思.

## 自建评论系统

这是整个改版最折腾的部分, 也是最有成就感的部分.

### 技术选型

| 组件 | 选择 | 原因 |
| --- | --- | --- |
| 数据库 | [SQLite](https://www.sqlite.org/) | 单文件, 部署简单, 不用额外服务 |
| ORM | [Drizzle](https://orm.drizzle.team/) | TypeScript 原生, 类型安全 |
| 认证 | [Better Auth](https://www.better-auth.com/) | 轻量, 支持 Google OAuth, 无状态会话 |

数据库一开始用的是 [Cloudflare D1](https://developers.cloudflare.com/d1/), 后来考虑国内访问, 还是 SQLite 更适合我这个小站, 数据完全自控.

### 数据迁移

从 WordPress 的 XML 转成数据库数据, 这里踩了个坑. 评论是嵌套的, 有 `parent_id`, 我一开始让 AI 一把梭, 结果层级关系全乱了.

正确的做法是分层导入:

1. 先导入所有顶级评论 (`parent_id` = 0)
2. 再导入第一层回复
3. 然后第二层、第三层…

AI 写了几个 SQL 迁移脚本, 配合 Python 小工具, 总算把评论数据完整迁移过来了. 清理评论格式也是个体力活, 二十几年积累的各种评论比较多…

### 安全措施

评论系统安全很重要, 做了这些防护:

* **蜜罐反垃圾**: 隐藏字段检测机器人
* **字段混淆**: 用混淆字段代替正常字段, 增加解析难度
* **XSS 防护**: [`sanitize-html`](https://www.npmjs.com/package/sanitize-html) 清洗输入, 只留基础标签
* **IP/UA 记录**: 方便追溯垃圾评论来源
* **禁止链接**: 垃圾评论 90% 为了留外链, 干脆一刀切

### 评论管理

后台可以分页浏览所有评论, 改状态 (public/pending/spam/deleted), 用 [Google OAuth](https://developers.google.com/identity/protocols/oauth2) 登录, 只有我的账号能进.

### 在线编辑器

除了评论管理, 后台还有个 **Markdown Studio** 在线编辑器, 支持实时预览和本地自动保存, 方便写文章.

## 系统架构

整个系统的架构长这样:

```
flowchart TB
    subgraph Client["🌐 Client (Browser)"]
        Browser[浏览器]
    end

    subgraph Astro["⚡ Astro SSR (Node.js)"]
        subgraph Layer1["请求处理层"]
            Pages["📄 Pages
(路由层)"]
            Components["🧩 Components
(视图层)"]
            Middleware["🔗 Middleware
(数据库初始化/请求拦截)"]
        end

        subgraph Layer2["业务逻辑层"]
            Actions["⚙️ Astro Actions
(服务端动作 - 表单处理/API)"]
        end

        subgraph Layer3["核心服务层"]
            Auth["🔐 Auth
(better-auth)"]
            DB["🗄️ DB
(Drizzle ORM)"]
            AI["🤖 AI
(OpenAI SDK)"]
            Logger["📊 Logger
(结构化日志)"]
        end
    end

    subgraph Storage["💾 持久化层"]
        SQLite[("SQLite Database
本地文件存储")]
    end

    Browser --> Pages
    Browser --> Components
    Pages --> Middleware
    Components --> Middleware
    Middleware --> Actions
    Actions --> Auth
    Actions --> DB
    Actions --> AI
    DB --> SQLite
```

### 用 Astro 的混合模式

Astro 的混合渲染策略真的很灵活:

| 页面类型 | 渲染模式 | 说明 |
| --- | --- | --- |
| 文章列表/详情 | SSG (静态) | 构建时生成, 速度最快 |
| 评论列表 | server:defer | 延迟服务端渲染, 首屏不等待 |
| 管理后台 | SSR | 每次请求动态渲染 |
| API 端点 | SSR | 动态处理请求 |

静态的归静态, 动态的归动态, 各司其职.

没用传统的 REST API, 用了 Astro Actions, 表单提交、状态更新都走 Actions, 写起来更简洁, 类型也更安全.

## 搞 UI: 和 AI 的拉扯艺术

说到 UI 这里 AI 这块还要讲究点技巧. 直接让 [Gemini](https://gemini.google.com/) 的画图来出设计稿是错误的, 画出来的都是些四不像.

但使用它的 Canvas 来直接把页面做出来就可以. 来回拉扯 PUA, 不断调整:

* “这个边框太粗了, 细一点”
* “背景色要更复古一点”
* “字体不够像素风”
* “阴影要硬边, 不要模糊”

最终就会得到你想要的网站设计了. 三栏布局, 左边是播放器和设置, 中间是内容, 右边是个人信息和日历, 完美复刻 2002 年的设计风格.

## 那些像素化的小组件

### 天气组件

Astro 推荐每篇文章有一个 hero image, 我想还不如显示天气呢. 于是做了个 Canvas 组件, 根据文章发布时的天气画不同的场景:

* sunny: 大太阳 + 火柴人
* rain: 下雨 + 打伞的火柴人
* snow: 下雪 + 雪人
* cloudy: 云朵飘过
* …

用了 528 行代码实现一个微型绘图引擎 `TinyP5`, 模仿 p5.js 的 API, 画这些像素风小动画.

### 日历组件

当年 Blog 的标配啊, 咱们得有. 点击日期可以跳转到那天发布的文章, 支持按月导航. AI 写这种代码非常麻利, 基本上描述清楚需求就能一把写出来.

数据在服务端按月分组, 传给客户端后渲染, 性能很好:

```
const postByMonth: Record<string, Array<{ date: string; link: string }>> = {};
// 按月分组，渲染某月时只遍历该月的数据
```

### 播放器组件

现在做播放器几行代码就行了, HTML5 的 `<audio>` 标签 + 一点 JS 就搞定. 样式做成了 iPod 风格的圆形控制盘, 复古味拉满.

支持播放/暂停、上一首/下一首, 播放完自动切下一首.

### 换肤组件

现在主打一个白天和黑夜自动切换. 默认跟系统走, 也可以手动切换.

用 CSS 变量管理主题色, 切换的时候改 `data-theme` 属性就行:

```
if (effectiveTheme === "dark") {
  htmlEl.setAttribute("data-theme", "dark");
} else {
  htmlEl.removeAttribute("data-theme");
}
```

天气 Canvas 也会监听主题变化, 自动重绘.

### 88x31 小 Logo

当年流行的 88x31 像素小 logo, 放在网站底部链接区, 互相交换的那种.

没有一个 AI 可以画出来, 太小了, AI 生成的都是糊的. 最后让 Gemini 用 Canvas 做了个生成 88x31 logo 的小工具, 像素级绘制. 现在都得用 2 倍大小 (176x62), 不然在 Retina 屏幕上会模糊.

## AI 辅助: 不止是写代码

AI 在这个项目里帮了很多忙:

* **架构设计**: 技术选型、目录结构、设计模式, AI 给了很多建议
* **代码生成**: 大部分组件代码都是 AI 写的, 我负责调整和优化
* **错别字修复**: 用豆包大模型的 API 批量修复文章里的错别字和格式
* **数据迁移脚本**: 各种小脚本, XML 解析、SQL 生成、格式转换
* **文档编写**: 架构文档、开发指南, AI 写初稿我来改

## 部署

### VPS 环境迁移

趁这次改版, 把 VPS 也整了一下. 原来跑的是 [CentOS](https://www.centos.org/) 7, 但 CentOS 已经 EOL 了, 新软件包都装不上, Node.js 版本也太老.

干脆重装成 [Debian](https://www.debian.org/) 13, 然后装上 [宝塔面板](https://www.bt.cn/):

* **Debian 13**: 稳定、更新及时、软件包新
* **宝塔面板**: 可视化管理 Nginx、Node.js、数据库, 省心

宝塔面板真的方便, 一键配置 SSL、反向代理、定时任务, 不用记各种命令了. 虽然命令行更 geek, 但我只是想写 Blog, 不是做运维…

### CI/CD 流程

本地写完文章或改完代码, 推到 GitHub 就完事了:

```
flowchart LR
    subgraph Local["💻 本地开发"]
        Write["✍️ 写文章/改代码"]
    end

    subgraph GitHub["🐙 GitHub"]
        Push["📤 Push 代码"]
        Action["⚡ GitHub Actions
(npm run build)"]
    end

    subgraph VPS["🖥️ 宝塔面板 (VPS)"]
        Sync["📥 同步构建产物"]
        Run["🚀 Node.js 运行"]
    end

    Write --> Push
    Push --> Action
    Action --> Sync
    Sync --> Run
```

### 用户访问架构

用户访问网站时的请求流程:

```
flowchart LR
    subgraph Internet["🌍 Internet"]
        User["👤 用户"]
    end

    subgraph Cloud["☁️ 腾讯云 EdgeOne"]
        EdgeOne["🚀 EdgeOne
(CDN + 安全防护)"]
    end

    subgraph VPS["🖥️ 宝塔面板 (VPS)"]
        Nginx["⚙️ Nginx
(反向代理)"]
        Node["📦 Node.js
(Astro SSR)"]
        SQLite[("💾 SQLite
数据库")]
    end

    User --> EdgeOne
    EdgeOne --> Nginx
    Nginx --> Node
    Node --> SQLite
```

* [EdgeOne](https://cloud.tencent.com/product/teo) 做 CDN 加速和安全防护
* Nginx 反向代理
* Node.js 跑 Astro SSR
* SQLite 数据库就一个文件, 备份方便

## 总结

这次改版花了不少时间, 但收获很大:

1. **彻底摆脱 WordPress**: 又慢, 又落后, 对 VPS 要求又高
2. **性能飞升**: 静态页面 + CDN, 速度不知道快了多少倍
3. **学习新技术**: Astro、Drizzle、Better Auth, 都是第一次用
4. **和 AI 协作**: 这可能是我用 AI 辅助最多的一个项目, 效率确实高
5. **重温经典**: 像素风设计让我想起二十几年前刚开始写 Blog 的日子

最后, 感谢你看到这里, 欢迎在下面留言, 测试一下我的评论系统 😄

米随随  岁次乙巳十一月廿七酉时
书于成都

[« 上一篇 年终总结 2025](/4299)

## // 评论列表 (-)

### 发表评论：

取消回复

发表评论

### :: PROFILE

![[米随随] s5s5](/logo.png)

Xiaochao Liu

> Front-end

> Loc: Chengdu, China

[[ LinkedIn ]](https://linkedin.com/in/s5s5) [[ Github ]](https://github.com/s5s5) [ Mail ]

### :: CALENDAR

< LOADING... >

| S | M | T | W | T | F | S |
| --- | --- | --- | --- | --- | --- | --- |

### :: LINK

![88x31 Button](https://assets.misuisui.com/assets/images/88x31-1.png)

![88x31 Button 2](https://assets.misuisui.com/assets/images/88x31-2.png)

© 2003-2026 [米随随] s5s5. ALL RIGHTS RESERVED.
  [蜀ICP备16014041号-1](https://beian.miit.gov.cn/)