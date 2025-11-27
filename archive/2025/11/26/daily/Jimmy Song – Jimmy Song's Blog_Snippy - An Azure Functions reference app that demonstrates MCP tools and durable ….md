---
title: Snippy - An Azure Functions reference app that demonstrates MCP tools and durable …
url: https://jimmysong.io/ai/snippy/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-11-26
fetch_date: 2025-11-27T16:52:22.656670
---

# Snippy - An Azure Functions reference app that demonstrates MCP tools and durable …

Open to career opportunities and collaborations.
[Learn more](/about/).

[JIMMY SONG](/)

Search
⌘K

* [Blog](/blog/)

  ### Explore the Blog

  Dive into deep analyses on AI, cloud native and industry trends.

  [View all articles](/blog/)

  + Columns
  + Others

  #### Columns

  + [AI Engineering

    Artificial Intelligence engineering, large language models, and intelligent agent applications](/categories/ai-engineering/)
  + [Cloud Native

    Cloud native infrastructure including Kubernetes, Service Mesh, Envoy, and Gateway API](/categories/cloud-native/)
  + [Engineering Practice

    Engineering culture, development practices, processes, and tools](/categories/engineering-practice/)
  + [Open Source

    Open source ecosystem, community contributions, and industry insights](/categories/open-source/)

  #### Others

  + [Archive

    Archived content (travel, reviews, personal updates, etc.)](/categories/archive/)
* [Explore](/)

  ### Discovery

  Explore content from multiple dimensions: by category, tag, search, and travel stories.

  + Feedback
  + More

  #### Feedback

  + [Contact

    Get in touch for sponsorships, partnerships, or inquiries.](/contact/)
  + [Business

    Partnerships, sponsorships, and in-depth industry analysis.](/business/)
  + [Announcements

    Stay up-to-date with the latest news and updates.](/notice/)

  #### More

  + [AI Resources

    AI resources for developers and tech enthusiasts.](/ai/)
  + [Advanced Search

    Full-text site search for precise content discovery.](/search/)
  + [Blog Analysis

    Explore Jimmy's writing journey and content distribution.](/analysis/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
* [About](/about/)
* [中文
  中文](/zh/ai/snippy/)

#### Search

Search

Type:

All Types
Blog
AI
Book
News

**Keyboard Navigation:**

`Tab` next result
|
`Shift+Tab` previous result
|
`↑↓` navigate
|
`Enter` open link
|
`Esc` Close

[Home](/)[AI Resources](/ai/)
Snippy

# Snippy

An Azure Functions reference app that demonstrates MCP tools and durable multi-agent orchestration.

Author:
Microsoft

Since:
2025-04-24

[Visit Website](https://learn.microsoft.com/azure/ai-services/) [GitHub](https://github.com/Azure-Samples/snippy)

## Detailed Introduction

Snippy is an open-source Azure Functions reference application that demonstrates exposing functions as MCP (Model Context Protocol) tools and coordinating stateful multi-agent workflows with Durable Agents. The project integrates Azure OpenAI for embeddings and LLM calls, Cosmos DB vector indexing for semantic code retrieval, and the Durable Task Scheduler for orchestrations. It provides labs and an `azd` workflow to provision and deploy the full stack locally or on Azure.

## Main Features

* MCP tool integration: expose Azure Functions as discoverable tools for AI assistants.
* Multi-agent orchestration: coordinate specialized agents (e.g., DeepWiki, CodeStyle) using Durable Agents and DTS.
* Vector search: semantic code-snippet retrieval powered by Cosmos DB vector indexing.
* One-click deployment: `azd up` provisions functions, Cosmos DB, OpenAI and observability components.

## Use Cases

* Learning & labs: hands-on tutorial to learn MCP tools, durable functions and vector search patterns.
* Developer assistants: demonstrate building tools discoverable by GitHub Copilot and similar assistants.
* Reproducible stacks: quick start templates for Codespaces, Dev Containers, and CI-driven deployments.

## Technical Features

* Platform & language: Python-based Azure Functions with `azd` and Bicep infrastructure templates.
* Observability: DTS dashboard and telemetry for monitoring orchestration and tool invocations.
* Extensibility: modular tool matrix and pipeline design to swap model backends and retrieval layers.
* License: MIT, suitable for learning, reproduction and extension.

Loading comments...
0

![Snippy](https://opengraph.githubassets.com/1/Azure-Samples/snippy)

##### Resource Info

🦾 Agents
🧩 MCP
🌱 Open Source

##### Related Resources

###### [VSCode Copilot Chat](/ai/vscode-copilot-chat/)

VSCode Copilot Chat is an open-source extension from Microsoft that brings …

Author：Microsoft

###### [Call Center AI](/ai/call-center-ai/)

Microsoft's open-source Call Center AI demonstrates integrating Azure …

Author：Microsoft

###### [Amplifier](/ai/amplifier/)

Microsoft's tooling for development and deployment assistance, aimed at …

Author：Microsoft

Navigation

* [Advanced Search](/search/)
* [Slides Share](/slide/)
* [Blog Post Tags](/tags/)
* [AI Resources](/ai/)

News

* [New Liquid Glass Theme](/notice/liquid-glass-theme/)
* [Website Update](/notice/site-major-update-202506/)
* [Istio Fundamentals](/notice/istio-fundamentals-course-updated/)
* [KubeCon China 2024](/notice/kubecon-china-2024-panel/)

Featured

* [Envoy Fundamentals](https://academy.tetrate.io/courses/envoy-fundamentals)
* [Istio Fundamentals](https://academy.tetrate.io/courses/istio-fundamentals)
* [RAG Chatbot](https://github.com/rootsongjc/rag-chatbot)
* [Istio Multi-Cluster](https://github.com/rootsongjc/istio-multi-cluster)

About

* [Jimmy](/about/)
* [Contact](/contact/)
* [Business](/business/)
* [Sponsor](/sponsor/)

Powered By

* [Cloudflare](https://cloudflare.com)
* [Hugo](https://gohugo.io)
* [VS Code](https://code.visualstudio.com)
* [GitHub](https://github.com/rootsongjc)

Follow

* [X(Twitter)](https://x.com/jimmysongio)
* ![footer image](/images/jimmysong-x-qr-code.png)

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribution required.

© 2017-2025 Jimmy Song All Right Reserved