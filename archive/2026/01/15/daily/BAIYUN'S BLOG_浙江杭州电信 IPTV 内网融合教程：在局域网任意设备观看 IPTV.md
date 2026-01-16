---
title: 浙江杭州电信 IPTV 内网融合教程：在局域网任意设备观看 IPTV
url: https://baiyun.me/zhejiang-hangzhou-iptv
source: BAIYUN'S BLOG
date: 2026-01-15
fetch_date: 2026-01-16T03:35:00.852785
---

# 浙江杭州电信 IPTV 内网融合教程：在局域网任意设备观看 IPTV

[Archive](/archive)[About](/about)

# 浙江杭州电信 IPTV 内网融合教程：在局域网任意设备观看 IPTV

January 15, 2026

## 背景

传统的 IPTV 需要使用运营商提供的 IPTV Box 连接电视才能观看，这种方案有几个缺点：

* 客厅如果只有一个网口就得在上网和 IPTV 之间二选一
* 每次看 IPTV 必须切换视频信号到 IPTV Box，稍微麻烦一些
* 必须忍受 IPTV Box 的 UI/UX
* 客厅电视没有空闲的 HDMI 端口可用（已经连接了多个设备，例如游戏机、其他电视盒子）

如果你想解决这些问题。网络上通常有很多术语，令人眼花缭乱，例如 IPTV 组播转单播、IPTV 单线复用、光猫超级管理员密码之类的概念。

本文经过实际测试摸索，找到一条相对简单高效的方案，大概只需要10分钟时间即可配置完毕。

## 前置条件

* 联系运营商开通 IPTV 服务，一般如果你已经在用融合宽带套餐，每个月加 10 块钱就可以
* 光猫为桥接模式（路由器拨号）
* 光猫和路由器放在一起，可以从光猫接两根线到路由器
* 路由器支持多 WAN 口模式
* 光猫有单独标识的 ITV 端口
* 局域网内有可以安装 docker 镜像的设备（例如 NAS）或者你有其他办法可以在局域网内部署 <https://github.com/stackia/rtp2httpd>

## 前置测试

将光猫 ITV 口接一根网线连到电脑，在电脑网络设置里手动设置此连接的 IP（指定为 192.168.1.1xx）具体 IP 段一般和光猫背面展示的控制台地址一致。本文后面以此 IP 段示例。
网关指定为 192.168.1.1

然后使用 VLC 播放器播放组播地址，可以从以下项目找具体地址：
<https://github.com/FHZDCJ/Zhejiang_Telecom_IPTV/blob/main/Zhejiang_Multicast/Zhejiang_Telecom_IPTV.m3u>

格式：`rtp://233.xxx.xxx.xxx:5140`，如果可以正常播放组播源，则继续后面的步骤。

> 在执行此测试的时候建议关闭 Wi-Fi 和 surge 之类的工具。确保所有请求直接通过网线发给光猫的 ITV 口。

## 组播转单播

如果前面的测试通过，说明你当前的网络条件可以正常播放组播源信号。

接下来为了实现在局域网内任意设备观看 IPTV，需要将组播信号（通常是 rtp 协议）转为单播信号（http 协议）。

首先需要在局域网内正常部署 <https://github.com/stackia/rtp2httpd> 服务。
我是直接在 NAS 上通过 Docker Compose 部署的，配置如下：

```
services:
  rtp2httpd:
    image: ghcr.io/stackia/rtp2httpd:latest
    container_name: rtp2httpd
    network_mode: host
    command: --noconfig --verbose 2 --listen 5140 --maxclients 20
```

待你可以正常访问 <http://NAS-IP:5140/status> 之后再进行下一步。

将光猫 ITV 口连接到路由器的第二个 WAN 口（第一个 WAN 口用于正常上网），这一步的主要目的是实现在正常上网的这个 LAN(局域网) 里面通过 HTTP 单播源观看 IPTV。

我用的是 Unifi 路由器，直接在控制台新建一个 WAN 口，关联刚才连接 ITV 口的端口，同时勾选 IGMP proxy 开关（这个很关键），选择静态 ip。

此时在 VLC 里打开 rtp2httpd 格式的单播信号源地址：

```
http://192.168.10.20:5140/rtp/239.xxx.xxx.xxx:5140
```

这里 192.168.10.20 是我的 NAS 地址，里面用 docker 部署了rtp2httpd（host 网络模式，默认绑定在 5140 端口），[239.xxx.xxx.xxx](http://239.xxx.xxx.xxx) 是具体 IPTV 频道的组播地址。

正常情况下这里就可以正常观看了。

最后，为了方便切换所有可观看的频道，可以将这里的 m3u 文件进行修改:
<https://github.com/FHZDCJ/Zhejiang_Telecom_IPTV/blob/main/Zhejiang_Multicast/Zhejiang_Telecom_IPTV_ONLINE_LOGO.m3u>

替换里面的链接为 rtp2httpd 格式，以下是替换后的示例：

```
#EXTM3U
#EXTINF:-1 tvg-id="CCTV1" tvg-name="CCTV1" group-title="央视",CCTV1综合
http://192.168.10.20:5140/rtp/233.xxx.xxx.xxx:5140
#EXTINF:-1 tvg-id="CCTV2" tvg-name="CCTV2" group-title="央视",CCTV2财经
http://192.168.10.20:5140/rtp/233.xxx.xxx.xxx:5140
```

将修改后的 m3u 文件导入到 APTV 之类的软件即可在 Apple TV、Mac、iPhone 等局域网设备观看 IPTV 频道。

## 总结

通过本文方案，唯一的物理链路修改是光猫到路由器多了一根网线，客厅网口和 HDMI 线完全没有任何调整，实现了在我现有的 Apple TV 设备上观看 IPTV 节目。整体具备低延迟高稳定性。

踩坑需要耗费不少时间，如果此文对你有帮助，欢迎通过 </buyMeCoffee> 请我喝杯咖啡。

> 本文原载于：[baiyun.me](https://baiyun.me/)
>
> 原文链接：<https://baiyun.me/zhejiang-hangzhou-iptv>

如果你喜欢我的内容，请考虑请我喝杯咖啡☕吧，非常感谢🥰 。

If you like my contents, please support me via BuyMeCoffee, Thanks a lot.

[🍻 请作者喝杯咖啡](/buyMeCoffee)

[Back to Home](/)

© 2017-2026