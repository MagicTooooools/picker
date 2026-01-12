---
title: Superpowers - An open-source development workflow and skills library for coding agents that …
url: https://jimmysong.io/ai/superpowers/
source: Jimmy Song – Jimmy Song's Blog
date: 2026-01-11
fetch_date: 2026-01-12T03:39:50.827924
---

# Superpowers - An open-source development workflow and skills library for coding agents that …

[Read: From using AI to building AI systems](/blog/from-using-ai-to-building-ai-systems/), a defining note on what I’m exploring.

[![Brand Icon](/images/icons/jimmysong.svg)
AI NATIVE INFRA](/)

Search
⌘K

* [Blog](/blog/)

  ### Explore the Blog

  Dive into deep analyses on AI, cloud native, and industry trends.

  [View all](/blog/)

  + Technology
  + More

  #### Technology

  + [AI Engineering

    Artificial Intelligence engineering, large language models, and intelligent agent applications](/categories/ai-engineering/)
  + [Cloud Native

    Cloud native infrastructure including Kubernetes, Service Mesh, Envoy, and Gateway API](/categories/cloud-native/)
  + [Engineering Practice

    Engineering culture, development practices, processes, and tools](/categories/engineering-practice/)
  + [Open Source

    Open source ecosystem, community contributions, and industry insights](/categories/open-source/)

  #### More

  + [Review

    Reviews and commentary on books, movies, games, hardware, and incidents.](/categories/review/)
  + [Travel

    Travel journals and experiences from around the world.](/categories/travel/)
  + [Conference

    Recaps and insights from tech conferences like KubeCon.](/categories/conference/)
  + [Perspective

    Thoughts and analysis on economics, culture, politics, and society.](/categories/perspective/)
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

  + [AI OSS Landscape

    AI resources for developers and tech enthusiasts.](/ai/)
  + [ArkSphere AI

    AI Native Infra, Agentic Runtime community.](/community/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
  + [Glossary

    Bilingual terminology glossary for translation and writing reference.](/glossary/)
* [About](/about/)
* [中文
  中文](/zh/ai/superpowers/)

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

[Home](/)[AI OSS Landscape](/ai/)
Superpowers

# Superpowers

An open-source development workflow and skills library for coding agents that emphasizes TDD, process, and verifiable automation.

Jesse Vincent
·

Since 2025-10-09

Loading score...

Score:
--
/ 100

--
|
--

Score data unavailable

[GitHub](https://github.com/obra/superpowers) [Website](https://blog.fsck.com/2025/10/09/superpowers/)

## Detailed Introduction

Superpowers is an open-source skills library and development workflow for coding agents. Before a coding agent writes code, Superpowers guides design refinement, presents designs in digestible chunks for approval, then produces an executable implementation plan. It then drives subagent-driven development with two-stage review to ensure implementations follow the plan. The project emphasizes test-driven development (RED-GREEN-REFACTOR) and process-driven simplicity to make autonomous coding predictable and verifiable.

## Main Features

* Triggered skills (brainstorming, writing plans, executing plans, requesting code review) that activate at the right stages.
* Enforced test-driven workflow to ensure changes are covered by failing tests before implementation.
* Subagent-driven parallel task execution with two-stage reviews (spec compliance, code quality).
* Built-in git worktree workflows, tmux monitoring, and plugin marketplace installation (e.g., Claude Code plugin).
* Clear contributor guides for adding new skills under the `skills/` directory.

## Use Cases

* Hand off coding tasks to agents while maintaining design reviewability and auditability.
* Rapidly build prototypes with strong test coverage using Superpowers’ TDD-first workflow.
* Break work into small tasks and execute them in parallel via subagents to accelerate delivery.
* Reuse skills across agent platforms such as Claude Code, Codex, and OpenCode.

## Technical Characteristics

* Script- and config-driven skills library suitable for multiple agent platforms.
* Supports Claude Code plugin marketplace installation and includes docs for other platforms.
* Emphasizes testability and verifiability with example tests and documentation in the repo.
* Lightweight, modular design for minimal-friction integration into existing automation pipelines.

![Superpowers](https://opengraph.githubassets.com/1/obra/superpowers)

##### Score Breakdown

🤖 Agent Framework
🦾 Agents
🛠️ Dev Tools

##### Related Resources

###### [ValueCell](/ai/valuecell/)

A community-driven multi-agent platform for finance that offers research, …

Author：ValueCell AI

###### [Midscene.js](/ai/midscene/)

A vision-language-model driven, cross-platform UI automation framework that uses …

Author：web-infra-dev

###### [Skills](/ai/anthropic-skills/)

An open-source library from Anthropic for defining, sharing, and reusing …

Author：Anthropic

Navigation

* [Advanced Search](/search/)
* [Slides Share](/slide/)
* [Blog Post Tags](/tags/)
* [AI OSS Landscape](/ai/)

News

* [KCD Beijing 2026](/notice/kcd-beijing-2026/)
* [ArkSphere Community Launch](/notice/announcement-arksphere-community/)
* [New Liquid Glass Theme](/notice/liquid-glass-theme/)
* [Website Update](/notice/site-major-update-202506/)

Featured

* [Envoy Fundamentals](https://academy.tetrate.io/courses/envoy-fundamentals)
* [Istio Fundamentals](https://academy.tetrate.io/courses/istio-fundamentals)
* [RAG Chatbot](https://github.com/rootsongjc/rag-chatbot)
* [Istio Multi-Cluster](https://github.com/rootsongjc/istio-multi-cluster)

About

* [Jimmy](/about/)
* [Contact](/contact/)
* [Business](/business/)
* [Community](/community/)

Powered By

* [Cloudflare](https://cloudflare.com)
* [Hugo](https://gohugo.io)
* [VS Code](https://code.visualstudio.com)
* [GitHub](https://github.com/rootsongjc)

Follow

* [X(Twitter)](https://x.com/jimmysongio)
* ![footer image](/images/jimmysong-x-qr-code.png)

© 2017-2026 Jimmy Song All Right Reserved · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)