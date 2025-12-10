---
title: Holy Shuck! Weaponizing NTLM Hashes as a Wordlist
url: https://trustedsec.com/blog/holy-shuck-weaponizing-ntlm-hashes-as-a-wordlist
source: TrustedSec
date: 2025-12-09
fetch_date: 2025-12-10T03:25:32.139388
---

# Holy Shuck! Weaponizing NTLM Hashes as a Wordlist

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
* [Holy Shuck! Weaponizing NTLM Hashes as a Wordlist](https://trustedsec.com/blog/holy-shuck-weaponizing-ntlm-hashes-as-a-wordlist)

December 09, 2025

# Holy Shuck! Weaponizing NTLM Hashes as a Wordlist

Written by
Austin Coontz

Active Directory Security Review
Penetration Testing

![](https://trusted-sec.transforms.svdcdn.com/production/images/Blog-Covers/HolyShuckWeaponizingNTLMHashes_WebHero.jpg?w=320&h=320&q=90&auto=format&fit=crop&dm=1764863065&s=32ef4a739fa896937c94a5c06ed35bc4)

Table of contents

* [What is Hash Shucking?](#What)
* [How This Applies to AD](#How)
* [To Shuck or Not To Shuck](#ToNot)
* [Hashcat Shucking Modes](#Modes)
* [Demo Time](#Demo)
* [Mitigations](#Mitigations)
* [Conclusion](#Conclusion)

Share

* Share URL
* [Share via Email](/cdn-cgi/l/email-protection#d1eea2a4b3bbb4b2a5ec92b9b4b2baf4e3e1bea4a5f4e3e1a5b9b8a2f4e3e1b0a3a5b8b2bdb4f4e3e1b7a3bebcf4e3e185a3a4a2a5b4b582b4b2f4e3e0f7b0bca1eab3beb5a8ec99bebda8f4e3e182b9a4b2baf4e3e0f4e3e186b4b0a1bebfb8abb8bfb6f4e3e19f859d9cf4e3e199b0a2b9b4a2f4e3e1b0a2f4e3e1b0f4e3e186bea3b5bdb8a2a5f4e290f4e3e1b9a5a5a1a2f4e290f4e397f4e397a5a3a4a2a5b4b5a2b4b2ffb2bebcf4e397b3bdbeb6f4e397b9bebda8fca2b9a4b2bafca6b4b0a1bebfb8abb8bfb6fcbfa5bdbcfcb9b0a2b9b4a2fcb0a2fcb0fca6bea3b5bdb8a2a5 "Share via Email")
* [Share on Facebook](http://www.facebook.com/sharer.php?u=https%3A%2F%2Ftrustedsec.com%2Fblog%2Fholy-shuck-weaponizing-ntlm-hashes-as-a-wordlist "Share on Facebook")
* [Share on X](http://twitter.com/share?text=Holy%20Shuck%21%20Weaponizing%20NTLM%20Hashes%20as%20a%20Wordlist%3A%20https%3A%2F%2Ftrustedsec.com%2Fblog%2Fholy-shuck-weaponizing-ntlm-hashes-as-a-wordlist "Share on X")
* [Share on LinkedIn](https://www.linkedin.com/shareArticle?url=https%3A%2F%2Ftrustedsec.com%2Fblog%2Fholy-shuck-weaponizing-ntlm-hashes-as-a-wordlist&mini=true "Share on LinkedIn")

Password reuse is common in Active Directory (AD). From an attacker’s perspective, it is a reliable path to lateral movement or privilege escalation. Most IT teams recognize the risk, but longer passwords and password managers encourage the belief that reusing a long password is safe. Once passwords reach high character lengths, recovering them is usually impractical for many hash types, and that reuse often slips by unnoticed.

Enter hash shucking. Instead of trying to recover plaintext passwords from slower algorithms like Kerberos tickets or cached credentials, we can use NTLM (NT) hashes as a wordlist in ***Hashcat***’***s*** NT Modes. This lets us quickly validate password reuse across NTLMv1 and NTLMv2 challenge-responses, Kerberos 5 etype 23 tickets, and DCC/DCC2 hashes. If a match is found, we can spend time more effectively by recovering the plaintext from the NT hash, or by using pass-the-hash (PtH). In this post, I describe hash shucking and its relevance to AD, outline the key ***Hashcat*** modes involved, demonstrate the technique with two examples, and close with practical mitigations to limit opportunities for hash shucking.

## What is Hash Shucking?

Say a company runs two sites. Site A is breached and exposes unsalted MD5 hashes:

```
md5(password)
```

Later, Site B upgrades the password storage by wrapping existing passwords with bcrypt. It is also breached, exposing:

```
bcrypt(md5(password))
```

With the MD5 hashes from Site A and the bcrypt-wrapped MD5 hashes from Site B, you can take the MD5 values from the Site A breach and supply them directly to bcrypt:

```
bcrypt(md5hash)
```

If the bcrypt check matches, we have confirmed the same password is reused without ever recovering the plaintext. This is hash shucking: the outer layer (bcrypt) is shucked off, and you can run password-recovery attacks against the inner hash (MD5) at much higher speeds. I recommend checking out [What the Shuck? Layered Hash Shucking](https://www.youtube.com/watch?v=OQD3qDYMyYQ) by Sam Croley for a more detailed explanation.

![](https://trusted-sec.transforms.svdcdn.com/production/images/Blog-assets/HolyShuck_Coontz/FigA_Coontz_HolyShuck.png?w=320&q=90&auto=format&fit=max&dm=1764775744&s=28fa7ed4b7da7a5c1430e85cd07c207c)

## How This Applies to AD

Passwords in AD are stored on domain controllers as NTLM hashes. The NT hash is computed as MD4 over the UTF-16LE encoding of the password.

```
MD4(UTF-16LE(password))
```

Kerberos supports multiple encryption types. When RC4-HMAC (etype 23) is allowed, TGS tickets can be requested that are encrypted with a key derived from the target account’s NT hash. Using any domain user, you can request a TGS for an account with an SPN, capture the resulting TGS-REP, extract the RC4-based TGS-REP ...