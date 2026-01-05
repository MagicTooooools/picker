---
title: Introduction to My Home Network
url: https://www.bboy.app/2026/01/04/introduction-to-my-home-network/
source: Bboysoul's Blog
date: 2026-01-04
fetch_date: 2026-01-05T03:55:25.549819
---

# Introduction to My Home Network

[![](/oss/head.webp)](/)

# [Bboysoul's Blog](/)

|  |  |  |  |
| --- | --- | --- | --- |
| [首页](/) | [关于我](/about.html) | [RSS订阅](/atom.xml) | 系列文章 ▾  * [我的生活](/tags/life/) * [项目更新](/tags/%E6%88%91%E5%85%B3%E6%B3%A8%E7%9A%84%E9%A1%B9%E7%9B%AE%E8%BF%99%E5%91%A8%E6%9B%B4%E6%96%B0%E4%BA%86%E4%BB%80%E4%B9%88/) * [胡言乱语](/tags/%E8%83%A1%E8%A8%80%E4%B9%B1%E8%AF%AD/) |

---

⬇️⬇️⬇️ 欢迎关注我的 telegram 频道和 twitter ⬇️⬇️⬇️

---

联系方式:
[Twitter](https://twitter.com/bboysoulcn)
[Github](https://github.com/bboysoulcn)
[Email](/cdn-cgi/l/email-protection#6b0909041218041e072b09090412450a1b1b)
[Telegram](https://t.me/bboyapp)

---

# Introduction to My Home Network

January 4, 2026
本文有 1220 个字
需要花费 3 分钟阅读

![](/oss/20260104-2.webp)

### Introduction

I used to love tinkering with various complex network configurations: multiple VLAN divisions, complex DNS splitting rules, layer 3 switches, various firewall policies, multiple wireless SSIDs… As a tech enthusiast, these configurations were indeed very fulfilling and could bring a “scientific and interference-free” internet experience.

However, when these complex configurations were applied to a home network, problems arose:

* The configuration was too complex, making it difficult to troubleshoot when issues occurred, affecting family internet usage
* Some devices might not be able to connect to the internet properly due to firewall rules
* Overly aggressive DNS splitting configuration caused some domestic apps to malfunction
* More devices meant more failure points and higher maintenance costs
* Family members just wanted to browse TikTok and shop on Pinduoduo, and didn’t need so many “advanced features”

After multiple attempts and reflections, I finally understood: **for home networks, stability and simplicity are far more important than rich features**. So, I started to simplify, building a minimalist yet adequate home network solution.

### Previous Related Blog Posts

* [简单的说下家里的网络](https://www.bboy.app/2020/07/27/%E7%AE%80%E5%8D%95%E7%9A%84%E8%AF%B4%E4%B8%8B%E5%AE%B6%E9%87%8C%E7%9A%84%E7%BD%91%E7%BB%9C/)
* [一个垃圾佬家里的网络](https://www.bboy.app/2020/03/09/%E4%B8%80%E4%B8%AA%E5%9E%83%E5%9C%BE%E4%BD%AC%E5%AE%B6%E9%87%8C%E7%9A%84%E7%BD%91%E7%BB%9C/)
* [最终买了一台华三的路由器](https://www.bboy.app/2023/02/27/%E6%9C%80%E7%BB%88%E4%B9%B0%E4%BA%86%E4%B8%80%E5%8F%B0%E5%8D%8E%E4%B8%89%E7%9A%84%E8%B7%AF%E7%94%B1%E5%99%A8/)

### Design Principles

When designing my home network, I followed two core principles:

* **Fewer devices, the better** - Reduce failure points and lower maintenance costs
* **Less configuration, the better** - Simplify management and improve reliability

### Network Topology

The entire home network topology is as follows:

```
China Unicom Fiber -> Optical Modem B610-4E (Bridge Mode)
                    |
            Wired Router RB5009UG+S+
            (Dial-up, DHCP, DNS, VPN)
                    |
          +---------+---------+
          |                   |
    Wireless Router      Switch MERCURY SG116M
    zte BE5100              |
    (Bridge Mode)      Wired devices in each room
```

### Equipment

* Optical Modem B610-4E
* Wired Router RB5009UG+S+
* Wireless Router zte BE5100
* Switch MERCURY SG116M
* UPS TG-BOX850
* Various Cat 6 network cables

### About the Optical Modem B610-4E

I use China Unicom broadband, and the optical modem is configured in bridge mode. The benefits of this are:

* Avoid network issues caused by double NAT
* Reduce one layer of forwarding, lowering latency
* Let the more powerful router handle dial-up and network management

After configuring bridge mode, the optical modem only handles optical-to-electrical conversion, and all routing functions are handled by the RB5009.

### Wired Router RB5009UG+S+

The RB5009UG+S+ is the core of the entire network. This is an enterprise-grade router from MikroTik running RouterOS. The reasons for choosing it are:

* Powerful performance, if it doesn’t break, you might never need to replace it
* RouterOS is feature-rich and highly customizable
* Stable and reliable, suitable for long-term operation

#### Main Function Configuration

**Dial-up**

As the core router, the RB5009 handles PPPoE dial-up connection to the ISP network. There’s nothing special about this, just standard configuration.

**DHCP Server**

I use the network segment `10.10.100.0/24`, with a DHCP address pool of `10.10.100.100-10.10.100.200`. Actually, I prefer a simpler network segment like `10.10.10.0/24`, but my previous company used that exact segment, so to avoid network segment conflicts when connecting via VPN, I changed to `10.10.100.0/24` and have been using it ever since.

The DNS address advertised by the DHCP server is `10.10.100.1`, which is the router itself, and all devices perform DNS resolution through the router.

**DNS Server**

RouterOS’s DNS functionality is quite powerful, eliminating the need for an additional DNS server. I’ve configured the following domestic DNS as upstream:

* 114.114.115.115
* 114.114.114.114
* 223.5.5.5
* 223.6.6.6
* 119.29.29.29

To improve DNS resolution speed and reduce queries to upstream servers, I set the Max Cache TTL to 1 day (86400 seconds). This way, frequently accessed domain resolution records are cached longer, greatly improving access speed.

**Back to Home VPN**

Since China Unicom broadband provides a public IPv4 address, I’ve configured RouterOS’s “Back to Home” VPN feature. This is a unique RouterOS feature that’s very simple to configure:

1. Enable the Back to Home VPN server on RouterOS
2. Use the RouterOS mobile app to scan a QR code to connect
3. No need for complex certificate configuration or port forwarding

When I’m out and need to access my home NAS, surveillance, or other intranet services, I just need to enable VPN on my phone to securely connect back to my home network. Compared to traditional OpenVPN or WireGuard, Back to Home is much simpler to configure.

### About the Wireless Router zte BE5100

This wireless router is configured in AP (bridge) mode, only responsible for providing wireless network coverage, with all routing functions handled by the RB5009. I chose the BE5100 mainly because:

* Supports WiFi 7, faster speeds
* Good signal coverage, basically covers the entire home
* ZTE offers good value for money
* Very stable in bridge mode

The advantage of this configuration is more centralized network management, all devices are on the same network segment, no need to deal with multi-level NAT issues, and policy configurations are all managed uniformly on the RB5009.

### About the Switch MERCURY SG116M

Since the RB5009 has a limited number of ports, and every room in my home has pre-wired network outlets, I added a 16-port gigabit switch.

MERCURY SG116M is a very cost-effective unmanaged switch:

* 16 gigabit RJ45 ports, meeting the connection needs of all wired devices at home
* Plug and play, no configuration needed
* Low power consumption, low heat generation
* Affordable price, just over a hundred yuan (now it’s basically a steal at current prices)

All room network cables converge to this switch in the weak current box, then connect to the RB5009 through a single network cable.

### About Network Cables

All network cables in my home use Cat 6 (Category 6) cables. Although Cat 5e would be sufficient for gigabit networks, considering:

* The price difference between Cat 6 and Cat 5e is minimal
* Cat 6 cables have more stable performance and better anti-interference capability
* Less signal attenuation, better transmission quality
* Reserve space for possible future 2.5G/5G/10G upgrades (though unlikely)

So I went directly with Cat 6 cables. However, for my current usage scenario, gigabit network is completely sufficient:

* NAS file transfer speed within the LAN can max out gigabit (about 110MB/s)
* 300M broadband download speed is about 62.5MB/s, far from the gigabit bottleneck
* No network latency is felt in daily use

### About UPS TG-BOX850

To prevent sudden power outages from dam...