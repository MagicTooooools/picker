---
title: LiteRT - A high-performance, scalable lightweight deep learning inference runtime for …
url: https://jimmysong.io/ai/litert/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-11-26
fetch_date: 2025-11-27T16:52:23.500677
---

# LiteRT - A high-performance, scalable lightweight deep learning inference runtime for …

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
  中文](/zh/ai/litert/)

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
LiteRT

# LiteRT

A high-performance, scalable lightweight deep learning inference runtime for edge devices.

Author:
Google

Since:
2024-09-04

[Visit Website](https://ai.google.dev/edge/litert/next/overview) [GitHub](https://github.com/google-ai-edge/LiteRT)

## Detailed Introduction

LiteRT is Google’s lightweight inference runtime evolved from TensorFlow Lite, designed for deploying machine learning and generative models on resource-constrained edge devices. LiteRT V1 maintains compatibility with the classic TFLite API for existing apps, while LiteRT V2 introduces asynchronous execution, automated accelerator selection, and efficient I/O buffer handling to simplify integrating GPU and NPU acceleration across mobile, embedded, and desktop platforms.

## Main Features

* Cross-platform support: Android, iOS, Linux, macOS, Windows, with extensions planned for Web and IoT.
* Hardware acceleration: unified paths for GPU and NPU acceleration and automated accelerator selection in V2.
* Async and efficient I/O: true asynchronous execution and zero-copy buffer interoperability to reduce latency and improve throughput.
* Ecosystem compatibility: migration paths from TFLite and integrations with LiteRT-LM and ai-edge-torch tools.

## Use Cases

* Mobile real-time inference: run segmentation, detection, or speech models in Android/iOS apps with low latency.
* Embedded and edge devices: deploy optimized models where compute and power are limited.
* Generative model acceleration: support low-latency on-device inference for quantized or compact generative models.
* Performance tuning and hardware adaptation: serve as the runtime foundation when GPU/NPU acceleration is required.

## Technical Features

* Runtime architecture: modular design supporting multiple backends and custom delegates.
* Build & deployment: Docker and Bazel/CMake build guides for cross-compilation and artifact generation.
* Open-source license: Apache-2.0 licensed for enterprise and community adoption.
* Developer experience: sample applications and migration guides to ease transition from existing TFLite workflows.

Loading comments...
0

![LiteRT](https://opengraph.githubassets.com/1/google-ai-edge/LiteRT)

##### Resource Info

⏱️ Runtime
🔮 Inference
🌐 Edge
🌱 Open Source

##### Related Resources

###### [Agent Development Kit Web (ADK Web)](/ai/adk-web/)

Google's built-in developer UI for the Agent Development Kit, designed to …

Author：Google

###### [adk-go](/ai/adk-go/)

A code-first Go toolkit for building, evaluating, and deploying sophisticated AI …

Author：Google

###### [Coral NPU](/ai/coral-npu/)

Coral NPU is an energy-efficient machine learning accelerator core for edge …

Author：Google

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