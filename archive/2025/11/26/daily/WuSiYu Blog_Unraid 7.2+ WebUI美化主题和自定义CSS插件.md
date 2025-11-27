---
title: Unraid 7.2+ WebUI美化主题和自定义CSS插件
url: https://wusiyu.me/unraid-7-2-webui-theme-and-custon-css-plugin/
source: WuSiYu Blog
date: 2025-11-26
fetch_date: 2025-11-27T16:52:27.988393
---

# Unraid 7.2+ WebUI美化主题和自定义CSS插件

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

# Unraid 7.2+ WebUI美化主题和自定义CSS插件

发布日期：2025年 11月 27日分类：[Linux & homelab](https://wusiyu.me/category/linux/)
[Unraid 7.2+ WebUI美化主题和自定义CSS插件无评论](https://wusiyu.me/unraid-7-2-webui-theme-and-custon-css-plugin/#respond)

![](https://wusiyu.me/wp-content/uploads/2025/11/screenshot2-1568x1316.png)

由于 Unraid 升级到 7.2+ 版本后，原本常用的 “Theme Engine” 插件已不再兼容，为了继续自定义 WebUI 的样式，我开发了一个轻量级的 CSS 注入插件，并在此基础上发布了一套我个人使用的 “Modernization” 主题。

## 1. Custom WebUI CSS 插件

这是一个专门为 Unraid 7.2+ 设计的轻量级插件。我的需求很简单：将自定义 CSS 代码注入到网页中。既然旧插件无法使用，我便编写了这个替代方案。

**主要功能：**

* **全局 CSS 注入：** 将 CSS 代码自动加载到每个页面的 `<head>` 中。
* **深色主题附加CSS：** 可输入仅在 Unraid 主题设置为 ‘black’ 时加载的额外CSS，用于方便在黑/白主题间兼容和切换。
* **额外资源文件：** 支持从 Flash 到 Web 目录同步额外的静态资源（如背景图片等），可在CSS中引用。

**项目地址：** <https://github.com/WuSiYu/unraid-custom-css>

Unraid论坛地址：<https://forums.unraid.net/topic/195276-plugin-simple-custom-webui-css-plugin-for-unraid-72>

> **注意：** 本插件仅在 Unraid 7.2+ 上测试通过。如果你使用的是 7.1.4 及以下版本，建议继续使用 Theme Engine。
>
> ![](https://wusiyu.me/wp-content/uploads/2025/11/Tower_CustomCSS-scaled.jpeg)

## 2. Modernization Theme

配合上述插件，我整理了个人使用的 CSS 样式，命名为 Modernization Theme。

**设计特点：**

* **现代化外观：** 重新设计了 Dashboard 和 Main 等核心页面的视觉体验。
* **按钮风格：** 改善了Unraid原本比较丑的圆角渐变边框设计，现在具有完美的圆角，并在支持的浏览器上还有渐变文字。
* **黑/白主题兼容：** 适配 Unraid 原生的 White 和 Black 两种基础主题。
* **稳健性：** 克制了 CSS 的修改范围，尽量避免破坏第三方插件的显示效果。

![](https://wusiyu.me/wp-content/uploads/2025/11/screenshot2.png)
![](https://wusiyu.me/wp-content/uploads/2025/11/screenshot-scaled.png)
![](https://wusiyu.me/wp-content/uploads/2025/11/%E6%88%AA%E5%B1%8F2025-11-25-22.13.26.png)
![](https://wusiyu.me/wp-content/uploads/2025/11/%E6%88%AA%E5%B1%8F2025-11-25-22.13.13.png)

## 3. 安装与使用指南

### 第一步：安装插件

进入 Unraid 的 `PLUGINS` -> `Install Plugin` 页面，输入以下 URL 进行安装：

```
https://raw.githubusercontent.com/WuSiYu/unraid-custom-css/refs/heads/master/custom.css.plg
```

（正在等待CA应用商店审核，不出意外等之后也可以直接在上面装）

### 第二步：输入主题CSS

1. 安装完成后，前往 `SETTINGS` -> `Custom WebUI CSS`。
2. 打开下方链接，复制其中的 CSS 代码： <https://github.com/WuSiYu/unraid-custom-css/blob/master/example.css>
3. 将代码粘贴到插件的配置框中，点击应用即可。

本主题有意限制了自定义样式的生效范围，以最大程度避免破坏第三方插件的样式。但如果你在使用过程中依然了遇到任何第三方插件的兼容性问题，欢迎反馈。

**内容**

[1
1. Custom WebUI CSS 插件](#1_Custom_WebUI_CSS_cha_jian)

[2
2. Modernization Theme](#2_Modernization_Theme)

[3
3. 安装与使用指南](#3_an_zhuang_yu_shi_yong_zhi_nan)

[3.1
第一步：安装插件](#di_yi_bu_an_zhuang_cha_jian)

[3.2
第二步：输入主题CSS](#di_er_bu_shu_ru_zhu_tiCSS)

发布日期：2025年 11月 27日作者：[WuSiYu](https://wusiyu.me/author/wusiyu/)

分类：[Linux & homelab](https://wusiyu.me/category/linux/)

![](https://secure.gravatar.com/avatar/86fbd622aa03e0c8b6e4e91862b88026a77d0248a430f9cca332c4583d5743bc?s=85&d=retro&r=r)

## 发表评论 [取消回复](/unraid-7-2-webui-theme-and-custon-css-plugin/#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

Δ

## 文章导航

[上一篇文章

杂谈：libvirt/qemu Windows游戏VM的一些优化配置](https://wusiyu.me/libvirt-qemu-windows-gaming-vm-optimization/)

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

2025 年 11 月

| 一 | 二 | 三 | 四 | 五 | 六 | 日 |
| --- | --- | --- | --- | --- | --- | --- |
|  | | | | | 1 | 2 |
| 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| 10 | 11 | 12 | 13 | 14 | 15 | 16 |
| 17 | 18 | 19 | 20 | 21 | 22 | 23 |
| 24 | 25 | 26 | [27](https://wusiyu.me/2025/11/27/) | 28 | 29 | 30 |

[10 月](https://wusiyu.me/2025/10/)

### 最多获赞

* [关于HP ENVY笔记本国行预装系统中内置隐藏的流氓软件](https://wusiyu.me/%E5%85%B3%E4%BA%8Ehp-envy%E7%AC%94%E8%AE%B0%E6%9C%AC%E5%9B%BD%E8%A1%8C%E9%A2%84%E8%A3%85%E7%B3%BB%E7%BB%9F%E4%B8%AD%E5%86%85%E7%BD%AE%E9%9A%90%E8%97%8F%E7%9A%84%E6%B5%81%E6%B0%93%E8%BD%AF%E4%BB%B6/)
* [SMCL - 使用Python编写简单Minecraft启动器](https://wusiyu.me/smcl-%E4%BD%BF%E7%94%A8python%E7%BC%96%E5%86%99%E7%AE%80%E5%8D%95minecraft%E5%90%AF%E5%8A%A8%E5%99%A8/)
* [Intel Core Ultra 笔记本处理器集成NPU初探（Intel AI Boost）](https://wusiyu.me/intel-core-ultra-npu-quicklook-intel-ai-boost/)
* [HP Proliant DL360p Gen8 服务器6pin风扇接口定义及检测/欺骗主板的方法](https://wusiyu.me/hp-proliant-dl306p-gen8-%E6%9C%8D%E5%8A%A1%E5%99%A86pin%E9%A3%8E%E6%89%87%E6%8E%A5%E5%8F%A3%E5%AE%9A%E4%B9%89%E5%8F%8A%E6%A3%80%E6%B5%8B-%E6%AC%BA%E9%AA%97%E4%B8%BB%E6%9D%BF%E7%9A%84%E6%96%B9%E6%B3%95/)
* [6502 CPU汇编语言指令集](https://wusiyu.me/6502-cpu%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80%E6%8C%87%E4%BB%A4%E9%9B%86/)
* [家用 Proxmox VE 从入门到套娃（对比ESXi）](https://wusiyu.me/%E5%AE%B6%E7%94%A8-proxmox-ve-%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E5%A5%97%E5%A8%83%EF%BC%88%E5%AF%B9%E6%AF%94esxi%EF%BC%89/)
* [Linux 在4K屏幕下的界面缩放设置](https://wusiyu.me/linux-%E5%9C%A84k%E5%B1%8F%E5%B9%95%E4%B8%8B%E7%9A%84%E7%95%8C%E9%9D%A2%E7%BC%A9%E6%94%BE%E8%AE%BE%E7%BD%AE/)
* [在Arch Linux中禁止生成/boot/initramfs-linux-fallback.img以节省/boot分区空间](https://wusiyu.me/archlinux-remove-initramfs-linux-fallback-img/)
* [小米WiFi放大器pro测评及改造(然后坏掉)](https://wusiyu.me/xiaomi-wifi-repeater-pro/)
* [HP ProLiant DL360p Gen8 服务器家用指南](https://wusiyu.me/hp-proliant-dl360p-gen8-%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%B6%E7%94%A8%E6%8C%87%E5%8D%97/)

### 今日氵图（不定期更新）

[![](https://wusiyu.me/wp-content/uploads/2023/07/fish.jpeg)](https://wusiyu.me/wp-content/uploads/2023/07/fish.jpeg)

### 其他操作

* [登录](https://wusiyu.me/wp-login.php)
* [条目 feed](https://wusiyu.me/feed/)
* [评论 feed](https://wusiyu.me/comments/feed/)
* [WordPress.org](https://cn.wordpress.org/)

**内容**

[1
1. Custom WebUI CSS 插件](#1_Custom_WebUI_CSS_cha_jian)

[2
2. Modernization Theme](#2_Modernization_Theme)

[3
3. 安装与使用指南](#3_an_zhuang_yu_shi_yong_zhi_nan)

[3.1
第一步：安装插件](#di_yi_bu_an_zhuang_cha_jian)

[3.2
第二步：输入主题CSS](#di_er_bu_shu_ru_zhu_tiCSS)

## 近期文章

* [Unraid 7.2+ WebUI美化主题和自定义CSS插件](https://wusiyu.me/unraid-7-2-webui-theme-and-custon-css-plugin/)
* [杂谈：libvirt/qemu Windows游戏VM的一些优化配置](https://wusiyu.me/libvirt-qemu-windows-gaming-vm-optimization/)
* [Fedora + libvirt 下在宿主关机时安全关闭虚拟机](https://wusiyu.me/fedora-libvirt-vm-graceful-shutdown-with-host/)
* [群晖ddrescue与暂时禁用USB外接硬盘自动挂载](https://wusiyu.me/synology-ddrescue-and-disable-usb-external-disk-auto-mount/)
* [OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/)

## 近期评论

* lios 发表在《[Intel Core Ultra 笔记本处理器集成NPU初探（Intel AI Boost）](https://wusiyu.me/intel-core-ultra-npu-quicklook-intel-ai-boost/#comment-14179)》
* WuSiYu 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-13229)》
* 行走的地瓜🍠 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-13116)》
* WuSiYu 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-12381)》
* RK 发表在《[OpenWrt One 路由器（MT7981，主线op）超频bl2编译教程](https://wusiyu.me/openwrt-one-mt7981-bl2-overclocking-compile-guide/#comment-12244)》

## 统计信息

* 在线访客: 0
* 今日浏览量: 834
* 昨日访问量: 350
* 近 30 天的访问量: 64,612
* 总浏览量: 1,380,173
* 总浏览量: 4
* 总计文章: 141
* 评论总数: 329
* 最后发表日期: 2025年 11月 27日

## 友情链接

* [Lensual‘s Space](https://lensual.space)
* [本地磁盘姬](http://ohayou.aimo.moe)
* [杰克部落](https://renjikai.com/)
* [Pantheon](https://blog.pantheon.press)

## 归档

归档

选择月份
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
 2021 年 6 月 ...