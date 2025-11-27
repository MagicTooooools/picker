---
title: 免费和付费 AI API 选择指南
url: https://ssshooter.com/ai-services-guide/
source: Usubeni Fantasy
date: 2025-11-26
fetch_date: 2025-11-27T16:52:18.900110
---

# 免费和付费 AI API 选择指南

[skip to content](#main)

[![usubeni fantasy logo](/logo-mobile.png) Usubeni Fantasy](/)    [归档](/archive/)  [标签](/tags/)  [关于](/about/)  [友链](/links/)  [虫洞](https://www.foreverblog.cn/go.html)

Close

     Dark Theme

## 目录

* <#免费方案>
  + [#Google Gemini](#google-gemini)
  + [#OpenRouter](#openrouter)
  + <#硅基流动>
  + <#公益站>
* <#付费方案>
  + [#OpenAI GPT](#openai-gpt)
  + [#Anthropic Claude](#anthropic-claude)
  + [#Deepseek](#deepseek)
  + [#BigModel](#bigmodel)
* <#注意事项>
  + <#配置>
  + <#隐私安全>
  + <#使用限制>
  + <#故障排除>
* <#总结>

# 免费和付费 AI API 选择指南

2025/11/26  / 8 分钟阅读

[人工智能](/tag/%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD/)  ,  [免费 AI](/tag/%E5%85%8D%E8%B4%B9%20AI/)  ,  [资源](/tag/%E8%B5%84%E6%BA%90/)

免费的 AI 服务非常多，但是免费的多是聊天功能。现在又很多应用的 AI 服务都需要用户自带 Key，AI 这东西从原理上来说又比较贵，所以使用 Key 进行 API 调用时往往是需要付费的。

但方法总比困难多，还是有一些免费或者低成本的方案可以选择。

下面就来总结一些目前比较好用的 AI 提供商，付费免费都有，供大家参考。

## 免费方案

### Google Gemini

**推荐理由：**

* 部分可免费使用（不过最近免费额度似乎缩紧了）
* 性能优秀，总结质量高
* 支持超长上下文
* 响应速度快

![Google AI Studio 获取 key](https://img.ssshooter.com/img/ai-services-guide/google.jpg)

**获取方式：**

1. 访问 [Google AI Studio](https://aistudio.google.com/)
2. 使用Google账号登录
3. 创建API Key
4. 在插件配置中选择”Google Gemini”并填入API Key

![Google AI Studio 复制 key](https://img.ssshooter.com/img/ai-services-guide/google-key.jpg)

### OpenRouter

<https://openrouter.ai/>

**推荐理由：**

* 大量免费模型可用
* 额度量大管饱

对于[免费模型](https://openrouter.ai/models?max_price=0)（型号ID以 `:free` 结尾）。

目前，比较热门的免费模型包括 [x-ai/grok-4-fast:free](https://openrouter.ai/x-ai/grok-4-fast%3Afree) 和 [deepseek/deepseek-chat-v3.1:free](https://openrouter.ai/deepseek/deepseek-chat-v3.1%3Afree)

对于免费模型，**请求速率限制取决于你已购买的积分数量**。如果你购买了至少 10 个积分，你的免费模型请求速率限制将是每天 1000 次。否则，你的免费模型 API 请求将被限制为每天 50 次。

推荐充值 10 积分，这个额度对于日常使用和开发测试是完全足够的。同时可以配置 Credit limit 为 0 的 Token 避免误用积分。

![openrouter](https://img.ssshooter.com/img/ai-services-guide/openrouter.jpg)

虽然你是充值了 10 刀，但是使用时不花钱，并且**额度十分可观**。所以，勉强也算是免费吧 😂

### 硅基流动

**适用场景：** 当网络无法访问 Gemini 或 OpenRouter 时

**推荐理由：**

* 部分小模型免费使用
* 支持多种开源模型
* 速度相对较慢
* 中国大陆访问友好

缺点：

* 免费模型延迟高，生成速度慢

![硅基流动 获取 key](https://img.ssshooter.com/img/ai-services-guide/siliconflow.jpg)

2025.11.23 更新：现在已经没有免费这个筛选项了，只能在列表里找标价为免费的模型。

**获取方式：**

1. 访问 [硅基流动](https://cloud.siliconflow.cn/)
2. 注册账号并完成认证
3. 获取API Key
4. 在插件配置中选择”自定义”，填入硅基流动的API地址和Key

![硅基流动 复制 key](https://img.ssshooter.com/img/ai-services-guide/siliconflow-key.jpg)

### 公益站

在 [linux.do](https://linux.do/)、[V2EX](https://www.v2ex.com/)、[X](https://x.com/) 等论坛或社媒蹲公益站。

![linux.do 公益站](https://img.ssshooter.com/img/ai-services-guide/linux-do-free-ai.jpg)

随便搜一下就有很多 👆

公益站的系统基本都是像下面这样的一套 👇

![公益站示例](https://img.ssshooter.com/img/ai-services-guide/free-ai.jpg)

## 付费方案

### OpenAI GPT

**官网地址：** <https://openai.com/>

**特点：**

* 掀起 AI 狂热的始祖
* 业界领先的（期间限定）AI 能力

### Anthropic Claude

**官网地址：** <https://claude.ai/>

**特点：**

* 擅长长文本理解
* 能力强，泛用性高
* 编程能力绝对领先

### Deepseek

**官网地址：** <https://deepseek.com/>

**特点：**

* 超高性价比
* 响应速度快

### BigModel

也就是 GLM 系列。

**官网地址：** <https://bigmodel.cn/>

**特点：**

* 编程能力较强
* Claude 平替，可以丝滑接入 Claude Code

## 注意事项

### 配置

几乎 100% 供应商都有适配 openAI API 的接口，如果你使用的应用没有明确支持某家供应商，但也几乎一定支持 openAI 兼容接口，在对应文档找到接口地址和 Key 填入即可。

OpenAI 兼容接口的优点：

* **标准化格式**：遵循 OpenAI 的 API 规范，使用相同的请求和响应格式
* **简化集成**：应用只需适配一套接口即可支持多个供应商
* **广泛兼容**：几乎所有的 AI 应用都支持这种接口格式
* **功能完整**：支持聊天完成、嵌入、流式输出等核心功能

常见 OpenAI 兼容接口地址示例：

```
# OpenRouter

https://openrouter.ai/api/v1

# 硅基流动

https://api.siliconflow.cn/v1

# Deepseek

https://api.deepseek.com/v1

# Google Gemini (OpenAI 兼容模式)

https://generativelanguage.googleapis.com/v1beta/openai/

# BigModel (智谱AI)

https://open.bigmodel.cn/api/paas/v4
```

![AI 配置例](https://img.ssshooter.com/img/ai-services-guide/ai-config.jpg)

以 [Mind Elixir Desktop](https://mind-elixir.com/) 为例，在设置中输入以下关键数据即可接通 AI 供应商：

* API 地址（Endpoint）：供应商提供的 API 访问地址，这需要参考你的供应商文档，大部分地址都是以 `/v1` 结尾，不过也有像 gemini 那样 `https://generativelanguage.googleapis.com/v1beta/openai/` 的非主流
* API Key：供应商提供的访问密钥，**记得注意密钥安全**
* 模型名称（Model）：选择或填写所需使用的模型名称（部分 AI 工具可以通过 Endpoint 自行搜索可用模型）

### 隐私安全

交出 API Key 等同于交出账号控制权，请务必确保 Key 的安全性，避免泄露给他人。同样的，你在使用 AI 应用时**必须确保应用的安全性**，避免 Key 被恶意使用。

使用非官方服务时，**请务必确认服务商的信誉和安全性**，避免使用来路不明的服务，**以防止数据泄露或滥用**。

### 使用限制

* 各服务商都有使用频率限制
* 免费服务可能有每日配额
* 网络状况影响响应速度

### 故障排除

* API Key 错误：检查 Key 是否正确复制
* 网络超时：尝试更换网络或服务商
* 配额用尽：等待重置或升级付费计划

## 总结

AI 服务市场已经相当成熟，用户可以根据自己的需求和预算选择合适的方案：

* **Google Gemini** 性能优秀，免费额度相对充足
* **OpenRouter** 充值 10 美元解锁大量免费模型，性价比极高，**强烈推荐**
* **硅基流动** 国内用户备选，网络访问稳定
* **编程开发**：Anthropic Claude 或 BigModel (GLM)
* **性价比优先**：Deepseek
* **公益站** 社区资源，适合临时使用

**关键使用建议：**

* 善用 OpenAI 兼容接口，一个配置通用多个平台
* 注意 API Key 安全，避免泄露
* 根据使用场景选择合适模型，避免资源浪费
* 关注各平台的免费政策和限额变化

随着 AI 技术的快速发展，各供应商的服务和价格也在不断调整，建议大家根据自己的实际需求和使用频率，灵活选择最适合的方案。记住，免费的往往是最贵的，在选择服务商时，平衡性能、稳定性和成本才是明智之举。

评论组件加载中……

© SSShooter 2025.[🚀 Usubeni Fantasy](/)

  [归档](/archive/)  [标签](/tags/)  [关于](/about/)  [友链](/links/)  [虫洞](https://www.foreverblog.cn/go.html)