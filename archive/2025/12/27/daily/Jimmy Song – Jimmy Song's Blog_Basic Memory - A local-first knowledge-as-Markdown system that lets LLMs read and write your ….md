---
title: Basic Memory - A local-first knowledge-as-Markdown system that lets LLMs read and write your …
url: https://jimmysong.io/ai/basic-memory/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-27
fetch_date: 2025-12-28T03:39:08.452509
---

# Basic Memory - A local-first knowledge-as-Markdown system that lets LLMs read and write your …

[Read: From using AI to building AI systems](/blog/from-using-ai-to-building-ai-systems/), a defining note on what I’m exploring.

[![Brand Icon](/images/icons/jimmysong.svg)
AI NATIVE INFRA](/)

Search
⌘K

* [Blog](/blog/)

  ### Explore the Blog

  Dive into deep analyses on AI, cloud native and industry trends.

  [View all](/blog/)

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

  + [AI OSS Landscape

    AI resources for developers and tech enthusiasts.](/ai/)
  + [ArkSphere AI

    AI Native Infra, Agentic Runtime community.](/community/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
* [About](/about/)
* [中文
  中文](/zh/ai/basic-memory/)

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
Basic Memory

# Basic Memory

A local-first knowledge-as-Markdown system that lets LLMs read and write your memory via the Model Context Protocol (MCP).

Basic Machines
·

Since 2024-12-02

Loading score...

Score:
--
/ 100

--
|
--

Score data unavailable

[GitHub](https://github.com/basicmachines-co/basic-memory) [Website](https://basicmemory.com)

## Detailed Introduction

Basic Memory is a local-first knowledge system that stores user knowledge as structured Markdown files and exposes them to compatible LLMs via the Model Context Protocol (MCP). It implements a writable “memory” concept that keeps data local by default while offering optional cloud sync and cross-device collaboration, making it suitable as a persistent personal knowledge base and conversational context layer.

## Main Features

* Local-first storage: Notes are plain Markdown files under user control.
* Bi-directional read/write: Both humans and LLMs read and write the same files, building a traversable memory graph.
* MCP support: Implements the Model Context Protocol for cross-tool interoperability.
* Lightweight indexing: Uses a local SQLite index for fast search and traversal.
* CLI & integrations: Provides CLI tools and integrations with editors and apps like VS Code and Claude Desktop.

## Use Cases

Fits scenarios that require persistent conversational context: developer project knowledge, research notes with semantic search, live-note syncing for meetings or streams, and personal assistants that maintain long-term memory across sessions. It can be used as a privacy-preserving alternative to cloud-only RAG setups.

## Technical Features

Parses Markdown files into Entities, Observations and Relations and builds a local SQLite index to support retrieval and graph traversal. The system provides a MCP server component, event-driven APIs, and bidirectional sync, enabling LLM-driven knowledge writing while keeping data under the user’s control.

Comments

Loading comments...
0

![Basic Memory](https://opengraph.githubassets.com/1/basicmachines-co/basic-memory)

##### Score Breakdown

🧏 Memory
🛠️ Dev Tools
💻 CLI
📱 Application

##### Related Resources

###### [DataFlow](/ai/dataflow/)

A data preparation and pipeline platform for domain training and …

Author：OpenDCAI

###### [sqlite-vector](/ai/sqlite-vector/)

Integrates embedding storage and vector search into SQLite, providing a …

Author：SQLiteAI

###### [PyMuPDF](/ai/pymupdf/)

A high-performance Python library for data extraction, analysis, conversion, and …

Author：Artifex

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