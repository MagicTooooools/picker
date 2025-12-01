---
title: Dia2 - An open-source streaming dialogue text-to-speech (TTS) model and inference …
url: https://jimmysong.io/ai/dia2/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-11-30
fetch_date: 2025-12-01T03:39:14.193113
---

# Dia2 - An open-source streaming dialogue text-to-speech (TTS) model and inference …

ArkSphere Community Launched. AI-native runtime. Infra. OSS.
[Learn more](https://arksphere.dev/).

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
  中文](/zh/ai/dia2/)

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
Dia2

# Dia2

An open-source streaming dialogue text-to-speech (TTS) model and inference codebase for real-time conversational audio generation.

Author:
Nari Labs

Since:
2025-11-17

[Visit Website](https://huggingface.co/nari-labs/Dia2-2B) [GitHub](https://github.com/nari-labs/dia2)

## Detailed Introduction

Dia2 is an open-source text-to-speech (TTS) model and inference implementation from Nari Labs focused on streaming conversational audio. The model can begin generating audio after receiving the initial input tokens and supports conditioning on audio prefixes to maintain speaker consistency and contextual continuity in multi-turn interactions. The repository provides 1B and 2B model checkpoints, example scripts, and quickstart instructions for research and deployment.

## Main Features

* Streaming generation: starts synthesis without waiting for the full text, reducing response latency.
* Conditional generation: supports audio-prefix conditioning for speaker consistency and smoother conversation flow.
* Multiple scales: model checkpoints at different sizes (1B, 2B) to balance quality and resource use.
* Open license: released under Apache-2.0 for research and non-proprietary use.

## Use Cases

* Real-time voice for conversational assistants and virtual characters, improving naturalness and responsiveness.
* Reply generation in voice-based dialog systems with multi-turn context handling.
* Research and teaching for TTS conditional generation, model comparison, and voice control experiments.

## Technical Features

* Inference implementation based on Python and the `uv` runtime, compatible with Hugging Face checkpoints and CUDA acceleration (recommended CUDA 12.8+).
* Generation length is limited by context steps (around 2 minutes); outputs include audio tokens, waveform, and timestamps.
* Command-line examples and a Gradio demo are provided for quick verification and integration.

Loading comments...
0

![Dia2](https://opengraph.githubassets.com/1/nari-labs/dia2)

##### Resource Info

🌱 Open Source
🗣️ Text to Speech
🔊 Audio
🔮 Inference

##### Related Resources

###### [Awesome Nano Banana Pro](/ai/awesome-nanobanana-pro/)

A curated, open-source collection of high-quality prompts and examples for the …

Author：ZeroLu

###### [Free LLM API resources](/ai/free-llm-api-resources/)

A community-maintained list of LLM providers and gateways offering free or trial …

Author：cheahjs

###### [Pixeltable](/ai/pixeltable/)

A declarative data infrastructure for multimodal AI workloads that simplifies …

Author：Pixeltable

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