---
title: python版的mtr(traceroute for macOS)
url: https://blog.est.im/2026/stdout-04
source: est の 输入 输出和出入
date: 2026-01-15
fetch_date: 2026-01-16T03:34:50.340619
---

# python版的mtr(traceroute for macOS)

# [est の 输入 输出和出入](/)

* [Home](/)
* [RSS](https://feeds.feedburner.com/initiative)
* [About](/about)
* [Category](/category)

## python版的mtr(traceroute for macOS)

Posted 2026-01-16 | stdout

首先，我讨厌编译，我喜欢二进制，直到昨天我惊讶的发现macOS上一个 `yes` 命令都是接近100KB的大小。homebrew 一大坨东西还不一定每次都成功。

说起编译，这几天读到一些关于软件法律方面的风险。zhihu说如果你的工具的不针对“特定用途”，那么就可以用一定免责的说辞，但是如果你提供下载只能拿来恰好做某一件特别具体的事，那么工具的提供者就有连带责任。我想这也是为啥大部分开源软件都是提供源码吧。我这代码又不能直接用，开源是为了研究技术。你自己编译之后拿来敲不对劲的命令那是用户自己的选择了。

那么回到主题， `mtr` 作为居家旅行必备网络工具，它只提供源码分发。9年前研究过，用python写了demo，但是终究不是太成熟，现在有 AI ，几句话就完成了

<https://github.com/est/trpy>

使用方法是 `sudo python3 cli.py jd.com` 这样。 `-6` 可以强制使用 IPv6，`-c1` 可以每一跳只probe一次并退出。

过程中反复折腾的，居然是最后一个 hop 重复显示，和丢失的问题。没想到AI也犯糊涂。

不过需要sudo还是很蛋痛。有一些折衷的办法比如去读 `traceroute -d`， `mtr` 或者 iproute2 的debug输出，有空再折腾。

期间遇到n次无法编辑文件的情况，估计AI输出乱了。还有不知道为什么，Google Antigravity每次写完代码就打 `Aurora` 几个字到末尾。

现在基本框架有了，下一步支持点什么插件好呢。

## Comments