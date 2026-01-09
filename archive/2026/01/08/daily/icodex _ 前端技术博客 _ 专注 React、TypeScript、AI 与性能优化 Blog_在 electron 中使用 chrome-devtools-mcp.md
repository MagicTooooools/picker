---
title: 在 electron 中使用 chrome-devtools-mcp
url: https://icodex.me/electron-devtools-mcp
source: icodex | 前端技术博客 | 专注 React、TypeScript、AI 与性能优化 Blog
date: 2026-01-08
fetch_date: 2026-01-09T03:35:34.480050
---

# 在 electron 中使用 chrome-devtools-mcp

[跳到主要内容](#__docusaurus_skipToContent_fallback)

[![icodex](/img/logo.png)![icodex](/img/logo.png)

**博客**](/)

搜索

[项目](/pages/project)

技术栈

* [TypeScript](/docs/typescript)
* [React](/docs/category/react)
* [JavaScript](/docs/category/javascript)
* [CSS](/docs/category/css)
* [NodeJS](/docs/category/node)

网络

* [Network](/docs/network/guides)

[提示词](/docs/category/prompt)[工程化](/docs/category/%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%8C%96%E4%BD%93%E7%B3%BB)[工具](/docs/category/tool)

Recent posts

* [在 electron 中使用 chrome-devtools-mcp](/electron-devtools-mcp)
* [pnpm 的 catalogs 功能](/pnpm-catalogs)
* [使用 SSE 与 Streamdown 实现 Markdown 流式渲染](/sse-streamdown)
* [JavaScript Intl 对象全面指南](/intl-usage)
* [POML 一种管理 Prompt 的工具](/prompt-tool)
* [如何使用 Translator Web API](/use-translator-webapi)
* [eslint 支持多线程并发 Lint](/eslint-new-concurrency)
* [下一代前端工具链对比](/fe-tool-compare)
* [如何开发自己的一个 shadcn 组件](/shadcn-component)
* [shadcn-ui实现原理](/shadcn)
* [TypeScript全局类型定义的方式](/ts-global-defination)
* [IntersectionObserver API 用法](/intersection)
* [web图像格式对比（三）](/webimage3)
* [web图像格式对比（四）](/webimage4)
* [web图像格式对比（一）](/webimage1)
* [web图像格式对比（二）](/webimage2)
* [混合内容请求限制](/mixcontent)
* [私有网络访问限制](/pna)
* [canvas 污染问题](/tainted_canvas)
* [饥荒云服务器搭建](/dstcloudserver)
* [如何给类库打包](/packlib)
* [实现 Promise.all 有哪些要点](/promiseall)
* [PureEsm](/pureesm)
* [学习vuecli源码的收获](/learncli)
* [vuecli源码解析(1)](/blog/2022-01-23-vuecli%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%901)
* [vuecli源码解析(2)](/blog/2022-01-23-vuecli%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%902)
* [vuecli源码解析(3)](/blog/2022-01-23-vuecli%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%903)
* [vuecli源码解析(4)](/blog/2022-01-23-vuecli%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%904)
* [webpack监听代理地址修改自动重启devServer](/blog/2022-01-05-webpack%E7%9B%91%E5%90%AC%E4%BB%A3%E7%90%86%E5%9C%B0%E5%9D%80%E4%BF%AE%E6%94%B9%E8%87%AA%E5%8A%A8%E9%87%8D%E5%90%AFdevServer)
* [个人网站迁移](/blog/2022-01-04-%E4%B8%AA%E4%BA%BA%E7%BD%91%E7%AB%99%E8%BF%81%E7%A7%BB)
* [webpack代理配置](/blog/2022-01-03-webpack%E4%BB%A3%E7%90%86%E9%85%8D%E7%BD%AE)
* [Promise A+规范的实现](/blog/2022-01-02-Promise%20A%2B%E8%A7%84%E8%8C%83%E7%9A%84%E5%AE%9E%E7%8E%B0)
* [meta标签](/blog/2022-01-02-meta%E6%A0%87%E7%AD%BE)
* [monorepo](/blog/2022-01-02-monorepo)
* [GitHub不再支持密码登录验证](/blog/2021-10-30-GitHub%E4%B8%8D%E5%86%8D%E6%94%AF%E6%8C%81%E5%AF%86%E7%A0%81%E7%99%BB%E5%BD%95%E9%AA%8C%E8%AF%81)

# 在 electron 中使用 chrome-devtools-mcp

2026年1月8日 · 阅读需 4 分钟

[![Oxygen](https://raw.githubusercontent.com/wood3n/wood3n/master/31716713.jpg)](https://github.com/wood3n)

[Oxygen](https://github.com/wood3n)

Front End Engineer

本文介绍在开发 electron 应用时如何使用 chrome-devtools-mcp，比较简单。

## 安装 mcp[​](#安装-mcp "安装 mcp的直接链接")

安装 chrome-devtools-mcp 比较简单，找到 GitHub 仓库，在 README 中能找到各种工具的安装方式，这里就不介绍了。

以 vscode 为例，推荐在项目目录下单独配置，避免多个项目之间配置无法共享。chrome-devtools-mcp 的基本配置如下：

```
{
  "servers": {
    "chrome-devtools-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

## 配置 mcp 连接 electron[​](#配置-mcp-连接-electron "配置 mcp 连接 electron的直接链接")

参考 chrome-devtools-mcp 中的文档 - [Manual connection using port forwarding](https://github.com/ChromeDevTools/chrome-devtools-mcp?tab=readme-ov-file#manual-connection-using-port-forwarding)，也就是通过 `--browser-url` 选项连接到正在运行的 Chrome 实例。适用于在沙箱环境中运行 MCP 服务器，且不允许启动新的 Chrome 实例。那 electron 就符合这种条件。

首先，我们需要在 chrome-devtools-mcp 配置中新增`--browser-url` 配置项，端口号随意

```
{
  "servers": {
    "chrome-devtools-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "chrome-devtools-mcp@latest",
        // 加上这一个参数，端口号随意
        "--browser-url=http://127.0.0.1:9222"
      ]
    }
  }
}
```

然后在 electron 应用中增加以下开发环境配置来启动开发环境调试端口，端口号必须与你在 mcp 中`browser-url` 指定的端口号相同。

```
import isDev from "electron-is-dev";

if (isDev) {
  // 启用远程调试端口
  app.commandLine.appendSwitch("remote-debugging-port", "9222");
  // 可选，开发环境数据隔离
  app.setPath("userData", join(app.getPath("appData"), `electron-mcp-dev`));
}
```

## 执行测试[​](#执行测试 "执行测试的直接链接")

就这么简单两步，接下来就能使用 chrome-devtools-mcp 执行你想做的任何自动测试了。

**标签：**

* [electron](/tags/electron)
* [chrome-devtools-mcp](/tags/chrome-devtools-mcp)
* [ai](/tags/ai)
* [mcp](/tags/mcp)

[较旧一篇

pnpm 的 catalogs 功能](/pnpm-catalogs)

* [安装 mcp](#安装-mcp)
* [配置 mcp 连接 electron](#配置-mcp-连接-electron)
* [执行测试](#执行测试)