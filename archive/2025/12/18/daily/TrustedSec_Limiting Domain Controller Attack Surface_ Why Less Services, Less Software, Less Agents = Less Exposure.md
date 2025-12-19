---
title: Limiting Domain Controller Attack Surface: Why Less Services, Less Software, Less Agents = Less Exposure
url: https://trustedsec.com/blog/limiting-domain-controller-attack-surface-why-less-services-less-software-less-agents-less-exposure
source: TrustedSec
date: 2025-12-18
fetch_date: 2025-12-19T03:24:51.580161
---

# Limiting Domain Controller Attack Surface: Why Less Services, Less Software, Less Agents = Less Exposure

[Skip to Main Content](#main)

All Trimarc services are now delivered through TrustedSec!
[Learn more](https://trustedsec.com/about-us/news/trimarc-joins-forces-with-trustedsec-to-strengthen-security-advisory-services)

Close

[TrustedSec](https://trustedsec.com/)

* [Solutions](https://trustedsec.com/solutions)

  ## Solutions

  Our custom solutions are tailored to address the unique challenges of different roles in security.

  [Solutions](https://trustedsec.com/solutions)

  + [01

    For Leadership

    We understand the challenges facing modern executives and develop solutions unique to leaders.](https://trustedsec.com/solutions/for-leadership)
  + [02

    For Operations

    We stay one step ahead to proactively safeguard our clients and partners.](https://trustedsec.com/solutions/for-operations)
  + [03

    For Infrastructure

    From architecture to resiliency and maintainability, we keep your tech aligned to best practices.](https://trustedsec.com/solutions/for-infrastructure)
  + [04

    For Assurance

    Our compliance experts guide partners through regulatory requirements to ensure standards are met.](https://trustedsec.com/solutions/for-assurance)
* [Services](https://trustedsec.com/services)

  ## Services

  From building to testing to hardening, our services support security at every stage.

  [Services](https://trustedsec.com/services)

  + [01

    Design

    Design an exceptional, custom security program alongside our security experts.](https://trustedsec.com/services/design)
  + [02

    Evaluate

    Evaluate your security program with proven assessment methodologies.](https://trustedsec.com/services/evaluate)
  + [03

    Harden

    Harden your security program with the help of our security experts.](https://trustedsec.com/services/harden)
  + [04

    Respond

    Respond to threats to your security program with the help of our security experts.](https://trustedsec.com/services/respond)
* [Research](https://trustedsec.com/research)
* [Blog](https://trustedsec.com/blog)
* [Resources](https://trustedsec.com/resources)
* [About Us](https://trustedsec.com/about-us)

  ## About Us

  Driven by purpose, fueled by experts.

  [About Us](https://trustedsec.com/about-us)

  + [01

    Our Team

    Meet our security experts.](https://trustedsec.com/about-us/our-team)
  + [02

    Our Partners

    Become a TrustedSec partner to help your customers anticipate and prepare for potential attacks.](https://trustedsec.com/about-us/our-partners)
  + [03

    News

    Our team is trusted by local and national media to be the subject matter experts for security news.](https://trustedsec.com/about-us/news)
  + [04

    Events

    See our upcoming webinars, conferences, talks, trainings, and more!](https://trustedsec.com/about-us/events)

Search

Menu

Search Input

Search

* [Contact Us](https://trustedsec.com/contact)
* [Report a breach](https://trustedsec.com/report-a-breach)

* [Solutions](https://trustedsec.com/solutions)
* [Services](https://trustedsec.com/services)
* [Research](https://trustedsec.com/research)
* [Blog](https://trustedsec.com/blog)
* [Resources](https://trustedsec.com/resources)
* [About Us](https://trustedsec.com/about-us)

Search

* [Contact Us](https://trustedsec.com/contact)
* [Report a breach](https://trustedsec.com/report-a-breach)

* [Blog](https://trustedsec.com/blog)
* [Limiting Domain Controller Attack Surface: Why Less Services, Less Software, Less Agents = Less Exposure](https://trustedsec.com/blog/limiting-domain-controller-attack-surface-why-less-services-less-software-less-agents-less-exposure)

December 18, 2025

# Limiting Domain Controller Attack Surface: Why Less Services, Less Software, Less Agents = Less Exposure

Written by
Scott Blake

Active Directory Security Review
Risk Assessment
Information Security Compliance
Trimarc Legacy Blog

![](https://trusted-sec.transforms.svdcdn.com/production/images/Blog-Covers/LimitedDomainControllerAttackSurface_WebHero.jpg?w=320&h=320&q=90&auto=format&fit=crop&dm=1765571181&s=8039bcf7019ad50fb649d63f80c3d4b5)

Table of contents

* [Approval Process](#Approval)
* [Risk/Risk Acceptance](#Risk/Risk)
* [Summary](#Summary)

Share

* Share URL
* [Share via Email](/cdn-cgi/l/email-protection#aa95d9dfc8c0cfc9de97e9c2cfc9c18f989ac5dfde8f989adec2c3d98f989acbd8dec3c9c6cf8f989accd8c5c78f989afed8dfd9decfcef9cfc98f989b8ccbc7da91c8c5ced397e6c3c7c3dec3c4cd8f989aeec5c7cbc3c48f989ae9c5c4ded8c5c6c6cfd88f989aebdedecbc9c18f989af9dfd8cccbc9cf8f99eb8f989afdc2d38f989ae6cfd9d98f989af9cfd8dcc3c9cfd98f98e98f989ae6cfd9d98f989af9c5ccdeddcbd8cf8f98e98f989ae6cfd9d98f989aebcdcfc4ded98f989a8f99ee8f989ae6cfd9d98f989aefd2dac5d9dfd8cf8f99eb8f989ac2dededad98f99eb8f98ec8f98ecded8dfd9decfced9cfc984c9c5c78f98ecc8c6c5cd8f98ecc6c3c7c3dec3c4cd87cec5c7cbc3c487c9c5c4ded8c5c6c6cfd887cbdedecbc9c187d9dfd8cccbc9cf87ddc2d387c6cfd9d987d9cfd8dcc3c9cfd987c6cfd9d987d9c5ccdeddcbd8cf87c6cfd9d987cbcdcfc4ded987c6cfd9d987cfd2dac5d9dfd8cf "Share via Email")
* [Share on Facebook](http://www.facebook.com/sharer.php?u=https%3A%2F%2Ftrustedsec.com%2Fblog%2Flimiting-domain-controller-attack-surface-why-less-services-less-software-less-agents-less-exposure "Share on Facebook")
* [Share on X](http://twitter.com/share?text=Limiting%20Domain%20Controller%20Attack%20Surface%3A%20Why%20Less%20Services%2C%20Less%20Software%2C%20Less%20Agents%20%3D%20Less%20Exposure%3A%20https%3A%2F%2Ftrustedsec.com%2Fblog%2Flimiting-domain-controller-attack-surface-why-less-services-less-software-less-agents-less-exposure "Share on X")
* [Share on LinkedIn](https://www.linkedin.com/shareArticle?url=https%3A%2F%2Ftrustedsec.com%2Fblog%2Flimiting-domain-controller-attack-surface-why-less-services-less-software-less-agents-less-exposure&mini=true "Share on LinkedIn")

Before we dive in, let’s get all the TrustedSec Certified Absolutes out of the way:

1. **All software presents some level of inherent risk.**
2. **Only required software that cannot live on other systems should be installed on Domain Controllers (DCs).**
3. **DCs should have a separate, detailed approval process for software/services/agents.**

Each organization’s needs and risk appetite will be different, but it is important to highlight that having less 'stuff' on DCs can lower the security risk to these critical systems by minimizing their attack surface.

At TrustedSec, we have assessed numerous Active Directory (AD) forests when performing our [**Active Directory Security Assessment**](https://trustedsec.com/services/active-directory-security-assessment). Too often we discover questionable software running on DCs or the reluctance to acknowledge that the Patch Management team is effectively Tier 0 with their elevated rights through the patching agent. Hence, this blog is intended to be a guide for making the best-informed decisions through awareness of potential issues and ensuring organizations are asking the right questions.

There is no debate that securing DCs, and therefore AD, is of the utmost importance. These Windows servers host AD Domain Services (AD DS) and control who can authenticate to and access resources within an organization. While there are best-practice configurations for items like ***User Rights Assignments*** and ***Security Options***, I wanted to highlight and emphasize how software allowed to run on a DC can be just as important (or detrimental!) to the security of AD.

These installations often introduce additional exposure that may get overlooked during deployment and are typically forgotten about for the full life cycle of the DC. For example, a single agent running on all DCs can perform its own Denial of Service (DoS) against AD; it’s happened before and will happen again.

**Note:** While this blog is directed at DCs, it is also applicable to other Tier 0 systems (Entra Connect, AD Certificate Services (AD CS), etc.) to a somewhat lesser degree.

#### *Common types of agents/software on DCs include:*

* Anti-Virus (AV)
* Endpoint Detection and...