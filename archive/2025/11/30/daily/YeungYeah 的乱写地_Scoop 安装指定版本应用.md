---
title: Scoop 安装指定版本应用
url: https://scottyeung.top/2025/scoop-install-specific-version-app/
source: YeungYeah 的乱写地
date: 2025-11-30
fetch_date: 2025-12-01T03:39:17.672501
---

# Scoop 安装指定版本应用

[YeungYeah 的乱写地](/)

* [关于](/about/)
* [文章](/posts/)
* [标签](/tags/)
* [开往](https://www.travellings.cn/go.html)
* [English](/en/)

[ ]

The content of this website is only deployed under the domain
𝙨𝙘𝙤𝙩𝙩𝙮𝙚𝙪𝙣𝙜.𝙩𝙤𝙥(scottyeung[dot]top). Any other domain sites are unauthorized
illegal mirrors.

# Scoop 安装指定版本应用

2025.11.30
 2025.11.30
 [Posts](/posts/)

切换到 Windows 环境，重新设置一些软件，在拉回博客设置 hugo 的时候，又遇到了[之前的 hugo 版本过高与本地博客主题不适配的问题](https://scottyeung.top/2025/degrade-homebrew-app-version/)，需要降级 hugo 的版本到指定版本。macOS 环境通过 Homebrew 管理应用，在 Windows 中通过 Scoop 来完成。

问了一下 Gemini，给的回复是 Scoop 也像 Homebrew 一样不支持安装指定版本的应用，但是可以通过指定 url 指定安装的配置文件，来安装旧版本的应用。

于是只要到 Scoop 的软件仓库，找到对应应用的配置文件，查看提交历史，找到历史版本的源文件 url 就可以了。

* 去 Scoop 的 GitHub 仓库（通常是 [ScoopMain](https://github.com/ScoopInstaller/Main/tree/master/bucket) 或 [ScoopExtras](https://github.com/ScoopInstaller/Extras/tree/master/bucket)）。
* 找到对应软件的 `.json` 文件（例如 `nodejs.json`）。
* 点击右上角的 **History** 查看提交历史。
* 找到你想要的那个版本的提交记录，点击查看文件。
* 点击 **Raw** 按钮，复制浏览器地址栏中的 URL。
* *URL 看起来像这样：* `https://raw.githubusercontent.com/.../commit_hash/.../app.json`

然后根据这个 url 进行安装。这样通过指定配置的 url 来安装，安装下来的 app 也没有办法更新，自动就锁了版本了。

|  |  |
| --- | --- |
| ``` 1 ``` | ``` scoop install https://raw.githubusercontent.com/ScoopInstaller/Main/4e72c5167244e249bbb063a93c28bb8aac034682/bucket/hugo-extended.json ``` |

但是这样找配置文件的历史版本，实际上是非常麻烦的，需要一页一页往后翻，如果版本旧点，得翻挺久。搜了一下，发现实际上 Scoop 是支持下载应用的指定版本的。下载指定版本后，可以通过 hold 命令禁止更新。

|  |  |
| --- | --- |
| ``` 1 2 3 4 ``` | ``` # Install specific version scoop install <app>@<version> # The scoop hold command prevents apps from being updated.  scoop hold <apps> ``` |

实际上这个命令在 Scoop 的 help 命令里面都能看到，Gemini 有点不太靠谱，在后面我发现了可以下载后，还是嘴硬不承认。

![](https://scottyeung.top/my-images/%7B0C842582-CFE1-488E-A12E-C0A4F71AA517%7D.png)

* 作者：[YeungYeah](https://scottyeung.top/)
* 链接：[https://scottyeung.top/2025/scoop-install-specific-version-app/](/2025/scoop-install-specific-version-app/)

## 相关文章：

* [Homebrew 降级安装指定版本应用](/2025/degrade-homebrew-app-version/)
* [系统工具替换之 Rust 化推进](/2023/replace-with-tools-based-rust/)
* [硬盘清理](/2022/reduce-usage-of-hard-disk/)
* [Logseq 移除空白 Journals 页面](/2025/logseq-remove-blank-journals/)
* [自建 Bitwarden 突然不可用](/2025/bitwarden-self-host-problem/)

[工具](/tags/%E5%B7%A5%E5%85%B7/)
[Windows](/tags/Windows/)
[经验](/tags/%E7%BB%8F%E9%AA%8C/)

* [注销网站备案 >](/2025/cancel-website-registration/)

© 2018–2025  YeungYeah