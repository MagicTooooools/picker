---
title: Burp小插件之SSRF
url: https://mp.weixin.qq.com/s/pbXdUrfKIZEBKEQXixgo1g
source: Doonsec's feed
date: 2026-01-05
fetch_date: 2026-01-06T03:29:29.881935
---

# Burp小插件之SSRF

![cover_image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokXvQqnA4X0M667t8Ud4uUEicuAqFGqicj9o6bqN6ibJI3hsUVAgKzdgbJQ/0?wx_fmt=jpeg)

# Burp小插件之SSRF

原创

pippybear

安全无界

![]()

在小说阅读器中沉浸阅读

声明：请勿利用文章内的相关技术从事非法测试，如因此产生的一切不良后果与文章作者和本公众号无关。

今天来分享一个小burp插件（SSRF探测），虽然已经有不少成熟的SSRF插件了，但是可能因为不是按照自己的想法捣鼓的，总觉得用起来不是很舒服，于是抽空人工+AI快速搞了一个小插件给自己使用。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokun2hSXiaCOL0MvmKhPbynwo8au9damzDMJot8z8icSLIyhJIW4qEQOuw/640?wx_fmt=png&from=appmsg)

其实核心也很简单，就是自动识别proxy中的request，如果带有url等参数，则发送SSRF探测，dns采用burp自带的，因此不用再额外配置。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokJJlzaV2CSrBQuriabOUXajsVXpfbVicazZ5AxQ9Us5wawyXv47ZUxibDQ/640?wx_fmt=png&from=appmsg)

像上面这样，打开Proxy检测就会自动对数据报中带有url等参数的request进行SSRF探测了，如下。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvoktQujhx2NH6ic8hWfyIwpZ895jRcCV1MVtgneYJeMmsmHYTBSXukHV2w/640?wx_fmt=png&from=appmsg)

支持关掉，虽然感觉这功能有点鸡肋，但是防止有时候需要嘛，主打的万一需要呢，哈哈。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokeSLz6jianU1nsqicyOBonRx94fjbvNalrrMzKH4I3tHhrBPw39jYlFYA/640?wx_fmt=png&from=appmsg)

另外这个和上面有点一个意思，万一想手动触发不是，这不就加了一个手动触发，主打的就是麻雀虽小，五脏俱全。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokypQ8BtA2LJGxueDmdu2fqEBDSbTFTr6SuKsibZZYTgQx32oxhMblWlA/640?wx_fmt=png&from=appmsg)

另外还稍稍支持了一下白名单，有时候测试的时候并不想测试某些host，这不就可以添加白名单，添加后自动扫描就会忽略掉相应host（UI丑是丑了点，但是能用不是，嘿嘿）。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvok2iciazenLD5qu4tc90Sz0hrjUfbaLm9JKEW4lTgHNN35oLkb2vlO2icicw/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokLaxCtiaZ4X3zT6ibLy1YnLB5iaRiaL9PzdQBBibRqA2zanib6EIbV2gJicRgw/640?wx_fmt=png&from=appmsg)

当然可以添加，那必然得支持可以remove，哈哈。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvok1dMWNqo8iaeqWBlI2clx3R3odb630kpLvgsX8K9fibibJxdvdibhqqibGEg/640?wx_fmt=png&from=appmsg)

最后就是控制台log输出，这个主要就是部分操作会log记录一下，不重要，主要还是我写的时候debug用，哈哈。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokrtDYe8b6DVqldss4kxeXWMPnB1l0X5HJ8xEMicgic5I0TYZryo3J0y8Q/640?wx_fmt=png&from=appmsg)

emmm，文章写着写着，发现颜色似乎有点单调，于是乎又小改了一下，大概效果如下，这下是不是舒服多了。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvok7fvQm3DWS0KeYlqqk8icSc8KYriagic9MrF7ZXdA3dVrfMwIpS7aCrMsg/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gTvrxYa45OmQlXJtSAKPvokOoCECvWlibdhHIjqh0q5t574G5oI2k5vO8g61ibibcv9jusJsDdQKe1Rg/640?wx_fmt=png&from=appmsg)

满足自我需求的小脚本哈，有需求的大佬，可以公众号回复“SSRF”获取哈。

预览时标签不可点

![]()

微信扫一扫
关注该公众号

继续滑动看下一个

轻触阅读原文

![](http://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gRWFX6SiaQE368qzz3ruUmbnpAzmoIcmWYrXOnic1DzRllicgLMZ3NZ49q4CG4Tq7mmIkL8oyibfLfMqw/0?wx_fmt=png)

安全无界

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

![作者头像](http://mmbiz.qpic.cn/sz_mmbiz_png/ib9b5DLqe7gRWFX6SiaQE368qzz3ruUmbnpAzmoIcmWYrXOnic1DzRllicgLMZ3NZ49q4CG4Tq7mmIkL8oyibfLfMqw/0?wx_fmt=png)

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