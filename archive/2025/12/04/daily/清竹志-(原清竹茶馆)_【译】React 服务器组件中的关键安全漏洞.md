---
title: 【译】React 服务器组件中的关键安全漏洞
url: https://blog.vadxq.com/article/security-in-react-server-components/
source: 清竹志-(原清竹茶馆)
date: 2025-12-04
fetch_date: 2025-12-05T03:21:05.039517
---

# 【译】React 服务器组件中的关键安全漏洞

[![清竹志-(原清竹茶馆)](https://qnimg.vadxq.com/blog/2017/logo.jpg)清竹志-(原清竹茶馆博客)](/)

[首页](/)[归档](/archives)[分类](/categories)[标签](/tags)[友情链接](/links)[关于](/about)

![【译】React 服务器组件中的关键安全漏洞](https://qnimg.vadxq.com/blog/2017/logo.jpg)

2025-12-04发表2025-12-04更新[前端开发](/categories/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/) / [前端工程化](/categories/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%8C%96/)9 分钟读完 (大约1338个字)0次访问

# 【译】React 服务器组件中的关键安全漏洞

**重要提醒**：React 服务器组件曝光了一枚未认证的远程代码执行（RCE）漏洞，只要项目启用了 RSC 支持，就可能被远程掌控。建议所有依赖 React 19 的项目立即排查并升级。

> 本文根据 React 团队于 2025 年 12 月 3 日发布的「[Critical Security Vulnerability in React Server Components](https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components)」翻译整理。

---

## 🚨 漏洞概述

* **12 月 3 日**，React 官方团队发布最高级别安全通告：React 服务器组件（RSC）存在**未认证的远程代码执行**漏洞。
* 漏洞已被登记为 [CVE-2025-55182](https://www.cve.org/CVERecord?id=CVE-2025-55182)，CVSS 得分 **10.0**（危害最高级别）。
* 攻击者无需通过身份验证，只要能向 Server Function 端点发送恶意 HTTP 请求，就可能在服务器上执行任意代码，直接接管你的后端环境。
* 即便你尚未实现任何 React Server Function，只要你的框架或构建工具启用了 RSC 能力，就可能处于风险之中。

## ⚠️ 你是否受到影响？

### 受影响的 React 版本

* 19.0
* 19.1.0
* 19.1.1
* 19.2.0

### 受影响的框架与工具

* Next.js
* React Router
* Waku
* @parcel/rsc
* @vitejs/plugin-rsc
* rwsdk

### 以下情况暂不受影响

✅ 完全运行在客户端、没有任何服务端代码的 React 应用
✅ 没有使用支持 React 服务器组件的框架或打包工具

> **特别注意**：只要支持 React 服务器组件，即便没有配置任何 Server Function 端点，也可能遭到攻击！

## 🛡️ 紧急修复方案

* React 团队已在 `19.0.1`、`19.1.2`、`19.2.1` 中修复，请立即升级到对应分支的安全版本。
* 若你使用的是 canary 版本（如 Next.js 14.3.0-canary.77+），请降级到最新稳定版，再等待后续补丁。
* 一些托管服务商已在 React 团队指导下部署临时缓解，但**请勿依赖临时方案**，必须升级依赖。

**基础升级命令示例**：

|  |  |
| --- | --- |
| ``` 1 2 3 ``` | ``` npm install react@19.2.1 react-dom@19.2.1 # 或 yarn add react@19.2.1 react-dom@19.2.1 ``` |

## 📅 漏洞时间线

* **11 月 29 日**：安全研究员 Lachlan Davidson 通过 Meta Bug Bounty 报告漏洞。
* **11 月 30 日**：Meta 安全团队确认漏洞，并与 React 团队展开修复。
* **12 月 1 日**：修复方案完成，同时与托管服务商和生态项目联动部署缓解措施。
* **12 月 3 日**：补丁发布到 npm，漏洞以 CVE-2025-55182 正式披露。

## 💡 技术背景

React Server Functions 允许客户端通过 HTTP 请求调用运行在服务器上的函数，React 会负责序列化与反序列化过程。漏洞恰恰出在服务端解码载荷的环节：

1. 客户端发起的函数调用被转换为 HTTP 请求。
2. 服务端解析载荷并执行对应的函数。
3. 攻击者可以伪造恶意载荷，诱使 React 在反序列化过程中执行任意代码。

在官方确认补丁完全部署之前，更多技术细节将保持保密，以免漏洞被大规模利用。

## 🎯 行动建议

1. **立即确认**项目所用的 React 与框架版本。
2. **马上升级** React 核心依赖以及框架/打包器提供的 RSC 支持包。
3. **同步通知团队**、合作伙伴及客户，确保所有部署都得到修复。
4. **监控服务器日志**与入侵检测，关注是否存在可疑请求。
5. **持续关注**官方公告（React、Next.js、Expo、Redwood 等），获取最新补丁状态。

## 升级指南（框架 & 构建工具）

### Next.js

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 ``` | ``` npm install next@15.0.5   # 15.0.x npm install next@15.1.9   # 15.1.x npm install next@15.2.6   # 15.2.x npm install next@15.3.6   # 15.3.x npm install next@15.4.8   # 15.4.x npm install next@15.5.7   # 15.5.x npm install next@16.0.7   # 16.0.x ``` |

若使用 `14.3.0-canary.77` 或更高的 canary，请降级至最新 14.x 稳定版：

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install next@14 ``` |

详见 [Next.js 安全公告](https://nextjs.org/blog/CVE-2025-66478)。

### React Router（不稳定 RSC API）

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` npm install react@latest npm install react-dom@latest npm install react-server-dom-parcel@latest npm install react-server-dom-webpack@latest npm install @vitejs/plugin-rsc@latest ``` |

### Expo

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install react@latest react-dom@latest react-server-dom-webpack@latest ``` |

### Redwood SDK

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` npm install rwsdk@latest npm install react@latest react-dom@latest react-server-dom-webpack@latest ``` |

更多迁移说明见 [Redwood 文档](https://docs.rwsdk.com/migrating/)。

### Waku

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install react@latest react-dom@latest react-server-dom-webpack@latest waku@latest ``` |

详情参考 [Waku 官方讨论](https://github.com/wakujs/waku/discussions/1823)。

### @vitejs/plugin-rsc

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install react@latest react-dom@latest @vitejs/plugin-rsc@latest ``` |

### react-server-dom-parcel

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install react@latest react-dom@latest react-server-dom-parcel@latest ``` |

### react-server-dom-turbopack

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install react@latest react-dom@latest react-server-dom-turbopack@latest ``` |

### react-server-dom-webpack

|  |  |
| --- | --- |
| ``` 1 ``` | ``` npm install react@latest react-dom@latest react-server-dom-webpack@latest ``` |

## 🙏 致谢

感谢安全研究员 [Lachlan Davidson](https://github.com/lachlan2k) 发现并报告漏洞，也向在补丁发布期间提供临时缓解与验证的托管服务商、框架团队和社区贡献者致以谢意。

## 📢 重要提醒

**安全无小事！** 该漏洞的严重性不容忽视，请务必在今天完成升级，保障用户数据与业务连续性。

**转发给你的技术团队**，让更多开发者看到这条重要信息！

【译】React 服务器组件中的关键安全漏洞

<https://blog.vadxq.com/article/security-in-react-server-components/>

###### 作者

vadxq

###### 发布于

2025-12-04

###### 更新于

2025-12-04

###### 许可协议

#[前端](/tags/%E5%89%8D%E7%AB%AF/)[Nexjts](/tags/Nexjts/)[工程化](/tags/%E5%B7%A5%E7%A8%8B%E5%8C%96/)[React](/tags/React/)

### 喜欢这篇文章？打赏一下作者吧

支付宝![支付宝](https://qnimg.vadxq.com/blog/2019/zhifubao.jpg)微信![微信](https://qnimg.vadxq.com/blog/2016/weixinreward.png)

[Vercel收购NuxtLabs，我和尤雨溪一样心情复杂](/article/nuxtlabs-is-joining-vercel/)

### 评论

### 目录

* [1🚨 漏洞概述](#🚨-漏洞概述)
* [2⚠️ 你是否受到影响？](#⚠️-你是否受到影响？)
  + [2.1受影响的 React 版本](#受影响的-React-版本)
  + [2.2受影响的框架与工具](#受影响的框架与工具)
  + [2.3以下情况暂不受影响](#以下情况暂不受影响)
* [3🛡️ 紧急修复方案](#🛡️-紧急修复方案)
* [4📅 漏洞时间线](#📅-漏洞时间线)
* [5💡 技术背景](#💡-技术背景)
* [6🎯 行动建议](#🎯-行动建议)
* [7升级指南（框架 & 构建工具）](#升级指南（框架-构建工具）)
  + [7.1Next.js](#Next-js)
  + [7.2React Router（不稳定 RSC API）](#React-Router（不稳定-RSC-API）)
  + [7.3Expo](#Expo)
  + [7.4Redwood SDK](#Redwood-SDK)
  + [7.5Waku](#Waku)
  + [7.6@vitejs/plugin-rsc](#vitejs-plugin-rsc)
  + [7.7react-server-dom-parcel](#react-server-dom-parcel)
  + [7.8react-server-dom-turbopack](#react-server-dom-turbopack)
  + [7.9react-server-dom-webpack](#react-server-dom-webpack)
* [8🙏 致谢](#🙏-致谢)
* [9📢 重要提醒](#📢-重要提醒)

![vadxq](https://qnimg.vadxq.com/blog/2016/blogheadimg20160517.jpg)

vadxq

做一个简单低调且浪漫的技术人

中国

文章

[29](/archives)

分类

[40](/categories)

标签

[56](/tags)

[关注我](https://github.com/vadxq)

### 分类

* [AI1](/categories/AI/)
  + [搜索1](/categories/AI/%E6%90%9C%E7%B4%A2/)
* [Flutter3](/categories/Flutter/)
  + [install1](/categories/Flutter/install/)
* [Gemini1](/categories/Gemini/)
  + [Gemini-Cli1](/categories/Gemini/Gemini-Cli/)
    - [Claude-Code1](/categories/Gemini/Gemini-Cli/Claude-Code/)
* [LLM2](/categories/LLM/)
  + [模型2](/categories/LLM/%E6%A8%A1%E5%9E%8B/)
    - [DeepSeek1](/categories/LLM/%E6%A8%A1%E5%9E%8B/DeepSeek/)
      * [Qwen1](/categories/LLM/%E6%A8%A1%E5%9E%8B/DeepSeek/Qwen/)
        + [OpenAI1](/categories/LLM/%E6%A8%A1%E5%9E%8B/DeepSeek/Qwen/OpenAI/)
    - [Google1](/categories/LLM/%E6%A8%A1%E5%9E%8B/Google/)
      * [A2A1](/categories/LLM/%E6%A8%A1%E5%9E%8B/Google/A2A/)
* [Macbook1](/categories/Macbook/)
* [Vercel1](/categories/Vercel/)
  + [Nuxtjs1](/categories/Vercel/Nuxtjs/)
    - [Vue1](/categories/Vercel/Nuxtjs/Vue/)
* [iOS1](/categories/iOS/)
  + [Swift1](/categories/iOS/Swift/)
* [lenny1](/categories/lenny/)
  + [Newsletter1](/categories/lenny/Newsletter/)
    - [Cursor1](/categories/lenny/Newsletter/Cursor/)
* [linux1](/categories/linux/)
  + [ssh1](/categories/linux/ssh/)
* [前端开发7](/categories/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/)
  + [前端工具1](/categories/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/%E5%89%8D%E7%AB%AF%E5%B7%A5%E5%85%B7/)
  + [前端工程化3](/categories/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%8C%96/)
  + [前端重温2](/categories/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/%E5%89%8D%E7%AB%AF%E9%87%8D%E6%B8%A9/)
  + [包管理器1](/categories/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/%E5%8C%85%E7%AE%A1%E7%90%86%E5%99%A8/)
* [区块链1](/categories/%E5%8C%BA%E5%9D%97%E9%93%BE/)
  + [Web31](/categories/%E5%8C%BA%E5%9D%97%E9%93%BE/Web3/)
* [博客1](/categories/%E5%8D%9A%E5%AE%A2/)
* [年终总结4](/categories/%E5%B9%B4%E7%BB%88%E6%80%BB%E7%BB%93/)
* [攻略1](/categories/%E6%94%BB%E7%95%A5/)
  + [旅游攻略1](/categories/%E6%94%BB%E7%95%A5/%E6%97%85%E6%B8%B8%E6%94%BB%E7%95%A5/)
* [服务器2](/categories/%E6%9C%8D%E5%8A%A1%E5%99%A8/)
* [算法1](/categories/%E7%AE%97%E6%B3%95...