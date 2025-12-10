---
title: How Fiber Network Works
url: http://catcoding.me/p/how-fiber-works/
source: CatCoding
date: 2025-12-09
fetch_date: 2025-12-10T03:26:06.000668
---

# How Fiber Network Works

[CatCoding](/)

* [Home](/)
* [Archives](/archives/)
* [Ideas](/ideas/)
* [Links](/links/)
* [Projects](/project/)
* [About](/about/)

* [Home](/)
* [Archives](/archives/)
* [Ideas](/ideas/)
* [Links](/links/)
* [Projects](/project/)
* [About](/about/)

## How Fiber Network Works

2025-12-09

I just got back from CKCon in beautiful Chiang Mai 🌴, where I gave a talk on the Fiber Network. To help everyone wrap their heads around how Fiber (CKB’s Lightning Network) actually moves assets, I hacked a visual simulation with AI.

To my surprise, people didn’t just understand it—they loved it! 🎉

Here is the “too long; didn’t read” version. But first, go ahead and play with the dots yourself, 👉 **Play the Simulation:** [fiber-simulation](https://fiber-world.vercel.app/simulate.html)

![](/images/ob_pasted-image-20251204113415.png)

We all love Layer 1 blockchains like Bitcoin or CKB for their security, but let’s be honest: they aren’t exactly built for speed.

Every transaction has to be shouted out to the entire world and written down by thousands of nodes. On CKB, you’re waiting about 8 seconds for a block; on Bitcoin, it’s 10 minutes! Plus, the fees can get nasty if you’re just trying to buy a coffee. ☕️

**So, how do we fix this?**

The Lightning Network is a **scalable, low-fee, and instant micro-payment solution for P2P payments**.

The secret sauce isn’t actually new. Even Satoshi Nakamoto hinted at this “high-frequency” magic in an [early email](https://en.bitcoin.it/wiki/Payment_channels):

> Intermediate transactions do not need to be broadcast. Only the final outcome gets recorded by the network.

A Lightning Network consists of **Peers** and **Channels**. A peer can send, receive, or forward a payment. A Channel is used for communication between two Peers.

![](/images/ob_pasted-image-20251204113629.png)

Imagine you and a friend want to trade money back and forth quickly:

1. **Opening the Channel:** You both put some money into a pot and sign a **Funding Tx**. This goes on the blockchain (L1).
2. **The Fun Part (Off-Chain):** Now that the channel is open, you can send money back and forth a million times instantly! You just update the balance sheet between you two (using HTLCs and signatures). No one else needs to know, and no blockchain fees are paid yet.
3. **Closing the Channel:** When you’re done, you agree on the final balance, sign a **Shutdown Tx**, and tell the blockchain.

Everything in the middle? That’s **off-chain** magic. ✨

Now, if Fiber was just about paying your direct neighbor, it would be boring. The real power comes from the **Network**.

![](/images/ob_pasted-image-20251204113608.png)

This means Alice can pay Bob even if they don’t have a direct channel between them. The payment can travel through one or more intermediate nodes. As long as there is a path with enough liquidity, the payment will reach its destination instantly.

All data is wrapped in **Onion Packets** (yes, like layers of an onion). The nodes in the middle serve as couriers, but they are blindfolded:

* They don’t know who sent the money.
* They don’t know who is receiving it.
* They only know “pass this to the next guy.”

They simply follow a basic rule: they forward the Hash Time Lock, and if the payment succeeds, they earn a tiny fee for their trouble. Easy peasy.

**The “Not So Easy” Part 😅**

While the idea is simple, building it is… well, an engineering adventure. We’re dealing with cryptography, heavy concurrency, routing algorithms, and a whole jungle of edge cases. But hey, that’s what makes it fun!

We’ve poured the last two years into building Fiber, and I’m proud to say it’s finally GA ready.

If you want to geek out on the details, check these out:

* [Mastering the Lightning Network (LN)](https://github.com/lnbook/lnbook) and [Basis of Lightning Technology](https://github.com/lightning/bolts)
* [nervosnetwork/fiber](https://github.com/nervosnetwork/fiber)

Here is the full presentation from my talk:
[CKB Fiber Network Engineering Updates](https://docs.google.com/presentation/d/1aKhG0ebR-UV4SqOWOd_Xjlz15hGVmmRUMLzIkwcUWeY/edit?usp=sharing)

![](/images/wechat-qr-code.jpg)

公号同步更新，欢迎关注👻

Tags:
[ckb](/tags#ckb)
[lightning-network](/tags#lightning-network)
[fiber](/tags#fiber)
[layer-2](/tags#layer-2)
[blockchain](/tags#blockchain)

[在开源中使用 LLM
 →](/p/llm-in-open-source/)

©
2025 | Proudly powered by [Hexo](https://hexo.io) with [Vexo](https://github.com/yanm1ng/hexo-theme-vexo)