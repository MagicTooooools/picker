---
title: UCG Fiber 主路由器 + OpenWrt 透明代理 + 高可用 方案和评测（对比ROS）
url: https://wusiyu.me/ucg-fiber-openwrt-proxy-failover-solution-and-review-compare-routeros/
source: WuSiYu Blog
date: 2026-01-14
fetch_date: 2026-01-15T03:34:53.333545
---

# UCG Fiber 主路由器 + OpenWrt 透明代理 + 高可用 方案和评测（对比ROS）

[跳至内容](#content)

[WuSiYu Blog](https://wusiyu.me/)

IT相关各种折腾

菜单
关闭

* [首页](http://wusiyu.me/)
* [HPC & ML sys](https://wusiyu.me/category/hpc-ml-sys/)
* [Linux & homelab](https://wusiyu.me/category/linux/)
* [智能硬件 & IOT](https://wusiyu.me/category/iot/)
* [Web 相关](https://wusiyu.me/category/web/)

# UCG Fiber 主路由器 + OpenWrt 透明代理 + 高可用 方案和评测（对比ROS）

发布日期：2026年 1月 15日分类：[Linux & homelab](https://wusiyu.me/category/linux/)
[UCG Fiber 主路由器 + OpenWrt 透明代理 + 高可用 方案和评测（对比ROS）无评论](https://wusiyu.me/ucg-fiber-openwrt-proxy-failover-solution-and-review-compare-routeros/#respond)

![](https://wusiyu.me/wp-content/uploads/2026/01/下载.jpg)

## 背景和需求

直连流量尽量通过主路由直连，同时内网所有设备透明代理。为了代理性能和方便管理，OpenWrt 可以跑在再服务器的虚拟机中，因此有必要在其离线时实现自动 failover，将内网降级为正常全直连网络。

过去用 RouterOS (ROS) 主路由（CCR2004） + OpenWrt 网关 + VRRP 实现，不过 ROS 的问题是设备管理不直观，不像 Unifi 的系统一样有着很好的大屏，以及 DPI、IDS/IPS 等功能。同时 ROS 没有 PPPoE 硬件 offload 支持，在这一点上甚至不如几百块的家用无线路由器。

因此我购入了一台 Unifi Cloud Gateway Fiber（UCG Fiber），三个10G口（2光1电）+ 4个2.5G电口，本文将探讨将其代替 ROS 作为高可用魔法（代理）家用网络环境的主路由器的方案和局限性。

## TLDR：Unifi与ROS优缺点对比

（这里的优缺点仅针对本场景）

**Unifi UCG Fiber**
优点：

* 界面美观，对客户端识别更智能（比如常见手机型号），DPI 功能还能分析流量种类和记录客户端的网络活动历史
* PPPoE hardware offload 支持，千兆宽带满速下载时 CPU 占用没什么变化，据其他人测试能完全跑满 10G PPPoE
* 小体积、高颜值，非常静音的主动散热，还有个小屏幕能看实时网速

缺点：

* 灵活性很低，不支持 VRRP（就算是Unifi的机架式设备也只能自己跟自己组HA），高可用代理方案配置麻烦，目前没找到可用的方法实现高可用的IPv6代理，只能强制让代理都走 IPv4
* 自带功能有些很不完善：
  + IPv6 支持烂，任何情况下 IPv6 只会走主要 WAN，哪怕 IPv6 地址下发的都是备用 WAN 的 v6，且主要 WAN 都没启用 v6
  + 策略路由（PBR）不支持 IPv6
  + 策略路由只能指定出口 WAN，没法指定下一跳到内网机器。而且 Fallback 功能残废，只有在 WAN 接口 Down 了时才会生效，哪怕系统都检测到该 WAN 不可用了也不会切换
  + Wireguard Server + 双 WAN 支持差，无法在备用 WAN 上接受连接（回程路由有 bug），除非主要 WAN 不可用
* 其他一些问题：端口转发的 NAT Loopback（在内网通过映射到公网的地址访问内部服务时）无法硬件加速，只能跑到1～3GBps，跑不满内网带宽
* 开启 Smart Queue QoS 后 PPPoE offload 失效，只能跑到 600Mbps 不到，且无法只在一个方向上开启（比如只在上行开启 QoS）
* 客户端管理功能很强，但没有一个地方能看当前所有的网络连接（Conntrack）和实时速度（类似ROS的IP -> Firewall -> Connections)

**RouterOS**

* 优点：灵活性很高，可以直接跟 OpenWrt 组VRRP，且 VRRP 支持脚本，ROS 的脚本可以便利的修改所有网络设置，可以实现各种自动化功能
* 缺点1：对于管理客户端流量不直观，Connections 里虽然能看到一个内网 IP 连了一个外网 IP，但还得去 DHCP 分配里查这个内网 IP 的主机名或MAC地址，可能还得上网查一下这个 MAC 地址的 vendor，才能确定这个客户端是谁；对于外网 IP 也没法跟 DNS 查询记录里的域名做关联，很不直观。
* 缺点2：MikroTik 家的硬路由性能有限，在我的 CCR2004 上 PPPoE 硬跑倒是能跑满千兆宽带下行，但一开双向的 Cake QoS 就不行了（哪怕开了fasttrack），不过可以只开上行的 QoS，下行带宽够大也不需要 QoS 了

## 方案概述

![](https://wusiyu.me/wp-content/uploads/2026/01/ucgop-%E7%AC%AC-2-%E9%A1%B5-1-scaled.png)

为实现 Unifi 主路由 + OpenWrt 全局透明代理 + 高可用，首先 Unifi 作为主路由器，连接光猫进行 PPPoE 拨号，OpenWrt 连接到 Unifi 的 LAN 上作为客户端（类似二级路由，但不同的是终端设备不去连接 OpenWrt），然后利用 Unifi 的双 WAN 主备切换的功能，让 OpenWrt 再作为 Unifi 的一个 WAN，即OpenWrt WAN Loopback，具体如下：

* 将 Unifi 的 WAN1 （使用DHCP，IPv6关闭）连接 OpenWrt 的 LAN ，OpenWrt 的 WAN 再连接到 Unifi 的 LAN
* 将 Unifi 的 WAN2 接光猫拨号，开启IPv6，并把内网的 IPv6 前缀来源也设置为 WAN2
* Unifi 上设置 WAN1（OpenWrt）为 Primary，WAN2（PPPoE）为 Backup，模式为Failover Only
* Unifi 上设置策略路由：
  + 1. 目标 Region 为 China 的都走 WAN2（不用勾 Kill Switch，没啥用且会跟下面的DNS hijack有奇妙的冲突）；
  + 2. 来自 OpenWrt 的流量都走 WAN2（避免循环，勾上Kill Switch）
* OpenWrt 上和标准的路由+代理设置基本相同，WAN 为 DHCP，连接 Unifi 内网。然后要将 WAN 和 WAN6 接口高级设置中的“自动获取 DNS 服务器”禁用，并填入运营商的 DNS 地址，避免循环
* **效果**：正常情况下，客户端的 DNS 请求先到 Unifi 的 DNS 服务器上（对于 DPI 功能有帮助），然后走 WAN1 -> OpenWrt 的无污染 DNS，解析得到国外地址继续走 OpenWrt 做代理（OpenWrt 再通过 Unifi LAN -> PPPoE 连接代理服务器），国内地址直接走 PPPoE 发出。一旦 OpenWrt 挂掉，Unifi 检测到 WAN1 不通，会自动把全部路由和 DNS 都切换到 PPPoE，实现自动降级直连
* （可选）此时 Unifi 上可以再设置一个 DNS hijack，让不遵守 DHCP 下发的 DNS 服务器的设备的所有 UDP 53 端口流量都强制发到 Unifi DNS 服务器：增加 DNAT 规则，匹配来源 IP 非 OpenWrt，目标 IP 非 Unifi，目标 UDP 53，将目标 IP 改写为 Unifi 的 IP

### IPv6 与 WireGuard Server 问题

按上述方案配置后，你会发现两个问题：

1. **IPv6 不通**：哪怕客户端已经具有了来自 PPPoE 的 v6 地址，但 v6 网络就是不通。这里来到了 Unifi 最逆天的地方了：除非主 WAN 断开，不然在任何情况下都试图让 IPv6 走主 WAN，哪怕主 WAN 都没配 IPv6，导致网络不通。
2. **Unifi 上运行的 WireGuard Server 不通**：哪怕 WireGuard Server 上设置了监听的是 WAN2 （PPPoE），只要主 WAN 还在，所有回包都会走主 WAN，导致外界的客户端连不上。

这两个问题都是 Unifi 的双 WAN 功能不完善导致的，必须 SSH 进去并设置自启脚本来修正：

编辑 `/data/internet_as_backup_wan_fix.sh` ，内容为（按需调整IFACE和WG\_PORT）：

```
#!/bin/sh

# ================= Configuration =================
# backup WAN Interface (real Internet, e.g. PPPoE to ISP)
IFACE="ppp1"
# Routing Table ID for backup WAN (Usually 202 for WAN2)
TABLE="202"
# WireGuard Server Listen Port
WG_PORT="16384"
# Log Tag for syslog
LOG_TAG="UniFi_Network_Fixer"
# =================================================

# Function to clean up rules on exit or restart
cleanup() {
    # Remove IPv6 default route
    ip -6 route del default dev "$IFACE" metric 1 2>/dev/null

    # Remove Policy Routing rule based on Source Port
    ip rule del sport "$WG_PORT" lookup "$TABLE" 2>/dev/null

    # Remove Force-SNAT rule from NAT table
    iptables -t nat -D POSTROUTING -o "$IFACE" -p udp --sport "$WG_PORT" -j MASQUERADE 2>/dev/null
}

# Run cleanup on start to ensure a clean slate
cleanup

logger -t "$LOG_TAG" "Starting network fix script..."

while true; do
    # Check if the interface exists
    if ip link show "$IFACE" > /dev/null 2>&1; then

        # -----------------------------------------------------
        # Task 1: Fix IPv6 Direct Access
        # Problem: Unifi doesn't add a default IPv6 route for Secondary WAN.
        # Fix: Manually add a default route to Main Table via ppp1.
        # -----------------------------------------------------
        HAS_V6_ROUTE=$(ip -6 route show default dev "$IFACE" metric 1)
        if [ -z "$HAS_V6_ROUTE" ]; then
            ip -6 route add default dev "$IFACE" metric 1
            logger -t "$LOG_TAG" "IPv6: Added default route via $IFACE"
        fi

        # -----------------------------------------------------
        # Task 2: Fix WireGuard VPN Server on Secondary WAN
        # Problem: Local UDP traffic uses Primary WAN gateway & source IP.
        # Fix A: Policy Routing based on Source Port (Directs traffic to WAN2 Table)
        # -----------------------------------------------------
        if ! ip rule show | grep -q "sport $WG_PORT lookup $TABLE"; then
            # Priority 98 ensures it runs before Unifi's default rules
            ip rule add sport "$WG_PORT" lookup "$TABLE" priority 98
            ip route flush cache
            logger -t "$LOG_TAG" "VPN: Added Policy Routing for Source Port $WG_PORT"
        fi

        # -----------------------------------------------------
        # Fix B: Force SNAT (Masquerade)
        # Problem: Source IP might be internal (e.g., 192.168.x.x) even if routed correctly.
        # Fix: Force NAT on egress to ensure Source IP matches the Public IP.
        # -----------------------------------------------------
        if ! iptables -t nat -C POSTROUTING -o "$IFACE" -p udp --sport "$WG_PORT" -j MASQUERADE 2>/dev/null; then
            # Insert at top (1) to override any conflicting rules
            iptables -t nat -I POSTROUTING 1 -o "$IFACE" -p udp --sport "$WG_PORT" -j MASQUERADE
            logger -t "$LOG_TAG" "VPN: Added Force-SNAT rule for port $WG_PORT"
        fi

    fi

    # Check every 30 seconds to handle re-dials or IP changes
    sleep 30
done
```

加上执行权限：`chmod +x /data/internet_as_backup_wan_fix.sh`

然后使用 `crontab -e` 并添加以下内容，设置开机自启：

```
@reboot /data/internet_as_backup_wan_fix.sh > /dev/null 2>&1 &
```

### 为什么一定要用这种方案？

1. **为什么不能 OpenWrt 直接做二级网关（LAN 侧）？** Unifi 不支持 VRRP。如果把 OpenWrt 设为 DHCP 下发的默认网关，一旦 OpenWrt 挂了，Unifi 无法自动把网关切回自己，全家断网。
2. **为什么不能 OpenWrt 做旁路网关配合 PBR？** Unifi 的策略路由（Traffic Routes）只能选 WAN 接口作为出口，不能指定 LAN 侧的某个 IP（OpenWrt）作为下一跳。
3. **为什么必须让 OpenWrt 充当 Primary WAN？**
   如果反过来让 PPPoE 作为 Primary WAN，OpenWrt 作为 Backup WAN，则无法利用双 WAN 的 Failover 功能来应对 OpenWrt 下线的情况，通过策略路由则会有以下问题：
   * **策略路由的 Fallback 功能残废**：需要设置策略路由让海外流量走 Backup WAN，但问题是策略路由的 Fallback 与双 WAN Fallback 机制不通，后者会用 ping 检测是否真正可用，而前者只会在相应接口 Down 掉时才会被禁用，导致就算 OpenWrt 挂掉或死机，但接口还在，海外可直连的流量依然无法 Fallback
   * **DNS 配置麻烦**：默认的 DNS 行为会直接走 PPPoE，造成 DNS 无法抗污染。而若指定走 OpenWrt 的话又难以 Failover，需要 SSH 进去 Unifi 来配置 dnsmasq 为 strict-order 模式，然后使用主-备 DNS 服务器，也很麻烦

**内容**

[1
背景和需求](#bei_jing_he_xu_qiu)

[2
TLDR：Unifi与ROS优缺点对比](#TLDRUnifi_yuROS_you_que_dian_dui_bi)

[3
方案概述](#fang_an_gai_shu)

[3.1
IPv6 与 WireGuard Server 问题](#IPv6_yu_WireGuard_Server_we...