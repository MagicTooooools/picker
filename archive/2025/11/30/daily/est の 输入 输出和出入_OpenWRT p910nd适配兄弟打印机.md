---
title: OpenWRT p910nd适配兄弟打印机
url: https://blog.est.im/2025/stdout-17
source: est の 输入 输出和出入
date: 2025-11-30
fetch_date: 2025-12-01T03:39:28.959483
---

# OpenWRT p910nd适配兄弟打印机

# [est の 输入 输出和出入](/)

* [Home](/)
* [RSS](https://feeds.feedburner.com/initiative)
* [About](/about)
* [Category](/category)

## OpenWRT p910nd适配兄弟打印机

Posted 2025-11-30 | stdout

买了个兄弟牌(Brother)的打印机，热销基础款，听说他家的联网打印很烂，所以只有USB的。

又买了个打印盒子，OpenWRT的，本来以为是 CUPS，结果一看 p910nd 好家伙。看了下这玩意纯粹是 tcp 直连 `/dev/usb/lp0` 。

在 macOS 上配置陷入了迷茫，最后问ChatGPT给调通了居然。

首先还是得下载官方驱动，然后添加 IP打印机

* Address 那里输入OpenWRT的 IP:9100
* Protocol 这里注意，选 HP JetDirect。这里的意思为 AppSocket。
* 最下面的 Use 选择驱动

也就是说IPP, AirPrint，LPD这些协议都不行。

然后试了下也失败，看了下默认USB双向通信，最好改成单向。OpenWRT上：

```
# cat /etc/config/p910nd
config p910nd
  option device        /dev/usb/lp0
  option port          0
  option bidirectional 0
  option enabled       1
```

macOS上：

`lpadmin -p <IP打印机名字> -o usb-no-bidi-default=true`

应该就行了。

支持 Airprint 就下一步想办法了。安卓上的打印只能输入IP，不支持 9100端口，估计支持了也是白费力，还得 .ppd 之类的翻译一下。

## Comments