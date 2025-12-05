---
title: Dembrandt - A Playwright-based CLI tool that extracts logos, colors, typography, spacing, …
url: https://jimmysong.io/ai/dembrandt/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-04
fetch_date: 2025-12-05T03:20:50.761428
---

# Dembrandt - A Playwright-based CLI tool that extracts logos, colors, typography, spacing, …

[ArkSphere Community](https://arksphere.dev/): AI-native runtime, infrastructure, and open source.

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
  + [ArkSphere AI

    AI Native Infra, Agentic Runtime community.](/community/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
* [About](/about/)
* [中文
  中文](/zh/ai/dembrandt/)

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
Dembrandt

# Dembrandt

A Playwright-based CLI tool that extracts logos, colors, typography, spacing, and components from any website and exports structured design tokens as JSON.

Author:
thevangelist

Since:
2025-11-22

[GitHub](https://github.com/thevangelist/dembrandt)

## Detailed Introduction

Dembrandt is a Playwright-based command-line tool that extracts a website’s design system into structured design tokens (JSON) in seconds. It renders pages, collects computed styles, analyzes color usage and typography patterns, and assigns confidence scores to results—making it useful for audits, documentation, and multi-site consolidation workflows.

## Key Features

* Extract logos, semantic colors, palettes, CSS variables, and typography automatically.
* Detect spacing scales, border radii, shadows, and responsive breakpoints.
* Save JSON output to `output/domain.com/YYYY-MM-DDTHH-MM-SS.json` with confidence metadata.
* Support flags like `--json-only`, `--dark-mode`, `--mobile`, and `--debug` for flexible extraction modes.

## Use Cases

* Brand audits and competitive analysis to capture visual guidelines quickly.
* Building or documenting a design system and tokens library.
* Consolidating styles across multiple sites for rebranding or migration.
* Generating baseline style references for frontend engineering.

## Technical Features

* Renders pages with Playwright and injects anti-detection scripts for robustness.
* Extracts computed DOM styles, groups similar colors, and scores color confidence.
* Waits for SPA hydration to ensure dynamic content is captured.
* Runs extractors in parallel to speed up collection and analysis, producing confidence-scored tokens.

Loading comments...
0

![Dembrandt](https://opengraph.githubassets.com/1/thevangelist/dembrandt)

##### Resource Info

🌱 Open Source
💻 CLI
🛠️ Dev Tools
🖥️ UI
🧲 Utility

##### Related Resources

###### [DroidRun](/ai/droidrun/)

An open-source mobile automation framework that lets you drive device …

Author：DroidRun

###### [DeepTeam](/ai/deepteam/)

An open-source framework for red-teaming large language models and LLM systems, …

Author：Confident AI

###### [Compounding Engineering Plugin](/ai/compounding-engineering-plugin/)

An open-source plugin for engineering compounding scenarios that integrates with …

Author：EveryInc

Navigation

* [Advanced Search](/search/)
* [Slides Share](/slide/)
* [Blog Post Tags](/tags/)
* [AI Resources](/ai/)

News

* [ArkSphere Community Launch](/notice/announcement-arksphere-community/)
* [New Liquid Glass Theme](/notice/liquid-glass-theme/)
* [Website Update](/notice/site-major-update-202506/)
* [Istio Fundamentals](/notice/istio-fundamentals-course-updated/)

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

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribution required.

© 2017-2025 Jimmy Song All Right Reserved