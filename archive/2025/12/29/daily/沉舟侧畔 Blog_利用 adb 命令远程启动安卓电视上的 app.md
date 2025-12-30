---
title: 利用 adb 命令远程启动安卓电视上的 app
url: https://springwood.me/adb-to-control-remote-android-tv/
source: 沉舟侧畔 Blog
date: 2025-12-29
fetch_date: 2025-12-30T03:32:31.447817
---

# 利用 adb 命令远程启动安卓电视上的 app

[Skip to content](#content)

[![沉舟侧畔 Blog](data:image/svg+xml...)![沉舟侧畔 Blog](https://springwood.me/wp-content/uploads/2024/10/treeLogo.png)](https://springwood.me/)

[沉舟侧畔 Blog](https://springwood.me)

⛵️ 新生的力量，生机勃勃 ⛵️

Primary Menu

* [📜 归档](https://springwood.me/archives/ "文章归档")
* [🔗 友链](https://springwood.me/link/ "友情链接")
* [👨‍💻 关于](https://springwood.me/about-me/ "关于本博客")
* [古诗词](https://poem.springwood.me "古诗词的日语读法")
* [🔒](https://springwood.me/wp-login.php?redirect_to=https%3A%2F%2Fspringwood.me%2Fadb-to-control-remote-android-tv%2F "登录")

![](data:image/svg+xml;base64...)![](https://springwood.me/wp-content/uploads/2025/02/emby.svg)

Posted in:
[PC/macOS](https://springwood.me/category/it/pc/), [TV](https://springwood.me/category/life/tv/), [Ubuntu/Linux](https://springwood.me/category/it/ubuntulinux/)

# 利用 adb 命令远程启动安卓电视上的 app

![](data:image/svg+xml;base64...)![](https://springwood.me/wp-content/litespeed/avatar/f75fb292c09179772e820625163b0d42.jpg?ver=1766562412)

Written by [springwood](https://springwood.me/author/billzt/)

[2025年12月29日2025年12月29日](https://springwood.me/adb-to-control-remote-android-tv/)

[利用 adb 命令远程启动安卓电视上的 app有 1 条评论](https://springwood.me/adb-to-control-remote-android-tv/#comments)

之前有篇文章里面写到我用家里的安卓电视搭建了 emby 服务器。

> [在安卓电视上安装 Emby 影音服务器](https://springwood.me/install-emby-server-on-android-tv/)

这样子有个问题是：emby server 这个 app 太笨重了。而安卓电视机虽然看着个头很大，配置却很渣，内存才 3GB。因此 emby server 有时候会被系统自动杀掉。这个“有时候”到底是什么时候，说不清，有时每隔几周，有时每隔几天。

如果发现服务不通的时候、人正好在家里，那简单，打开电视机把这个 server 重新运行一下就好了。可是如果那个时候不在家里就尴尬了。

询问了 ChatGPT，对方说了一大堆巴拉巴拉表示安卓电视机不适合搭建服务器，应该用台式电脑或者 NAS 等类似设备。但目前没这个条件。其实我也不需要保证它 7天\*24小时都正常在线，只需要找到一个办法、万一断线的时候也能够远程重启就可以了。这个办法找到了，就是 adb 命令。

Table of Contents

Toggle

* 在电脑上（macOS）进行 adb 远程操作
* 在手机上（iOS）进行 adb 远程操作

## 在电脑上（macOS）进行 adb 远程操作

android 官方提供 macOS 版本的 adb 命令行工具。第一次操作时需要人在家里直接对电视机操作，以后就不用了。

1. 在电视机上打开“开发者选项”（具体步骤略，AI 直接能搜到）
2. 在 macOS 电脑上，利用 adb 通过 IP 地址连接到电视机（具体步骤略，AI 直接能搜到）。这一步很像 SSH 。
3. 此时电视机上会出现对话框提醒你是否信任该设备，勾选“始终信任这台设备”，然后确定。以后就可以不用特意打开电视机了。
4. `adb shell cmd package resolve-activity --brief com.emby.embyserver` 利用这条命令查询 app 启动器的具体路径。此时会输出两行，第二行是 `com.emby.embyserver/XXXXXXXXXX.MainActivity`，这是下一步要用的内容。
5. `adb shell am start -n com.emby.embyserver/XXXXXXXXXX.MainActivity`，就能启动它了。

如果不在家里，则用 WireGuard 翻回家里的内网，就可以正常操作。听说在🇨🇳使用 WireGuard 非常困难（因为 WireGuard 基于 UDP，而🇨🇳特色的运营商会干扰 UDP），这就是润的好处了。

## 在手机上（iOS）进行 adb 远程操作

手机上且不说没有终端，就算有，也不可能让你安装 adb，怎么办呢？此时就要绕个弯：虽然手机上没有 adb，但是家里的路由器上可以有啊！而手机可以登录到路由器啊。

> [把红米 AX5 路由器刷成了 OpenWrt 系统](https://springwood.me/redmi-ax5-to-openwrt/)

> [我家的路由器和 NAS](https://springwood.me/my-routers/)

1. 在路由器上通过 OpenWrt 自带的包管理系统安装 adb。
2. 在手机上用浏览器登录 OpenWrt 管理页面，利用里面的 tty 终端窗口即可进行 adb 操作。

Tags:
[android](https://springwood.me/tag/android/), [iOS](https://springwood.me/tag/ios/), [macOS](https://springwood.me/tag/macos/)

## Comment (1) on "利用 adb 命令远程启动安卓电视上的 app"

1. ![](data:image/svg+xml;base64...)![](https://springwood.me/wp-content/litespeed/avatar/3dac22544d26b6d0b6e3a39fb992a220.jpg?ver=1766596738) **youfa**说道：

   [2025年12月30日 11:24](https://springwood.me/adb-to-control-remote-android-tv/#comment-23648)

   ![Google Chrome 141.0.0.0](data:image/svg+xml;base64... "Google Chrome 141.0.0.0")![Google Chrome 141.0.0.0](https://springwood.me/wp-content/plugins/wp-useragent/img/16/net/chrome.png "Google Chrome 141.0.0.0") ![GNU/Linux x64](data:image/svg+xml;base64... "GNU/Linux x64")![GNU/Linux x64](https://springwood.me/wp-content/plugins/wp-useragent/img/16/os/linux.png "GNU/Linux x64")

   可以做了脚本放在openwrt里用crontab定期检测
   如果电视上面的embyserver不在了, 就重新启动

   [回复](https://springwood.me/adb-to-control-remote-android-tv/?replytocom=23648#respond)

### 发表回复 [取消回复](/adb-to-control-remote-android-tv/#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

[ ]  在此浏览器中保存我的显示名称、邮箱地址和网站地址，以便下次评论时使用。

[x] 如果有人回复我的评论，请通过电子邮件通知我。

搜索：

[![](data:image/svg+xml;base64...)](https://springwood.me/feed/)

[![](data:image/svg+xml;base64...)](/cdn-cgi/l/email-protection#c5aba4a8acabaaaeaaa0f5f585a2a8a4aca9eba6aaa8)

[5G](https://springwood.me/tag/5g/)
[Apple Pay](https://springwood.me/tag/apple-pay/)
[App Store](https://springwood.me/tag/app-store/)
[ChatGPT](https://springwood.me/tag/chatgpt/)
[emoji](https://springwood.me/tag/emoji/)
[GFW](https://springwood.me/tag/gfw/)
[iOS](https://springwood.me/tag/ios/)
[iPhone](https://springwood.me/tag/iphone/)
[js](https://springwood.me/tag/js/)
[line](https://springwood.me/tag/line/)
[linux](https://springwood.me/tag/linux/)
[M1](https://springwood.me/tag/m1/)
[mac](https://springwood.me/tag/mac/)
[macOS](https://springwood.me/tag/macos/)
[My Number Card](https://springwood.me/tag/my-number-card/)
[office](https://springwood.me/tag/office/)
[python](https://springwood.me/tag/python/)
[QQ](https://springwood.me/tag/qq/)
[SCI](https://springwood.me/tag/sci/)
[VPS](https://springwood.me/tag/vps/)
[wordpress](https://springwood.me/tag/wordpress/)
[亚马逊](https://springwood.me/tag/amazon/)
[信用卡](https://springwood.me/tag/credit-card/)
[健康码](https://springwood.me/tag/health-code/)
[公积金](https://springwood.me/tag/public-loan/)
[动态清零](https://springwood.me/tag/zero-covid/)
[博客](https://springwood.me/tag/blog/)
[台湾](https://springwood.me/tag/taiwan/)
[回忆](https://springwood.me/tag/recall-the-past/)
[国行](https://springwood.me/tag/chinese-apple-device/)
[在留资格](https://springwood.me/tag/residence/)
[地图](https://springwood.me/tag/map/)
[字幕](https://springwood.me/tag/subtitles/)
[密码](https://springwood.me/tag/password/)
[封城](https://springwood.me/tag/lock-down/)
[广告](https://springwood.me/tag/ads/)
[微信](https://springwood.me/tag/wechat-cn/)
[数据库](https://springwood.me/tag/database/)
[新冠](https://springwood.me/tag/covid/)
[日剧](https://springwood.me/tag/jp-drama/)
[日本](https://springwood.me/tag/japan/)
[日本生活](https://springwood.me/tag/jp-life/)
[日语](https://springwood.me/tag/japanese-language/)
[核酸](https://springwood.me/tag/dna-test/)
[润](https://springwood.me/tag/run/)
[生物信息学](https://springwood.me/tag/bioinformatics/)
[电视](https://springwood.me/tag/tv/)
[翻墙](https://springwood.me/tag/over-gfw/)
[育儿](https://springwood.me/tag/baby/)
[银行卡](https://springwood.me/tag/bank-card/)
[飞机](https://springwood.me/tag/plane/)
[香港](https://springwood.me/tag/hong-kong/)

Copyright © 2010 - 2025 [springwood](https://m.cmx.im/%40springwood).
Theme Galaxis by [ScriptsTown](https://scriptstown.com/).

* [其它专辑](https://springwood.me/series/)
* [隐私政策](https://springwood.me/privacy/)

Back to Top

Table of Contents

×

* 在电脑上（macOS）进行 adb 远程操作
* 在手机上（iOS）进行 adb 远程操作

→
Index