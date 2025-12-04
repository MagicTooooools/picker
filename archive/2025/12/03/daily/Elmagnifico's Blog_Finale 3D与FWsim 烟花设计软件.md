---
title: Finale 3D与FWsim 烟花设计软件
url: https://elmagnifico.tech/2025/12/04/Finale3d-FWsim/
source: Elmagnifico's Blog
date: 2025-12-03
fetch_date: 2025-12-04T03:23:04.600536
---

# Finale 3D与FWsim 烟花设计软件

Toggle navigation

[Elmagnifico's Blog](/)

* [Home](/)
* [Tags](/tags/)
* [About](/about/)

[Fireworks](/tags/#Fireworks "Fireworks")
[SFX](/tags/#SFX "SFX")

# Finale 3D与FWsim 烟花设计软件

## 灯光设计、舞美设计、烟花模拟

Views:  times
Updated on December 4, 2025
Posted by elmagnifico on December 4, 2025

## Foreword

体验一下目前烟花领域比较主流的两款设计软件：Finale 3D 和 FWsim。它们都可以算是偏烟花方向的 SFX 设计工具。之前折腾过一些偏灯光的舞美软件，里面多少带了一点烟花模块，但基本都是“顺带做做”，专业度一般。

这次就专门来看看，真正以烟花为核心的专业软件，发展到了什么程度。

## Finale 3D

![image-20251203162643547](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203162643753.png)

> https://finale3d.com/

3D 模拟烟花效果，所见即所得，大致能力包括：

* 支持 3D 模型导入
* 支持地图导入
* 支持音频时间码，同步时间
* 支持自定义烟花效果模型
* 内置烟花库

![image-20251203163821923](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203163821960.png)

看起来整体价格也还算友好，不是那种完全摸不到的级别。

![image-20251203162511847](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203162511886.png)

注册激活以后虽然不能保存和导出成正式工程，但对于单纯体验、研究格式、看工作流，已经够用了。

![image-20251203162719546](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203162719570.png)

导入格式支持的是 CSV，只要有一个别人做好的工程文件在手，这种明文格式就很容易反向解析、适配到自己的流程里。

烟花的时间轴表现还是比较细的，比如升空时间、爆炸点、爆炸后持续点亮的时间段，都很清晰地标在时间轴上，方便精细编排。

![image-20251203163409408](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203163409436.png)

Pro 版本还支持无人机灯光秀的显示。

![image-20251203163940342](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203163940366.png)

VVIZ 是 Verge 无人机的脚本/路径格式。

![image-20251203173125895](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203173125927.png)

实际设置的每个烟花，都有点类似关键帧的概念，在编辑器里一条一条列出来，可以批量修改，也可以单独点开查看。

由于烟花释放的位置基本是固定的，对应的点火端口也基本固定，所以通常是先在软件里编好脚本，导出之后燃放位置、分线盒、端口都能一一对应，下面的烟花控制台就可以直接识别读取。

相对而言，这套模拟更偏“烟花本体”，目前没法同时把舞台灯光的效果也一起模拟进去，对移动烟花的支持也比较有限。虽然理论上可以把移动烟花的释放端口当成关键帧导进去，但实际用下来还是有点别扭。

国内以前也有自己的烟花设计软件，不过基本都偏“编排工具”，没有成熟的三维可视化效果，只能靠想象补完画面。现在行业里更多是直接用 Finale 3D，然而中文资料、教程什么的确实不算多。

## FWsim

> https://www.FWsim.com/

![image-20251203174042365](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203174042403.png)

FWsim 的价格就更亲民一些，一些单纯想设计着玩、或者偶尔做个小项目的玩家也能负担得起，买来练手没太大压力。基础功能的覆盖面相对而言也更广一点。

FWsim 的技术栈是基于 .NET。

![image-20251203174349593](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203174350200.png)

FWsim 的上手体验确实更好一些，安装完第一次打开，就会直接给你一个完整的示例工程跑一遍，能很快感受到它大概能做到什么程度。

![image-20251203174447114](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203174447155.png)

进度条上关于每个片段的说明也非常清楚，缩放比例、易用性都比预期要好。

模拟场景的操作方式也很“游戏化”，可以直接用 WASD 在场景里走动、调整视角，整体的反馈比较直观。

![image-20251203174636394](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203174636529.png)

烟花库也更直观，鼠标悬停就会有动态演示效果，挑素材的时候非常方便。

不过也不是没有槽点。

![image-20251203175152050](https://img.elmagnifico.tech/static/upload/elmagnifico/20251203175152196.png)

FWsim 里烟花的点位需要完全人工定义。每个点位定义完之后，可以在 3D 空间里直接拖拽，再通过属性栏精调坐标，这一点没问题。但它在 3D 视图中无法框选，只能一个一个点选，批量移动也不支持，这块的交互体验就明显不如 Finale 3D 了。

## Summary

整体体验下来，两款软件都算成熟可用，也能覆盖大部分“纯烟花”场景。不过一旦牵扯到和灯光、舞美等其他系统的联动，问题就来了。

3D 空间内的所有元素最好是能放到同一个场景里统一预览，这样才能真正看清整体效果。但目前行业内整合能力看起来还不太够，很难做到把灯光设计和烟花设计无缝合在一块。尤其是灯光比烟花复杂得多：持续时间更长、动态变化更密集、参与互动的元素也更多，这块要想做到真正的一体化协同，感觉还有不少路要走。

舞台灯光涉及到舞台施工，所以灯光设计软件往往都要出对应的施工图纸，并且在实际工作的时候，这部分内容也是分包给专业的人去做，而不是由舞台设计方完成，行业内做的比较细，但是烟花这个东西，本身就有点野路子，烟花设计、施工、实施往往都是一伙人完成，不存在分包的情况，行业细分还是差一些，目前还跟不上其他行业卷的程度。烟花行业内也是各自为政，各玩各的，很少有统一的共识或者行业内统一的设计方案等约束。

---

* [Previous
  懂车帝珠海赛道日与小米深圳总部参观](/2025/11/23/dongchedi-zic/ "懂车帝珠海赛道日与小米深圳总部参观")

---

##### CATALOG

---

##### [FEATURED TAGS](/tags/)

[RaspberryPi](/tags/#RaspberryPi "RaspberryPi")
[嵌入式](/tags/#嵌入式 "嵌入式")
[Git](/tags/#Git "Git")
[脚本](/tags/#脚本 "脚本")
[python](/tags/#python "python")
[LeetCode](/tags/#LeetCode "LeetCode")
[C++](/tags/#C++ "C++")
[APM](/tags/#APM "APM")
[FreeRTOS](/tags/#FreeRTOS "FreeRTOS")
[Markdown](/tags/#Markdown "Markdown")
[Embedded](/tags/#Embedded "Embedded")
[SD](/tags/#SD "SD")
[Linux](/tags/#Linux "Linux")
[Vim](/tags/#Vim "Vim")
[Ubuntu](/tags/#Ubuntu "Ubuntu")
[Tools](/tags/#Tools "Tools")
[STM32](/tags/#STM32 "STM32")
[Maya](/tags/#Maya "Maya")
[LPWAN](/tags/#LPWAN "LPWAN")
[Graph Theory](/tags/#Graph Theory "Graph Theory")
[Algorithm](/tags/#Algorithm "Algorithm")
[PathFind](/tags/#PathFind "PathFind")
[OMPL](/tags/#OMPL "OMPL")
[VPS](/tags/#VPS "VPS")
[QT](/tags/#QT "QT")
[Router](/tags/#Router "Router")
[JS](/tags/#JS "JS")
[Chrome](/tags/#Chrome "Chrome")
[Tampermonkey](/tags/#Tampermonkey "Tampermonkey")
[API](/tags/#API "API")
[Java](/tags/#Java "Java")
[Spring](/tags/#Spring "Spring")
[MySql](/tags/#MySql "MySql")
[Springboot](/tags/#Springboot "Springboot")
[Docker](/tags/#Docker "Docker")
[V2ray](/tags/#V2ray "V2ray")
[TTRSS](/tags/#TTRSS "TTRSS")
[Nintendo Switch](/tags/#Nintendo Switch "Nintendo Switch")
[Trace](/tags/#Trace "Trace")
[Crack](/tags/#Crack "Crack")
[BLHeli](/tags/#BLHeli "BLHeli")
[DSHOT](/tags/#DSHOT "DSHOT")
[ESC](/tags/#ESC "ESC")
[Music](/tags/#Music "Music")
[C#](/tags/#C# "C#")
[EasyCon](/tags/#EasyCon "EasyCon")
[Blog](/tags/#Blog "Blog")
[杂谈](/tags/#杂谈 "杂谈")
[Proxy](/tags/#Proxy "Proxy")
[UAV](/tags/#UAV "UAV")
[GuinnessWorldRecords](/tags/#GuinnessWorldRecords "GuinnessWorldRecords")
[NAS](/tags/#NAS "NAS")
[群晖](/tags/#群晖 "群晖")
[ZeroTier](/tags/#ZeroTier "ZeroTier")
[Typora](/tags/#Typora "Typora")
[Map](/tags/#Map "Map")
[旅游](/tags/#旅游 "旅游")
[Log](/tags/#Log "Log")
[JSON](/tags/#JSON "JSON")
[Cython](/tags/#Cython "Cython")
[Equip](/tags/#Equip "Equip")
[Goods](/tags/#Goods "Goods")
[Share](/tags/#Share "Share")
[DMX512](/tags/#DMX512 "DMX512")
[Blender](/tags/#Blender "Blender")
[Game](/tags/#Game "Game")
[AP](/tags/#AP "AP")
[Network](/tags/#Network "Network")
[CloudFlare](/tags/#CloudFlare "CloudFlare")
[DIY](/tags/#DIY "DIY")
[WIFI](/tags/#WIFI "WIFI")
[Camera](/tags/#Camera "Camera")
[Diablo](/tags/#Diablo "Diablo")
[Sensor](/tags/#Sensor "Sensor")
[SES](/tags/#SES "SES")
[QQ](/tags/#QQ "QQ")
[Bot](/tags/#Bot "Bot")
[Python](/tags/#Python "Python")
[Vmq](/tags/#Vmq "Vmq")
[Jenkins](/tags/#Jenkins "Jenkins")
[米家](/tags/#米家 "米家")
[ESP32](/tags/#ESP32 "ESP32")
[Software](/tags/#Software "Software")
[C](/tags/#C "C")
[MT793x](/tags/#MT793x "MT793x")
[NXP](/tags/#NXP "NXP")
[CH32](/tags/#CH32 "CH32")
[OpenWrt](/tags/#OpenWrt "OpenWrt")
[Onion](/tags/#Onion "Onion")
[Copilot](/tags/#Copilot "Copilot")
[Cursor](/tags/#Cursor "Cursor")
[Investment](/tags/#Investment "Investment")
[ChatGPT](/tags/#ChatGPT "ChatGPT")
[SFX](/tags/#SFX "SFX")
[Debug](/tags/#Debug "Debug")
[RouterOS](/tags/#RouterOS "RouterOS")
[Mikrotik](/tags/#Mikrotik "Mikrotik")
[GitLab](/tags/#GitLab "GitLab")
[Drone](/tags/#Drone "Drone")
[OpenAI](/tags/#OpenAI "OpenAI")
[VS Code](/tags/#VS Code "VS Code")
[管理](/tags/#管理 "管理")
[build](/tags/#build "build")
[Kconfig](/tags/#Kconfig "Kconfig")
[CMake](/tags/#CMake "CMake")
[Su7 Ultra](/tags/#Su7 Ultra "Su7 Ultra")
[Car](/tags/#Car "Car")
[AI](/tags/#AI "AI")
[MCP](/tags/#MCP "MCP")
[LLM](/tags/#LLM "LLM")

---

##### FRIENDS

* [智伤帝](https://blog.l0v0.com/)
* [小静的博客](http://smilejing.cn/)
* [晨曦的博客](https://blog.whuzfb.cn/)
* [hiRipple](https://hiripple.com/)
* [草东日记](https://www.gaicas.com/)
* [MARKSZのBlog](https://molunerfinn.com/)
* [等你成为我的朋友](http://等你成为我的朋友)
* [Wait for you](http://Wait for you)

本站总访问量次
访客数人

载入天数...载入时分秒...

Copyright © Elmagnifico's Blog 2025

Theme by [elmagnifico](https://github.com/elmagnificogi/elmagnificogi.github.io)