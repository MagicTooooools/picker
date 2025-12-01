---
title: Secure By Design roundup - November 2025
url: https://shostack.org/blog/appsec-roundup-nov-2025/
source: Shostack & Friends Blog
date: 2025-11-30
fetch_date: 2025-12-01T03:38:34.492011
---

# Secure By Design roundup - November 2025

[Skip to main content](#main-content)

[![Shostack and Associates logo, click for Homepage](/img/Shostack-logo-white.png)](/)

* [About](/about/)
  + [Shostack + Associates](/about/)
  + [Adam Shostack](/about/adam/)
* [Services](/training/)
  + [Training](/training/)
  + [Accelerator](/secure-design-accelerator/)
  + [Expert Witness](/expert-witness/)
  + [Consulting](/consulting/)
* [Resources](/resources/)
  + [Overview](/resources/)
  + [Threat Modeling](/resources/threat-modeling/)
  + [Books](/books/)
  + [Games](/tm-games/)
  + [Cyber Public Health](/resources/cyber-public-health/)
  + [Lessons Learned](/resources/lessons/)
  + [Videos](/resources/videos/)
  + [Whitepapers](/resources/whitepapers/)
* [Blog](/blog/)
* [Contact](/contact/)

1. [Shostack + Associates](/)
2. [Blog](/blog/)
3. Secure By Design roundup - November 2025

Shostack + Friends Blog

# Secure By Design roundup - November 2025

Perspective on CISOs as facilitators, a deep dive into the types of diagrams for medical devices, poetry, Chinese LLMs, Chinese drones and Chinese routers. Do any of them contain secrets?
![a photograph of a robot, sitting in a library, working on a jigsaw puzzle. The robot holds up the jigsaw puzzle, and snow is falling inside the library](/images/blog/img/2025/appsec-roundup-winter-1000w.png)

This month, we lead off with Melanie Ensign’s [CISO as Super-Facilitator:
Elevating Board and C-Suite Security Leadership](https://discernibleinc.com/blog/ciso-as-super-facilitator-elevating-board-c-suite-security-leadership). Like last
month’s series by Phil Venables, this is nominally targeted at
the CISO. But a secure by design program or a threat modeling
program involves both technical improvements, like rolling out
SAST, and culture changes, getting engineers excited about,
empowered for, and then responsible for security what they develop
or deploy.

### Threat Modeling

* Chris Espinosa has a great blog post going in depth into [Security
  Architecture Views for Medical Device Cybersecurity](https://bluegoatcyber.com/blog/examining-the-fdas-recommended-security-architecture-views-for-medical-device-security/). The video
  goes into some interesting lessons from real submissions.

### Appsec

* Cat Hicks has an interview with Dr. Ariana Mirian, [Can
  we make Security Empirical, and why might we want to?](https://www.fightforthehuman.com/empirical-security/) In the
  extensive writeup, there’s lots about the psychology of security
  champions programs.
* The Rust re-write of Sudo has [a
  set of vulnerabilities](https://www.phoronix.com/news/sudo-rs-security-ubuntu-25.10). Re-writing large codebases in rust
  isn’t free, and we should be clear-eyed about what we get.
* The team at Trail of Bits blogs that [Constant-time support lands in LLVM: Protecting cryptographic code at the compiler level](https://blog.trailofbits.com/2025/11/25/constant-time-support-lands-in-llvm-protecting-cryptographic-code-at-the-compiler-level/).
* As app stores proliferate, understanding how they work is of
  increasing importance to a growing number of appsec programs. Google has been [pushing](https://android-developers.googleblog.com/2025/09/lets-talk-security-answering-your-top.html) for
  developer “verification,” to “deters bad actors and makes it
  harder for them to spread harm.” They’ve [pulled
  back](https://www.bleepingcomputer.com/news/google/google-backpedals-on-new-android-developer-registration-rules/) after a predictable backlash. It’s not clear to me how
  anyone expects this to deter criminal enterprises with access to
  large collections of stolen ID information.

### AI

* ”Funny idea! Poetry
  Discombobulates AI
  Jailbreaks in fall mist”
  You could puzzle that out, or you could read the longer [Adversarial Poetry as a Universal Single-Turn
  Jailbreak Mechanism in Large Language Models](https://arxiv.org/abs/2511.15304v1).
* In widely reported news, [CrowdStrike claims trigger
  words that lead DeepSeek to produce more vulnerable
  code](https://www.crowdstrike.com/en-us/blog/crowdstrike-researchers-identify-hidden-vulnerabilities-ai-coded-software/). I am somewhat skeptical, this would be exceptional
  tuning if possible, and it implies the possible existence of
  trigger words to create more secure code. They don’t clearly describe how they judge vulnerable
  code, other than to say they have an LLM judge with a secret
  prompt. (This is an exceptionally hard problem, a 90% accurate
  insecure code detector would be a hugely important discovery.)
  The prompt isn’t provided, nor is the identity of the “LLM-based
  judge,” a phrase that’s longer than “gemini 3 70b.” Also, after explaining that they’re not identifying the
  western models “for reasons of space”, they use the phrase “CrowdStrike
  Counter Adversary Operations” repeatedly where “we” would do fine.
  (Additional [discussion at lawfare](https://www.lawfaremedia.org/article/deepseek-and-musk%27s-grok-both-toe-the-party-line).)

### Regulation

* There seems to be a lot of “ban Chinese products” for
  “security reasons” floating around these days, and there seems to
  be a lack of clear criteria for such bans. Examples include [Florida’s DJI Drone Ban: A $200
  Million Disaster With No Evidence](https://dronexl.co/2025/11/22/florida-dji-drone-ban-200-million-disaster/). Or [TP Link](https://www.washingtonpost.com/technology/2025/10/30/tp-link-proposed-ban-commerce-department/), ban where, according
  to the Washington Post,
  “TP-Link Systems
  has repeatedly sought Commerce’s input as to where the government
  believes there could be residual concerns. Commerce has so far not
  responded to TP-Link’s outreach in that regard.” If “the
  government has secret evidence of vulns,” I have two points:
  first, in the United States, we have a vulnerabilities equities
  process that’s generally supposed to funnel that information to
  manufacturers so they can fix their products and make the world
  more secure. Second, if another country were doing this to the
  United States, we’d call them non-tariff trade barriers..

### Games Received

![The spot the secret game](/images/blog/img/2025/spot-the-secret-400w.jpeg)

* At OWASP, GitGuardian was giving out [Spot the Secret](https://labs.gitguardian.com/spot-the-secrets), a fast
  paced game with a delightful mechanism: a UV flashlight to reveal
  secrets printed on the card. (Flashlight included.)

### Shostack + Associates News

* We’re getting ready for our sold-out training event with MxD. If
  you missed it, let them know you you’d like more!
* ”Publish your Threat Models” will be published at [“Cybersecurity
  in Healthcare” (HealthSec) 2025](https://publish.illinois.edu/healthsec2025/), part of [ACSAC](https://www.acsac.org/2025/).

Image by midjourney: ”a photograph of a robot, sitting in a library,
working on a jigsaw puzzle. The robot is spotlighted by light
streaming in through a small window, through which you can it's
snowing.” I appreciate how this one is holding up the jigsaw and
it’s snowing inside, both demonstrating AI is bad at concepts.

Originally published by Adam on 30 Nov 2025

Categories:
  [application security](/blog/category/application-security)
  [software engineering](/blog/category/software-engineering)
  [security](/blog/category/security)

## Our Favorite Content

[General threat modeling posts](/blog/category/threat-modeling/)

[The Security Principles of Saltzer and Schroeder, illustrated with Star Wars](/blog/the-security-principles-of-saltzer-and-schroeder/)

[Other Star Wars blog posts](/blog/category/star-wars/)

[Modeling attackers and their motives](/blog/modeling-attackers-and-their-motives/)

[Doing science with near misses](/blog/doing-science-with-near-misses/)

[Posts about Adam’s “Threats” book](/blog/category/threats-book/)

[Posts about Adam’s “Threat Modeling” book](/blog/category/threat-modeling-book/)

[Posts about “The New School of Information Security” book](/blog/category/the-new-school/)

[...