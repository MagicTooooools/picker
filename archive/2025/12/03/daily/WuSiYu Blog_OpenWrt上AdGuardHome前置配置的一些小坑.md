---
title: OpenWrt上AdGuardHome前置配置的一些小坑
url: https://wusiyu.me/openwrt-adguardhome-on-front-with-passwall/
source: WuSiYu Blog
date: 2025-12-03
fetch_date: 2025-12-04T03:22:54.139407
---

# OpenWrt上AdGuardHome前置配置的一些小坑

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

# OpenWrt上AdGuardHome前置配置的一些小坑

发布日期：2025年 12月 3日分类：[Linux & homelab](https://wusiyu.me/category/linux/)
[OpenWrt上AdGuardHome前置配置的一些小坑无评论](https://wusiyu.me/openwrt-adguardhome-on-front-with-passwall/#respond)

![](https://wusiyu.me/wp-content/uploads/2025/12/截屏2025-12-03-19.44.55.jpeg)

为了配合*一些上网插件*并达到最好的过滤效果，需要将AdGuardHome前置于OpenWrt自带的dnsmasq：

```
[设备] -> [AdGuardHome 53端口] -> [dnsmasq 54端口] -> 一些插件 -> 多种DNS服务器
```

AdGuardHome在这里作为第一级DNS服务器，运行在标准53端口上，直接接受客户端连接。然后其上游为OpenWrt的自带dnsmasq的DNS服务器，这里需要将其端口改为端口54以避免冲突。之后*一些上网插件*会自动将dnsmasq的上游设置为它的*一些程序*，最终连接到大陆或外网的DNS。

这一套似乎没什么问题，但其实有一些坑：

### 坑1: DHCP v4 不下发DNS服务器配置

OpenWrt dnsmasq的DNS服务若运行在未53端口上时，其DHCP v4 服务就不会默认发送DNS选项。导致部分支持IPv6的设备依然可以通过IPv6连接DNS，表现正常，但仅支持IPv4的设备就会无法上网。

解法很简单，需要在**OpenWrt -> 网络 -> lan -> DHCP 服务器 -> 高级设置** 处，手动强制加入一个DHCP选项来下发DNS服务器配置：`6,<路由器IP>`

![](https://wusiyu.me/wp-content/uploads/2025/12/%E6%88%AA%E5%B1%8F2025-12-03-01.30.08.png)

### 坑2: 在一些插件内，勾选DNS重定向会绕过AdGuardHome

![](https://wusiyu.me/wp-content/uploads/2025/12/%E6%88%AA%E5%B1%8F2025-12-03-19.35.24.png)

如果你勾选了这个选择，那DNS请求会被强制直接转达到dnsmasq的地址，也就是54端口上，导致AdGuardHome被绕过了。那如果你仍然需要类似的功能呢？可以在 **OpenWrt -> 网络 -> 防火墙 -> 端口转发中手动创建一条规则**

![](https://wusiyu.me/wp-content/uploads/2025/12/image.png)

具体如下：

```
# /etc/config/firewall
config redirect 'dns_int'
	option name 'Intercept-DNS'
	option family 'any'
	option proto 'tcp udp'
	option src 'lan'
	option src_dport '53'
	option target 'DNAT'
```

**内容**

[1
坑1: DHCP v4 不下发DNS服务器配置](#keng1_DHCP_v4_bu_xia_faDNS_fu_wu_qi_pei_zhi)

[2
坑2: 在一些插件内，勾选DNS重定向会绕过AdGuardHome](#keng2_zai_yi_xie_cha_jian_nei_gou_xuanDNS_zhong_ding_xiang_hui_rao_guoAdGuardHome)

发布日期：2025年 12月 3日作者：[WuSiYu](https://wusiyu.me/author/wusiyu/)

分类：[Linux & homelab](https://wusiyu.me/category/linux/)

![](https://secure.gravatar.com/avatar/86fbd622aa03e0c8b6e4e91862b88026a77d0248a430f9cca332c4583d5743bc?s=85&d=retro&r=r)

## 发表评论 [取消回复](/openwrt-adguardhome-on-front-with-passwall/#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

Δ

## 文章导航

[上一篇文章

Unraid 7.2+ WebUI美化主题和自定义CSS插件](https://wusiyu.me/unraid-7-2-webui-theme-and-custon-css-plugin/)

### 关于

本站为WuSiYu的个人博客，

HPC/MLsys方向在读博士生；

涉猎或将会涉猎：

* 高性能计算
* AI工具链/运行时
* 智能硬件&DIY
* Linux和Linux桌面
* 家庭服务器与小型路由器
* 全栈Web开发

### Publications

* **[ASPLOS '25]** [Past-Future Scheduler for LLM Serving under SLA Guarantees](https://doi.org/10.1145/3676641.3716011)
* **[ICPP '24]** [PRoof: A Comprehensive Hierarchical Profiling Framework for Deep Neural Networks with Roofline Analysis](https://doi.org/10.1145/3673038.3673116)

搜索…

2025 年 12 月

| 一 | 二 | 三 | 四 | 五 | 六 | 日 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 2 | [3](https://wusiyu.me/2025/12/03/) | 4 | 5 | 6 | 7 |
| 8 | 9 | 10 | 11 | 12 | 13 | 14 |
| 15 | 16 | 17 | 18 | 19 | 20 | 21 |
| 22 | 23 | 24 | 25 | 26 | 27 | 28 |
| 29 | 30 | 31 |  | | | |

[11 月](https://wusiyu.me/2025/11/)

### 最多获赞

* [关于HP ENVY笔记本国行预装系统中内置隐藏的流氓软件](https://wusiyu.me/%E5%85%B3%E4%BA%8Ehp-envy%E7%AC%94%E8%AE%B0%E6%9C%AC%E5%9B%BD%E8%A1%8C%E9%A2%84%E8%A3%85%E7%B3%BB%E7%BB%9F%E4%B8%AD%E5%86%85%E7%BD%AE%E9%9A%90%E8%97%8F%E7%9A%84%E6%B5%81%E6%B0%93%E8%BD%AF%E4%BB%B6/)
* [SMCL - 使用Python编写简单Minecraft启动器](https://wusiyu.me/smcl-%E4%BD%BF%E7%94%A8python%E7%BC%96%E5%86%99%E7%AE%80%E5%8D%95minecraft%E5%90%AF%E5%8A%A8%E5%99%A8/)
* [Intel Core Ultra 笔记本处理器集成NPU初探（Intel AI Boost）](https://wusiyu.me/intel-core-ultra-npu-quicklook-intel-ai-boost/)
* [HP Proliant DL360p Gen8 服务器6pin风扇接口定义及检测/欺骗主板的方法](https://wusiyu.me/hp-proliant-dl306p-gen8-%E6%9C%8D%E5%8A%A1%E5%99%A86pin%E9%A3%8E%E6%89%87%E6%8E%A5%E5%8F%A3%E5%AE%9A%E4%B9%89%E5%8F%8A%E6%A3%80%E6%B5%8B-%E6%AC%BA%E9%AA%97%E4%B8%BB%E6%9D%BF%E7%9A%84%E6%96%B9%E6%B3%95/)
* [6502 CPU汇编语言指令集](https://wusiyu.me/6502-cpu%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80%E6%8C%87%E4%BB%A4%E9%9B%86/)
* [家用 Proxmox VE 从入门到套娃（对比ESXi）](https://wusiyu.me/%E5%AE%B6%E7%94%A8-proxmox-ve-%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E5%A5%97%E5%A8%83%EF%BC%88%E5%AF%B9%E6%AF%94esxi%EF%BC%89/)
* [Linux 在4K屏幕下的界面缩放设置](https://wusiyu.me/linux-%E5%9C%A84k%E5%B1%8F%E5%B9%95%E4%B8%8B%E7%9A%84%E7%95%8C%E9%9D%A2%E7%BC%A9%E6%94%BE%E8%AE%BE%E7%BD%AE/)
* [HP ProLiant DL360p Gen8 服务器家用指南](https://wusiyu.me/hp-proliant-dl360p-gen8-%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%B6%E7%94%A8%E6%8C%87%E5%8D%97/)
* [在Arch Linux中禁止生成/boot/initramfs-linux-fallback.img以节省/boot分区空间](https://wusiyu.me/archlinux-remove-initramfs-linux-fallback-img/)

### 今日氵图（不定期更新）

[![](https://wusiyu.me/wp-content/uploads/2023/07/fish.jpeg)](https://wusiyu.me/wp-content/uploads/2023/07/fish.jpeg)

### 其他操作

* [登录](https://wusiyu.me/wp-login.php)
* [条目 feed](https://wusiyu.me/feed/)
* [评论 feed](https://wusiyu.me/comments/feed/)
* [WordPress.org](https://cn.wordpress.org/)

**内容**

[1
坑1: DHCP v4 不下发DNS服务器配置](#keng1_DHCP_v4_bu_xia_faDNS_fu_wu_qi_pei_zhi)

[2
坑2: 在一些插件内，勾选DNS重定向会绕过AdGuardHome](#keng2_zai_yi_xie_cha_jian_nei_gou_xuanDNS_zhong_ding_xiang_hui_rao_guoAdGuardHome)

## 近期文章

* [OpenWrt上AdGuardHome前置配置的一些小坑](https://wusiyu.me/openwrt-adguardhome-on-front-with-passwall/)
* [Unraid 7.2+ WebUI美化主题和自定义CSS插件](https://wusiyu.me/unraid-7-2-webui-theme-and-custon-css-plugin/)
* [杂谈：libvirt/qemu Windows游戏VM的一些优化配置](https://wusiyu.me/libvirt-qemu-windows-gaming-vm-optimization/)
* [Fedora + libvirt 下在宿主关机时安全关闭虚拟机](https://wusiyu.me/fedora-libvirt-vm-graceful-shutdown-with-host/)
* [群晖ddrescue与暂时禁用USB外接硬盘自动挂载](https://wusiyu.me/synology-ddrescue-and-disable-usb-external-disk-auto-mount/)

## 近期评论

* lios 发表在《[Intel Core Ultra 笔记本处理器集成NPU初探（Intel AI Boost）](https://wusiyu.me/intel-core-ultra-npu-quicklook-intel-ai-boost/#comment-14179)》
* WuSiYu 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-13229)》
* 行走的地瓜🍠 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-13116)》
* WuSiYu 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-12381)》
* RK 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-12244)》

## 统计信息

* 在线访客: 2
* 今日浏览量: 790
* 昨日访问量: 1,259
* 近 30 天的访问量: 122,456
* 总浏览量: 1,443,955
* 总浏览量: 4
* 总计文章: 142
* 评论总数: 329
* 最后发表日期: 2025年 12月 4日

## 友情链接

* [Lensual‘s Space](https://lensual.space)
* [本地磁盘姬](http://ohayou.aimo.moe)
* [杰克部落](https://renjikai.com/)
* [Pantheon](https://blog.pantheon.press)

## 归档

归档

选择月份
 2025 年 12 月  (1)
 2025 年 11 月  (1)
 2025 年 10 月  (1)
 2025 年 7 月  (1)
 2025 年 3 月  (1)
 2024 年 12 月  (1)
 2024 年 7 月  (1)
 2024 年 6 月  (1)
 2024 年 4 月  (1)
 2023 年 8 月  (1)
 2023 年 7 月  (1)
 2023 年 6 月  (1)
 2023 年 3 月  (1)
 2023 年 1 月  (2)
 2022 年 6 月  (2)
 2022 年 5 月  (3)
 2022 年 1 月  (1)
 2021 年 9 月  (1)
 2021 年 8 月  (1)
 2021 年 6 月  (1)
 2021 年 5 月  (1)
 2021 年 3 月  (2)
 2021 年 2 月  (2)
 2020 年 4 月  (1)
 2020 年 2 月  (3)
 2020 年 1 月  (2)
 2019 年 9 月  (2)
 2019 年 7 月  (1)
 2019 年 4 月  (1)
 2019 年 3 月  (1)
 2019 年 2 月  (1)
 2018 年 11 月  (1)
 2018 年 10 月  (1)
 2018 年 7 月  (3)
 2018 年 6 月  (1)
 2018 年 3 月  (1)
 2018 年 2 月  (3)
 2018 年 1 月  (3)
 2017 年 12 月  (1)
 2017 年 7 月  (1)
 2017 年 5 月  (1)
 2017 年 4 月  (2)
 2017 年 3 月  (2)
 2017 年 2 月  (1)
 2016 年 12 月  (5)
 2016 年 10 月  (3)
 2016 年 9 月  (2)
 2016 年 8 月  (2)
 2016 年 7 月  (1)
 2016 年 6 月  (1)
 2016 年 5 月  (2)
 2016 年 4 月  (3)
 2016 年 3 月  (6)
 2016 年 2 月  (2)
 2016 年 1 月  (6)
 2015 年 12 月  (2)
 2015 年 11 月  (5)
 2015 年 10 月  (1)
 2015 年 9 月  (5)
 2015 年 8 月  (2)
 2015 年 7 月  (1)
 2015 年 6 月  (5)
 2015 年 5 月  (2)
 2015 年 2 月  (3)
 2015 年 1 月  (4)
 2014 年 10 月  (1)
 2014 年 8 月  (3)
 2014 年 7 月  (9)
 2014 年 6 月  (1)
 2014 年 4 月  (1)
 2014 年 3 月  ...