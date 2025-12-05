---
title: 20251204的胡言乱语
url: https://www.bboy.app/2025/12/04/20251204%E7%9A%84%E8%83%A1%E8%A8%80%E4%B9%B1%E8%AF%AD/
source: Bboysoul's Blog
date: 2025-12-04
fetch_date: 2025-12-05T03:21:06.575114
---

# 20251204的胡言乱语

[![](/oss/head.webp)](/)

# [Bboysoul's Blog](/)

|  |  |  |  |
| --- | --- | --- | --- |
| [首页](/) | [公告](/gonggao.html) | [项目](/projects.html) | [RSS](https://www.bboy.app/atom.xml) |

---

⬇️⬇️⬇️ 欢迎关注我的 telegram 频道和 twitter ⬇️⬇️⬇️

---

联系方式:
[Twitter](https://twitter.com/bboysoulcn)
[Github](https://github.com/bboysoulcn)
[Email](/cdn-cgi/l/email-protection#7517171a0c061a00193517171a0c5b140505)
[Telegram](https://t.me/bboyapp)

---

# 20251204的胡言乱语

December 4, 2025
本文有 1113 个字
需要花费 3 分钟阅读

![](/oss/20251204-2.webp)

### 简介

欢迎关注我的频道，不时发送垃圾消息

<https://t.me/bboyapp>

或者关注我的 twitter

<https://twitter.com/bboysoulcn>

1. ### The $1,000 AWS mistake

一位工程师分享了从 Hetzner 向 AWS S3 同步大规模地理数据时遭遇的意外天价账单：因未配置 VPC Gateway Endpoint，导致流量通过 NAT Gateway 走了弯路，单日产生 20TB 数据传输费用超过 1000 美元，最终通过免费的 Gateway Endpoint 解决了问题。

<https://www.geocod.io/code-and-coordinates/2025-11-18-the-1000-aws-mistake/>

2. ### ‘The French people want to save us’: help pours in for glassmaker Duralex

法国玻璃制造商 Duralex 发起 500 万欧元众筹，仅用 5 小时 40 分钟就完成目标，最终认购金额达近 2000 万欧元，这家以经典 Picardie 杯闻名的百年品牌因唤起法国人的集体记忆与爱国情怀而获得巨大支持，目前正努力实现 2027 年盈亏平衡。

<https://www.theguardian.com/world/2025/nov/22/french-people-want-to-save-us-help-pours-glassmaker-duralex>

3. ### Kubernetes Configuration Good Practices

Kubernetes 官方博客总结的配置最佳实践：使用最新稳定 API、将配置存入版本控制、优先使用 YAML、保持配置简洁、合理使用 Deployment 和 Service、善用标签选择器以及避免裸 Pod 等核心原则，帮助开发者构建更清晰、可维护的 Kubernetes 集群配置。

<https://kubernetes.io/blog/2025/11/25/configuration-good-practices/>

4. ### World’s Most Stable Raspberry Pi? 81% Better NTP with Thermal Management

通过 CPU 核心隔离和 PID 温控稳定技术，作者将树莓派 GPS PPS NTP 服务器的频率变化降低了 81%，RMS 偏移减少 49%，达到 35 纳秒级别的时间精度，证明通过主动控制 CPU 温度可以稳定晶振频率从而实现极致的时间同步性能。

<https://austinsnerdythings.com/2025/11/24/worlds-most-stable-raspberry-pi-81-better-ntp-with-thermal-management/>

5. ### Load Testing: how many HTTP requests/second can a Single Machine handle?

通过实际负载测试证明：1 核 2GB 机器可稳定处理 200-300 RPS，2 核 4GB 机器可处理 500-1000 RPS，4 核 8GB 机器可稳定处理 2000-3000 RPS，峰值短时可达 4000 RPS，说明单机配合单个数据库足以满足绝大多数应用需求，无需过早引入复杂的微服务架构。

<https://binaryigor.com/how-many-http-requests-can-a-single-machine-handle.html>

6. ### Datacenters in space are a terrible, horrible, no good idea

前 NASA 工程师从电力、散热、辐射防护和通信四个维度详细论证了太空数据中心的不可行性：ISS 级太阳能阵列仅能支持 200 个 GPU，散热需要巨大的辐射板，辐射耐受需要大幅降低性能，网络带宽远低于地面，总体成本和性能都远逊于地面数据中心。

<https://taranis.ie/datacenters-in-space-are-a-terrible-horrible-no-good-idea/>

7. ### How WebSockets Work

详细解释了 WebSocket 的工作原理：从 HTTP/1.1 升级握手开始，通过持久连接实现服务端与客户端的双向实时通信，适用于在线聊天、多人游戏和实时日志等场景，并讨论了其状态化难以横向扩展的缺点以及使用二进制数据优化传输效率的技巧。

<https://www.deepintodev.com/blog/how-websockets-work>

8. ### HAProxy Ingress NGINX Migration Assistant

HAProxy 提供的从 Ingress NGINX 迁移到 HAProxy Kubernetes Ingress Controller 的完整工具包，包括注解映射对照表、Helm 安装指南和配置示例，帮助用户快速完成从即将退役的 Ingress NGINX 到高性能 HAProxy 的平滑过渡。

<https://www.haproxy.com/ingress-nginx-migration-assistant>

9. ### Xiaomi Miloco

小米开源的本地智能家居 Copilot 框架，基于端侧 LLM 和摄像头视觉数据，通过自然语言交互实现智能设备控制、场景事件理解和规则设定，支持小米智能家居生态设备接入，保护家庭隐私的同时提供更自由创新的智能家居整合方案。

<https://github.com/XiaoMi/xiaomi-miloco>

10. ### 2025 Computer Stocktake

博主盘点了家中所有的电脑设备：从运行 Plex 和 Usenet 的 Dell OptiPlex、游戏 PC、多台 ThinkPad 笔记本、M3/M4 MacBook Pro、ROG Ally 掌机到树莓派和待修复的 Macintosh SE/30，详细记录每台设备的配置和用途，展现了一个技术爱好者的设备收藏全景。

<https://blog.decryption.net.au/posts/mycomputers.html>

欢迎关注我的博客[www.bboy.app](https://www.bboy.app/)

Have Fun

---

Tags:

* [胡言乱语](https://www.bboy.app/tags/%E8%83%A1%E8%A8%80%E4%B9%B1%E8%AF%AD/);

* + [简介](#简介)
  + [The $1,000 AWS mistake](#the-1000-aws-mistake)
  + [‘The French people want to save us’: help pours in for glassmaker Duralex](#the-french-people-want-to-save-us-help-pours-in-for-glassmaker-duralex)
  + [Kubernetes Configuration Good Practices](#kubernetes-configuration-good-practices)
  + [World’s Most Stable Raspberry Pi? 81% Better NTP with Thermal Management](#worlds-most-stable-raspberry-pi-81-better-ntp-with-thermal-management)
  + [Load Testing: how many HTTP requests/second can a Single Machine handle?](#load-testing-how-many-http-requestssecond-can-a-single-machine-handle)
  + [Datacenters in space are a terrible, horrible, no good idea](#datacenters-in-space-are-a-terrible-horrible-no-good-idea)
  + [How WebSockets Work](#how-websockets-work)
  + [HAProxy Ingress NGINX Migration Assistant](#haproxy-ingress-nginx-migration-assistant)
  + [Xiaomi Miloco](#xiaomi-miloco)
  + [2025 Computer Stocktake](#2025-computer-stocktake)

---

|  |  |
| --- | --- |
| 本站总访问量  次 | 本站总访客数  人 |

2016 - 2025
Bboysoul