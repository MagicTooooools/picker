---
title: PatrickStar - A framework that enables parallel training of large pretrained models via …
url: https://jimmysong.io/ai/patrickstar/
source: Jimmy Song – Jimmy Song's Blog
date: 2026-01-12
fetch_date: 2026-01-13T03:34:14.496907
---

# PatrickStar - A framework that enables parallel training of large pretrained models via …

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
  中文](/zh/ai/patrickstar/)

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
PatrickStar

# PatrickStar

A framework that enables parallel training of large pretrained models via chunk-based dynamic memory management.

Tencent
·

Since 2021-04-02

Loading score...

Score:
--
/ 100

--
|
--

Score data unavailable

[GitHub](https://github.com/tencent/patrickstar) [Website](https://arxiv.org/abs/2108.05818)

## Detailed Introduction

PatrickStar is a PyTorch-based parallel training framework for large pretrained models (PTMs). It uses a chunk-based dynamic memory management and heterogeneous training strategy to offload non-critical data to CPU memory, allowing training of larger models with fewer GPUs and reducing OOM risk. Maintained by Tencent’s NLP / WeChat AI teams, PatrickStar aims to democratize access to large-scale model training.

## Main Features

* Chunk-based dynamic memory scheduling: manage activations and parameters by computation windows to lower GPU memory usage.
* Heterogeneous offloading: move non-immediate data to CPU to support mixed CPU/GPU memory usage.
* Efficient communication and scalability: optimized collective operations for multi-GPU and multi-node setups.
* PyTorch compatibility: configuration style similar to DeepSpeed for easier migration.

## Use Cases

Suitable for pretraining and large-scale fine-tuning, especially when hardware is constrained and teams need to train models in the tens to hundreds of billions of parameters. Also useful for benchmarking, framework research, and academic or research teaching environments.

## Technical Features

PatrickStar implements chunk-based memory management and runtime dynamic scheduling to keep only the chunks needed for current computation while asynchronously migrating others. It optimizes collective communication for multi-card efficiency and provides benchmarks and examples for V100/A100 clusters. The project is released under BSD-3-Clause.

![PatrickStar](https://opengraph.githubassets.com/1/Tencent/PatrickStar)

##### Score Breakdown

🏋️ Training
⚡ Optimization
🏗️ Framework
📁 Project
🖥️ ML Platform

##### Related Resources

###### [AngelSlim](/ai/angelslim/)

AngelSlim is a model compression toolkit from Tencent providing easy-to-use …

Author：Tencent

###### [HunyuanImage-3.0](/ai/hunyuanimage-3-0/)

HunyuanImage-3.0 is an open-source native multimodal image generation model from …

Author：Tencent

###### [PromptEnhancer](/ai/promptenhancer/)

A chain-of-thought prompt rewriting utility that restructures input prompts into …

Author：Tencent

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