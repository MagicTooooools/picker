---
title: DiffMem - A git-based differential memory backend that keeps current-state files and …
url: https://jimmysong.io/ai/diffmem/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-11-26
fetch_date: 2025-11-27T16:52:22.195899
---

# DiffMem - A git-based differential memory backend that keeps current-state files and …

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
  中文](/zh/ai/diffmem/)

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
DiffMem

# DiffMem

A git-based differential memory backend that keeps current-state files and preserves evolution in git history for temporal, auditable agent memory.

Author:
Growth Kinetics

Since:
2025-08-20

[Visit Website](https://github.com/Growth-Kinetics/DiffMem) [GitHub](https://github.com/Growth-Kinetics/DiffMem)

## Detailed Introduction

DiffMem introduces a git-based differential memory approach for AI agents: current-state knowledge is stored as human-readable Markdown files while historical evolution is preserved in Git commits. Agents can load a compact “now” view for fast responses or selectively pull diffs and logs for temporal reasoning. The design emphasizes auditability, portability and token-efficient retrieval for long-horizon memory scenarios.

## Main Features

* Differential view: separate surface (current files) from depth (git history) to keep active contexts lean.
* Human-readable storage: Markdown-based memory units that are easy to inspect and edit.
* Temporal queries: use git diffs and logs for time-aware retrieval and evolution analysis.
* Lightweight prototype: runs in-process with minimal dependencies (e.g., gitpython, rank-bm25) for fast experimentation.

## Use Cases

* Long-lived agents: maintain auditable, reconstructable memory for agents that evolve over years.
* Research & prototyping: explore pruning, smart forgetting and temporal reasoning strategies.
* Collaborative memories: shared repos for multi-agent or multi-user memory workflows with git-based merges.

## Technical Features

* Retrieval strategies: BM25 + semantic hybrid retrieval with multi-depth context assembly (core/wide/deep/temporal).
* No server required: modular library design for local in-process use.
* Extensible: combine with vector stores or external retrieval layers for enhanced semantic capabilities.
* Open-source license: MIT, suitable for research and engineering reuse.

Loading comments...
0

![DiffMem](https://opengraph.githubassets.com/1/Growth-Kinetics/DiffMem)

##### Resource Info

🦾 Agents
🧏 Memory
🌱 Open Source

##### Related Resources

###### [FinRobot](/ai/finrobot/)

An open-source AI agent platform for financial analysis that integrates …

Author：AI4Finance Foundation

###### [DocsGPT](/ai/docs-gpt/)

An open-source enterprise document agent platform combining RAG and multi-model …

Author：arc53

###### [Continuous Claude](/ai/continuous-claude/)

A lightweight CLI that runs Claude Code in a loop to create PRs, wait for CI …

Author：Anand Chowdhary

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