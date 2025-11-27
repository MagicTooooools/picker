---
title: Continuous Claude - A lightweight CLI that runs Claude Code in a loop to create PRs, wait for CI …
url: https://jimmysong.io/ai/continuous-claude/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-11-26
fetch_date: 2025-11-27T16:52:21.918109
---

# Continuous Claude - A lightweight CLI that runs Claude Code in a loop to create PRs, wait for CI …

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
  中文](/zh/ai/continuous-claude/)

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
Continuous Claude

# Continuous Claude

A lightweight CLI that runs Claude Code in a loop to create PRs, wait for CI checks, and merge changes.

Author:
Anand Chowdhary

Since:
2025-11-15

[Visit Website](https://anandchowdhary.com/open-source/2025/continuous-claude) [GitHub](https://github.com/AnandChowdhary/continuous-claude)

## Detailed Introduction

Continuous Claude is a lightweight CLI that runs
[Claude Code](https://code.claude.com)in a loop to perform iterative development: each iteration generates changes on a branch, creates a pull request, waits for CI checks and reviews, and merges when checks pass. The project uses a shared markdown file (default `SHARED_TASK_NOTES.md`) as persistent context so subsequent runs remember previous decisions and outstanding tasks, enabling a relay-like autonomous workflow. It depends on the GitHub CLI (`gh`) and access to Claude Code, making it suitable for long-running, incremental improvements on large repositories.

## Main Features

* Loop-driven execution with configurable max runs or cost budget.
* Full PR lifecycle automation: branch, commit, PR, monitor checks, merge or discard.
* Persistent context via a shared notes file to reduce context drift between runs.
* Parallel execution support via git worktrees for concurrent subtask processing.

## Use Cases

Ideal for large-scale, long-term repository work such as incrementally adding unit tests, fixing post-upgrade breakages, large refactors, or automated style migrations. Teams that want to hand off repetitive engineering tasks to models while keeping human review via PR/CI will find Continuous Claude acts as an enhanced Dependabot or automation assistant.

## Technical Features

* Shell/CLI-first design, easy to run in CI, containers, or developer machines.
* Integrates Claude Code with `gh` to wire model outputs into standard GitHub workflows.
* Supports `--disable-commits` dry-run mode for safe validation.
* MIT-licensed and documented on the project homepage for research and experimentation.

Loading comments...
0

![Continuous Claude](https://opengraph.githubassets.com/1/AnandChowdhary/continuous-claude)

##### Resource Info

🦾 Agents
💻 CLI
🛠️ Dev Tools
🌱 Open Source

##### Related Resources

###### [FinRobot](/ai/finrobot/)

An open-source AI agent platform for financial analysis that integrates …

Author：AI4Finance Foundation

###### [DocsGPT](/ai/docs-gpt/)

An open-source enterprise document agent platform combining RAG and multi-model …

Author：arc53

###### [DiffMem](/ai/diffmem/)

A git-based differential memory backend that keeps current-state files and …

Author：Growth Kinetics

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