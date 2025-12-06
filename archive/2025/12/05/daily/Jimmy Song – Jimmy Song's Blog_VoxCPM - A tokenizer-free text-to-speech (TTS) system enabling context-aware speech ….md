---
title: VoxCPM - A tokenizer-free text-to-speech (TTS) system enabling context-aware speech …
url: https://jimmysong.io/ai/vox-cpm/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-05
fetch_date: 2025-12-06T03:13:09.316634
---

# VoxCPM - A tokenizer-free text-to-speech (TTS) system enabling context-aware speech …

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
  中文](/zh/ai/vox-cpm/)

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
VoxCPM

# VoxCPM

A tokenizer-free text-to-speech (TTS) system enabling context-aware speech generation and true-to-life zero-shot voice cloning.

Author:
OpenBMB

Since:
2025-09-16

[Visit Website](https://openbmb.github.io/VoxCPM-demopage) [GitHub](https://github.com/OpenBMB/VoxCPM)

## Detailed Introduction

VoxCPM is an open-source tokenizer-free text-to-speech (TTS) system released by OpenBMB. It models speech in a continuous acoustic space to enable context-aware, highly expressive synthesis and supports zero-shot voice cloning from short reference audio. Built on a MiniCPM-4 backbone, VoxCPM provides training and inference pipelines, pretrained weights, and an interactive demo on Hugging Face for quick evaluation.

## Main Features

* Context-aware expressiveness: generates prosody and speaking style that match the semantic content by modeling continuous acoustic representations.
* True-to-life voice cloning: accurate zero-shot cloning capturing timbre, prosody, and fine-grained characteristics from brief reference audio.
* Efficient inference: engineering optimizations enable streaming synthesis with low real-time factor (RTF) on consumer GPUs.
* Open-source release: code, checkpoints and examples published under Apache-2.0 license on GitHub and Hugging Face.

## Use Cases

VoxCPM is suitable for high-fidelity and context-sensitive synthesis tasks such as voice assistants, media dubbing, linguistic research, and prototyping TTS for low-resource languages. Researchers can experiment with novel synthesis techniques, and engineers can quickly integrate pretrained models and demo services.

## Technical Features

VoxCPM employs tokenizer-free continuous acoustic modeling combined with hierarchical language modeling and FSQ constraints to decouple semantics and acoustics. The system uses a diffusion autoregressive pipeline built on MiniCPM-4, and provides training recipes, inference interfaces, and example scripts; demo and model downloads are available via the project page and Hugging Face demo:
[Demo](https://huggingface.co/spaces/OpenBMB/VoxCPM-Demo)and
[ArXiv report](https://arxiv.org/abs/2509.24650).

Loading comments...
0

![VoxCPM](https://opengraph.githubassets.com/1/OpenBMB/VoxCPM)

##### Resource Info

🗣️ Text to Speech
🌱 Open Source
🔊 Audio

##### Related Resources

###### [MiniCPM-V](/ai/mini-cpm-v/)

MiniCPM-V is a family of efficient end-side multimodal large models that support …

Author：OpenBMB

###### [ChatDev](/ai/chatdev/)

A multi-agent collaboration framework powered by large language models to turn …

Author：OpenBMB

###### [IMS Toucan](/ai/ims-toucan/)

A controllable and fast text-to-speech (TTS) toolkit supporting over 7000 …

Author：Institute for Natural Language Processing, University of Stuttgart

Navigation

* [Advanced Search](/search/)
* [Slides Share](/slide/)
* [Blog Post Tags](/tags/)
* [AI Resources](/ai/)

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

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribution required.

© 2017-2025 Jimmy Song All Right Reserved