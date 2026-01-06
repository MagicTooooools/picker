---
title: AI魔改OpenWrt系统第9周，代码已开源
url: https://mp.weixin.qq.com/s/Xguk8Q0PWAVHP3WEMbzG8w
source: Doonsec's feed
date: 2026-01-05
fetch_date: 2026-01-06T03:29:17.513563
---

# AI魔改OpenWrt系统第9周，代码已开源

![cover_image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SiawfUibNPCffIXyYaRxVbQCwPKfylUpwB06fkpIMDQV2973sFk6SIuAA/0?wx_fmt=jpeg)

# AI魔改OpenWrt系统第9周，代码已开源

原创

TT

OpenWrt

![]()

在小说阅读器中沉浸阅读

经过两个多月的开发，AI魔改OpenWrt系统的代码终于达到开源标准，目前我已经将所有代码上传到github，可以自行下载代码编译。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9S1mXxprHqbDhd22ggF1VmBBbZyMqbIOhvPdmCuLxbOcSYmO5C4oicAtQ/640?wx_fmt=png&from=appmsg)

该项目是利用AI改造我的开源项目OpenAppFilter，然后对主题进行一些优化，让OpenWrt看起来更像一个商用的路由器系统，比如有完整的终端管理，可以查看终端的详细记录，包括应用记录、流量、上网时长等，还有上网行为管理功能。

目前系统已经内测了一段时间，大的问题没有，存在一些小问题不影响使用。项目默认分支基于OpenWrt24.10.4稳定版，虽然OpenWrt 24.10.5已经发布，但是为了兼容一些国内的插件，还是先用24.10.4，过两个月再更新到最新。

接下来我将会编译公测版本固件，会支持热门的OpenWrt设备，有些粉丝可能还不会刷OpenWrt系统，后面我也会在网站上提供一些教程。

下面来看看系统截图：

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SzsZWQIBAKriaX4Y89MYQHvggtHTz9kvecQKa7FeqAUsM19db6ic9b0gw/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SicGlpticKfcMOcLF3hhDhc2jZsfZAwKeyJAQrshyHy0U5DxaRYZf9Avw/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SIlX4clGb2cqFLo3qcct2Ajq0FE8waPsdPibrsJMKkVfaKAwcJGDMXGw/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SAjia14rsjQ8YzPW2661OnAq60erUc42iaIcATOH1S27WjMUNNX4xDLVA/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SfKiagLhDNZYibKkREXMQpkygVssK6iadvJPtrcCJEGOkreHXqFLW75WYQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9Sj9mOpAHJhnehiadlbJ7WX5V0y1qY3ahzdUFXFlicClk7FUSDNmiadCwAQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SeR1icmjMp8TO9hxUoJKrSgxVp5B2tpicGaAAZ1nic5vgUqnqwcDlwxxLw/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SOsS4kcQmNbiaPkTLYmQ0bON9Y0f6qMPvFo3DxOBonz9OkE3mibwkM8Ig/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9Sia1xLYOnILBwe1rticQMGaBUx4iaSY03Xb9LX97Uw3UichnNNjoolYdSKQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9S0tU798tB55kIRYRCbYKWFxvFVIhU6sy1q5Qn8vVSkjNPoKqBgfgn5g/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SAkjqPGp4dSfYTFk81XH3FYnBUIJckeHu6ZqAyibiccTa3qf0UVJN7KMQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SJ8Iot8EWUl8Zy6BtbaUp4UhjibLg8I1M09aeSDtwAI90PMB4vaHsLAg/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SMuAtg4yoF47tHExPAxNlVZGqqmFfPDRH53SzEoJbqgws7t4eKX3CuQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9SOhCcKctozUapMLAou3DRbeKlHicZ9nPNyncK60pePyJwAFEplxAXt7A/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9S7jZQia09LSxEVPse78czXDUUJL4WmmiaeHo7wXibXGNbkdtqwo5m2cK9g/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9Syz3tllha0a29F60x9fP9bOYYS9yddlSraiasNNXibHl5TeUDiafAKB4bQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9Sa6SNOxnbKSCAgRj63VEzfwv0vYKdtibzqN45NQPFRFRcV4Zr3X7U0SA/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4dGgALU2VXy5Jvpn4WVibTc6ibn2ibUUy9Sx3a4EX3M9WFQJ9q2HO2ky1Grric7Eq3MtDp1J3fZCN7crbJHgDk4J2A/640?wx_fmt=png&from=appmsg)

开源地址

二次开发的系统代号为FanchmWrt，定位为一款家用的防火墙系统，简称FCM。

开源地址为:

https://github.com/fanchmwrt/fanchmwrt

在github直接搜fanchmwrt即可

编译方法和标准的OpenWrt一样，支持所有架构的OpenWrt设备，大家记得在github中给个star，提高项目的热度。

---

历史文章：

[支持刷机的路由器(2025)](https://mp.weixin.qq.com/s?__biz=MzU4MTgxNDc2MQ==&mid=2247486582&idx=1&sn=9602044c4ef4b94f28b9473766958efa&scene=21#wechat_redirect)

[AI魔改OpenWrt系统（第8周）](https://mp.weixin.qq.com/s?__biz=MzU4MTgxNDc2MQ==&mid=2247486575&idx=1&sn=2105974980d18290ae0d7cdca3ce4a5c&scene=21#wechat_redirect)

[OpenWrt支持手机App管理了](https://mp.weixin.qq.com/s?__biz=MzU4MTgxNDc2MQ==&mid=2247486448&idx=1&sn=97a3276f3779ed7c82870a78c898d412&scene=21#wechat_redirect)

[H3C NX30 Pro路由器刷机教程（拆机版）](https://mp.weixin.qq.com/s?__biz=MzU4MTgxNDc2MQ==&mid=2247486399&idx=1&sn=cc14c655ad52a395b35f79dca50f022c&scene=21#wechat_redirect)

![图片](https://mmbiz.qpic.cn/mmbiz_png/4dGgALU2VXwxOVx85bAbC3CbFUBYEHduhydpeBQNuGCgk5xPlneKvuO7LSjFTYdEMoSffnUichcTV8Z2xFF25Qg/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&randomid=dm2rfhjh&tp=webp#imgIndex=15)

欢迎关注公众号

定期分享OpenWrt干货

OpenWrt OAF插件作者(2.5k star)

一款可以屏蔽游戏、视频的路由器插件

预览时标签不可点

![]()

微信扫一扫
关注该公众号

继续滑动看下一个

轻触阅读原文

![](http://mmbiz.qpic.cn/mmbiz_png/4dGgALU2VXwGPhSnjG6IhzI0wCrUicApDmpsL1c5VyoWFph6dicu8RydO8StibF1ibHIF7zOeAUrz31GPo9UGqNOTw/0?wx_fmt=png)

OpenWrt

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

![作者头像](http://mmbiz.qpic.cn/mmbiz_png/4dGgALU2VXwGPhSnjG6IhzI0wCrUicApDmpsL1c5VyoWFph6dicu8RydO8StibF1ibHIF7zOeAUrz31GPo9UGqNOTw/0?wx_fmt=png)

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