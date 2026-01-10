---
title: 在 Linux Router 上配置 Source-Based Routing 策略（源进源出）
url: https://blog.186526.xyz/post/bgp-router-with-sbr/?utm_source=atom_feed
source: blog@real186526
date: 2026-01-09
fetch_date: 2026-01-10T03:35:40.930677
---

# 在 Linux Router 上配置 Source-Based Routing 策略（源进源出）

[![Avatar](https://186526.xyz/avatar.png)](/)😺

# [blog@real186526](/)

## 空中散步与四拍子。

1. [主页](/)
2. [归档](/archives/)
3. [搜索](/search/)
4. [RSS](/atom.xml)

- 暗色模式

## 目录

1. [Netfilter 和 nftables](#netfilter-和-nftables)
2. [iproute2、路由分表和策略路由](#iproute2路由分表和策略路由)

[Technology](/categories/technology/)

## [在 Linux Router 上配置 Source-Based Routing 策略（源进源出）](/post/bgp-router-with-sbr/)

Jan 10, 2026

阅读时长: 4 分钟

近期从朋友手里嫖了一点带宽，但是上游的网络配置明显禁止了非对称路由，所以按照我们 AS47778 的骨干设计来讲，内网的路由不对称，会导致有些并非来自该上游的来向包被选路到该上游出口。

所以，我们接下来基于 nftables 和 iproute2 来配置了 Source-Based Routing，来保证仅有来自该上游的包可以选择该上游进行转发。

## Netfilter 和 nftables

Netfilter 是 Linux 内核中的一个用于管理数据包的框架。一切数据包都从 Netfilter 经过，经历如下图所示的周期。在 prerouting、input、output、forward 和 postrouting 阶段，Netfilter 允许 iptables 和 nftables 这样的应用程序对数据包进行控制，这也正是防火墙的基础。

![Netfilter-packet-flow](/post/bgp-router-with-sbr/Netfilter-packet-flow.png)

看起来很麻烦吧，所以我们来简化一下。

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` (NETIN) -> PREROUTING -> <ROUTING> -> FOWARD -> POSTROUTING -> (NETOUT)                              |                       ↑                              |                   <ROUTING>                              ↓                       ↑                            INPUT   -> [LOCAL]  ->  OUTPUT ``` |

对于到本地的包 A，从 NETIN 进入之后，会通过 `PREROUTING` 链，到达 ROUTING 进行路由决策，检查到是本机包后进入 `INPUT` 链，然后被实际的发送给本地的程序。
本地的程序处理后，发送向外部的包 B，从 LOCAL 进入后到达 `OUTPUT` 链，再次到达 ROUTING 进行路由决策，通过 `POSTROUTING` 链后最终从 NETOUT 离开本机。

对于需要转发的包 C，从 NETIN 进入之后，通过 `PREROUINTG` 链，到达 ROUTING 进行路由决策，检查到需要转发，进入 `FORWARD` 链，通过 `POSTROUTING` 链后最终从 NETOUT 离开本机。

对于我们目前的情况而言，我们来分析以下不同包的情况：

1. 去包从该上游进入网络内部，在本节点需要转发到网内的去包：正常转发。
2. 去包从该上游进入网络内部，在本节点需要响应回包：回包正常按照选路离开节点或收到回包后源进源出即可。
3. 去包从其他节点进入网络内部，在本节点需要继续转发在网内的去包：正常按照选路离开节点即可。
4. 去包从其他节点进入网络内部，在本节点需要响应回包：回包不可以从该上游离开网络，需要源进源出。

在 Linux 的网络栈中，我们可以通过向包添加 fwmark (通常是一个 32bit 的无符号证书) 的方式来为数据包添加 tag，通过这个 tag 来在路由决策里将路由导向不同的 NETOUT。

同时也存在着一个 conntrack 机制和对应的 conntrack 表，用来追踪整个连接，通常被用于 NAT 环境中方便维护转发关系，我们在这里也会使用用来解决 2 的回包进行源进源出问题。

对于我们的场景而言，我们主要需要以上的 2 和 3 两个情况进行源进源出策略。

|  |  |
| --- | --- |
| ```  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 ``` | ``` #!/usr/sbin/nft -f  table inet source_routing {     chain prerouting {         type filter hook prerouting priority mangle; policy accept;          iifname "up1" ct mark set 0x00000001         iifname "node1" ct mark set 0x00000002         iifname "node2" ct mark set 0x00000003          ct state {established, related} meta mark set ct mark     }      chain output {         type route hook output priority mangle; policy accept;         meta mark set ct mark     } } ``` |

我们在这里创建一个名为 source\_routing 的表，同时为 IPv4 和 IPv6 应用 (inet)。

在 `PREROUTING` 链上，为来自不同接口的 packet 的连接打上了 ct mark，用来追踪连接。
对于已经被 ct mark 匹配并追踪的既有连接的包，我们直接将 ct mark 作为 fwmark 的值复制。
通过`PREROUTING` 链和 conntrack，我们给符合 2 条件的包打上了 fwmark。

在 `OUTPUT` 链上，是本地程序响应的包，符合 4 条件。我们直接 copy ct mark 到 fwmark。

## iproute2、路由分表和策略路由

在 Linux 中，最多可以存在 255 (1-255) 个路由表，系统默认为我们配置 255 (local) / 254 (main) / 253 (default) 三个路由表。

local 表中包含本机路由和广播信息，127.0.0.1/8 的路由就来自该路由表，该表的内容由内核主动维护，理论来讲不需要额外的修改。

main 表中包含默认配置的路由信息，是默认使用工具添加的表格，正常的 `ip route add` 就会将路由添加到该表。

default 表默认内容为空，可以手动指明向该表写路由。

对于我们需求而言，我们需要创造 N 个表（N 即该节点的外联节点+上游个数），并给对应的表添加路由来。

在 Linux with iproute2，我们需要在 `/etc/iproute2/rt_tables` 维护表格配置。

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 ``` | ``` 255     local 254     main 253     default 0       unspec  # Following is what we modified. 100 up1_table 101 node1_table 102 node2_table ``` |

我们在这里添加了 `100` `101` `102` 三个表，分别 mapping 到 `up1_table` `node1_table` `node2_table` 上。

对于我们的要求，可以直接添加 `default via $gateway dev $device` 到各个路由上。

|  |  |
| --- | --- |
| ```  1  2  3  4  5  6  7  8  9 10 11 12 13 ``` | ``` #!/bin/bash # policy.sh export src="fe80::2" # 通过该变量来保证不同 interface 上出包 src 的 IP 一致。  while IFS='|' read -r fwmark device table gateway; do     fwmark=$(echo "$fwmark" | tr -d '[:space:]')     device=$(echo "$device" | tr -d '[:space:]')     table=$(echo "$table" | tr -d '[:space:]')     gateway=$(echo "$gateway" | tr -d '[:space:]')      sudo ip route add default via "$gateway" dev "$device" src "$src" table "$table"     sudo ip rule add fwmark "$fwmark" lookup "$table" done ``` |

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` # policy.conf # fwmark|device|table|gateway 0x1|up1|up1_table|fe80::1 0x2|node1|node1_table|fe80::1 0x3|node2|node2_table|fe80::1 ``` |

|  |  |
| --- | --- |
| ``` 1 ``` | ``` cat policy.conf | sudo policy.sh ``` |

通过这个小脚本，我们分别给每个表都按照配置加入了对应的到 gateway 的默认路由，然后配置了包含 fwmark 的包都按照对应的 fwmark 命中到表上。

此时对于来自该上游并需要在本地响应的回包：

1. 入包命中 `PREROUTING` 链，并被打上 ct mark
2. 回包进入 `OUTPUT` 链，将 ct mark 复制到 fwmark 上
3. 回包进入 ROUTING 路由决策，匹配 fwmark 到对应的仅默认路由路由表上
4. 回包离开本机

以上，我们成功配置了源进源出的网络路由方案，顺带对 fwmark 和 nftables 等有了初步的了解。若要进一步了解 nftables 及其配置， 官方 wiki 是个不错的选择，无论是配置过程中遇到问题还是希望进行更复杂的配置，都能在[官方 wiki](https://wiki.nftables.org/wiki-nftables/index.php/Main_Page) 找到详尽的说明。总之，配网愉快~

[Linux](/tags/linux/)
[Network](/tags/network/)
[BGP](/tags/bgp/)
[SBR](/tags/sbr/)

使用 CC BY-NC-SA 4.0 协议授权

最后更新于 Jan 10, 2026 05:05 +0800

## 相关文章

[## 如何使用 debootstrap 构建一个最小化的Ubuntu](/post/debootstrap/)

Please enable JavaScript to view the [comments powered by Disqus.](https://disqus.com/?ref_noscript)

![Avatar](https://186526.xyz/avatar.png)😺

## 空中散步与四拍子。

©
2020 -
2026 [blog@real186526](https://blog.186526.xyz/)

使用 [Hugo](https://gohugo.io/) v0.125.6 with go1.22.2 构建
Theme **[Stack](https://github.com/CaiJimmy/hugo-theme-stack)** designed by [Jimmy](https://jimmycai.com)
[Forked](https://github.com/186526/hugo-theme-stack) with 💖 by [real186526](https://186526.xyz)

[![Moe ICP 20218600](https://unpkg.assets.real186526.cn/shields.io/badge/%E8%90%8CICP%E5%A4%87-20218600-blue)](https://icp.gov.moe/?keyword=20218600)
![Latest Update](https://unpkg.assets.real186526.cn/shields.io/date/1767992767?color=sucessful&label=latest%20update)