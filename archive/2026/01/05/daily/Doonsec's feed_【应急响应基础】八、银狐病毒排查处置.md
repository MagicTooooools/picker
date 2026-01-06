---
title: 【应急响应基础】八、银狐病毒排查处置
url: https://mp.weixin.qq.com/s/TCDj9i0bPURDBVescnHyrg
source: Doonsec's feed
date: 2026-01-05
fetch_date: 2026-01-06T03:29:42.749812
---

# 【应急响应基础】八、银狐病毒排查处置

![cover_image](https://mmbiz.qpic.cn/mmbiz_jpg/a5FBLichkAGuoBBBCkpNschN7ibK60O22VM5qib4wS8FCNQguO5T3SZicgPh7wJAA6HN5sogRicp8k6Fp1ViaLxngsgw/0?wx_fmt=jpeg)

# 【应急响应基础】八、银狐病毒排查处置

原创

攻防灯塔安全智库

攻防灯塔安全智库

![]()

在小说阅读器中沉浸阅读

# 【应急响应基础】七、勒索病毒排查

免责声明

请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息或者工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，文章作者不承担任何法律及连带责任。

[ 简介 ]

病毒排查

**银狐病毒最早于2020年初以“游蛇”等别名出现，2022年底其核心远控木马源码泄露后，演变为多团伙甚至产业化的恶意家族。自2023年开始大规模传播，2024-2025年期间，银狐病毒借助AI技术，出现持续大规模变种攻击银狐病毒通过即时通讯工具（如微信、钉钉）发送伪装成“发票”、“财税”等文件的钓鱼链接，用户一旦点击就会感染病毒。此外，病毒还会利用压缩文件（如RAR、ZIP）进行传播，文件名与钓鱼信息高度一致，以逃避检测。 银狐病毒能够远程控制受害者的计算机，窃取敏感信息，甚至进行挖矿等恶意操作。攻击者通过监控受害者的日常操作，实施诈骗等行为，给受害者带来直接的经济损失。**

---

---

|  |
| --- |
| **本期导读**  ![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGveaJbfBg7dSibMADK4MRvECyiaSJMZ3C4bSfzvyaYHTmbjaVTw7cLL8NXXURHicgO7uqGaNcqV86RBA/640?wx_fmt=png&from=appmsg)  银狐病毒排查处置 |

银狐病毒排查处置

***混子Hacker***

一、银狐病毒排查处置

**1.1 排查分析**

**根使用systeminfo发现进程9044外联恶意IP 47.243.247.76**

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGsNHz3fe4pib7quFXMr03nosdtuLCPlZGZM9Eh9XaOGAGCFldQjthvn32fhIib4Le5rRcahzZI2JFbw/640?wx_fmt=png&from=appmsg)

微步查询情报显示为远控

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGsNHz3fe4pib7quFXMr03nosSkk8OIYHcQnNWzqQcAiaQXGOeh0u5QawwGiccPgBKy8cq3W9ql2ff0eg/640?wx_fmt=png&from=appmsg)

查询进程树信息发现其父进程是6000，并且还存在一个子进程8256

进程6000对应程序是资源管理器

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGsNHz3fe4pib7quFXMr03nosLU9KYNGcWvUu7e9JL6w8aw2GKISNacanLDQG4CmwzQLZQypTGicFTdw/640?wx_fmt=png&from=appmsg)

**通过分析查询发现进程9044是TheWorld.exe是国产浏览器“世界之窗”的主程序，常被木马利用作为**合法载体**进行 DLL 劫持或注入**

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGsNHz3fe4pib7quFXMr03nosKyvBQAtL6yAyicz17DJLiceqiaFoONJshC9nrgtib4Udjnw6Bahf9yzB1A/640?wx_fmt=png&from=appmsg)

**使用Au**t**oruns继续排查注册表、计划任务、服务等权限维持项，发现可疑的注册表项**

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGuoBBBCkpNschN7ibK60O22V8Hj1ELWxEIVGiaT0ztXQCoa0WbrlMpczuA1hckicibfBcmye0uXC9A2aw/640?wx_fmt=png&from=appmsg)

找到对应路径发现文件被加了权限无法查看，使用attrib -s -h去掉对应的权限

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGuoBBBCkpNschN7ibK60O22VS3Ik2p4gt8ERicMd1VD1aZW4tVK81vic5LtokuNwNJ1hgGFJkCL5V9rg/640?wx_fmt=png&from=appmsg)

查看下载目录发现可疑文件

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGt7dfhkFcOTWWKWATb51sv8gSZibJX5qwEJa2Oc618TI4PrMB6p7Qft6qWw9gC7qvqWzzufJ3NrQQg/640?wx_fmt=png&from=appmsg)

同时在浏览器下载历史中发现文件下载记录

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGt7dfhkFcOTWWKWATb51sv8LVeTNJqJSmlw4umkgiawocBsGDQxmKFzabHqO3YF4jNrqic8MxxG2Mlw/640?wx_fmt=png&from=appmsg)

获取MD5进行分析发现该文件为木马

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGt7dfhkFcOTWWKWATb51sv8CumldPbbNWrbJNB5BZwysC6GbdicfuDAINbghj2AvGQ02hemFYqucBA/640?wx_fmt=png&from=appmsg)

根据该文件的下载时间进行查询发现可疑目录

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGuoBBBCkpNschN7ibK60O22VsfiaYWVacUicVEaMIOhg69Gz3FrGjiawFz7RADZdnCy7GsemxNBxtMm2g/640?wx_fmt=png&from=appmsg)

目录下的文件同样被加了权限，使用attrib去掉权限后查看

发现可疑config.ini文件文件较大同时打开为乱码

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGuoBBBCkpNschN7ibK60O22VWXWoYTHiaMBn2TnRIK6pYYZC5wrhzjTQR6NWibSZa1xaicImNSibDewIkw/640?wx_fmt=png&from=appmsg)

获取MD5查询发现是shellcode

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGuoBBBCkpNschN7ibK60O22VuE2FibCgZIPFBicSLw5lxuup6zOKh1UDbVsTXjicOpticjnCjSoobwkJBw/640?wx_fmt=png&from=appmsg)

同时对chrome\_dlf.dll提取MD5时报毒

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGuoBBBCkpNschN7ibK60O22VBVOUIvpicqEon73szmR7icNyhYZBGJXyauZt0EEXT3iaFuFpiaGywCPpWQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGt7dfhkFcOTWWKWATb51sv8M9DFaymNfZYT04SO1RwYu9XQUz4U2KPwiaGzplaVq1aW58FRxOGcnHA/640?wx_fmt=png&from=appmsg)

**1.2 攻击溯源**

整理：

```
1、用户通过浏览器下载恶意文件并运行2、运行之后通过劫持TheWorld.exe的chrome_elf.dll文件3、加载config.ini中的shellcode并运行进行外联4、创建注册表启动项进行权限维持
```

**1.3 处置加固**

1、删除注册表启动项

2、删除恶意dll文件和config.ini文件

3、结束程序TheWorld.exe及其子进程（注意这里程序被加了权限无法通过任务管理器结束，可以使用工具进行结束进程）

---

|  |
| --- |
| 本专栏往期文章：  [【应急响应基础】一、系统查询-Windows基础排查](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247485737&idx=1&sn=1ed4691cc7b1a4702441eb5d7523fa92&scene=21#wechat_redirect) [【应急响应基础】一、系统查询-Windows基础排查2](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247485764&idx=1&sn=cabd2b8befadb102b88df54ebc7d7424&scene=21#wechat_redirect) [【应急响应基础】二、Windows病毒排查-手动排查](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247485790&idx=1&sn=02d8831c3a9a337bce67a8cf2658b519&scene=21#wechat_redirect) [【应急响应基础】三、主机排查-Linux基础排查](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247485841&idx=1&sn=67fe9edfb3212921d26e27ae17d50237&scene=21#wechat_redirect) [【应急响应基础】四、Linux病毒排查](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247486009&idx=1&sn=e6092c648055460b7b19086f3316fb7c&scene=21#wechat_redirect)  [【应急响应基础】五、入侵排查-Windows入侵排查](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247486316&idx=1&sn=b609e7ba03897627f15f9906e711f959&scene=21#wechat_redirect)  [【应急响应基础】六、应急工具使用](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247488106&idx=1&sn=de7b429419bf485c6d4a5193d36beb35&scene=21#wechat_redirect)  [【应急响应基础】七、勒索病毒排查](https://mp.weixin.qq.com/s?__biz=MzUxMTk4OTA1NQ==&mid=2247488829&idx=1&sn=67924b7c31edd58a2cbedfe5f24089c4&scene=21#wechat_redirect) |

<<<  END >>>

原创文章｜转载请附上原文出处链接

更多漏洞｜关注作者查看

作者｜混子Hacker

预览时标签不可点

![]()

微信扫一扫
关注该公众号

继续滑动看下一个

轻触阅读原文

![](http://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGsZ2rNcUW2BDuiagmxWXLZlTvWEXdL0L4ASZDBPbLYOY1LbAyJcEkJUIt4qlPRYTQWGBZPNBUolbNw/0?wx_fmt=png)

攻防灯塔安全智库

向上滑动看下一个

知道了

![]()
微信扫一扫
使用小程序

取消
允许

取消
允许

取消
允许

×
分析

![跳转二维码]()

![作者头像](http://mmbiz.qpic.cn/mmbiz_png/a5FBLichkAGsZ2rNcUW2BDuiagmxWXLZlTvWEXdL0L4ASZDBPbLYOY1LbAyJcEkJUIt4qlPRYTQWGBZPNBUolbNw/0?wx_fmt=png)

微信扫一扫可打开此内容，
使用完整服务

：
，
，
，
，
，
，
，
，
，
，
，
，
。

视频
小程序
赞
，轻点两下取消赞
在看
，轻点两下取消在看
分享
留言
收藏
听过