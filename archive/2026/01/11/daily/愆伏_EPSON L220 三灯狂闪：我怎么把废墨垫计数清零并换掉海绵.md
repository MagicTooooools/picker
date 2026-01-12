---
title: EPSON L220 三灯狂闪：我怎么把废墨垫计数清零并换掉海绵
url: https://www.tortorse.com/archives/epson-l220-waste-ink-pad-reset/
source: 愆伏
date: 2026-01-11
fetch_date: 2026-01-12T03:40:03.522175
---

# EPSON L220 三灯狂闪：我怎么把废墨垫计数清零并换掉海绵

[愆伏](/)

互联网杂谈

* [首页](/)
* [分类](/categories/)
* [关于](/about/)
* [归档](/archives/)
* [标签](/tags/)
* [实验室](/lab/)
* [链接](/links/)
* [统计](/statistics/)
* [播客](/podcast/)
* [读书](/books/)
* [游戏](/games/)
* [听歌](/musics/)
* 搜索

* 文章目录
* 站点概览

1. [1. 这事其实就两件事](#%E8%BF%99%E4%BA%8B%E5%85%B6%E5%AE%9E%E5%B0%B1%E4%B8%A4%E4%BB%B6%E4%BA%8B)
2. [2. 最大的麻烦：清零软件只有 Windows 版](#%E6%9C%80%E5%A4%A7%E7%9A%84%E9%BA%BB%E7%83%A6%E6%B8%85%E9%9B%B6%E8%BD%AF%E4%BB%B6%E5%8F%AA%E6%9C%89-windows-%E7%89%88)
3. [3. 清零步骤（我这次用的路径）](#%E6%B8%85%E9%9B%B6%E6%AD%A5%E9%AA%A4%E6%88%91%E8%BF%99%E6%AC%A1%E7%94%A8%E7%9A%84%E8%B7%AF%E5%BE%84)
4. [4. 更换废墨垫海绵（物理部分）](#%E6%9B%B4%E6%8D%A2%E5%BA%9F%E5%A2%A8%E5%9E%AB%E6%B5%B7%E7%BB%B5%E7%89%A9%E7%90%86%E9%83%A8%E5%88%86)
5. [5. 写在最后](#%E5%86%99%E5%9C%A8%E6%9C%80%E5%90%8E)

![tortorse](/assets/images/avatar.png)

tortorse

一个浸淫互联网行业多年的斜杠中年，通过博客表达自己的所思所想以及所经历的历史进程

[518
日志](/archives/)

[8
分类](/categories/)

[328
标签](/tags/)

[![Creative Commons](https://cdnjs.cloudflare.com/ajax/libs/creativecommons-vocabulary/2020.11.3/assets/license_badges/small/by_nc_sa.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/zh-CN)

# EPSON L220 三灯狂闪：我怎么把废墨垫计数清零并换掉海绵

发表于
2026-01-11

分类于

[技术](/categories/%E6%8A%80%E6%9C%AF/)

评论数：

阅读次数：

本文字数：
1.2k

阅读时长 ≈
2 分钟

最近给孩子打印的资料有点多，家里的打印机终于“不堪重负”罢工了。

当时的现象是：**墨水灯、纸张灯、电源灯三个灯一起快速闪烁**，按键也没反应。这两天还要继续打印资料，要是修不好，就得出门找打印店，这就有点麻烦了。

我翻了下 EPSON L220 的手册，故障说明大概是这样：

![](/wp-content/uploads/2026/epson-l220-manual-error-lights.jpg)

手册里说可能是扫描部分有问题，但我扫描台上什么都没放，而且以前也遇到过类似情况，所以我更怀疑是另一个常见原因：**废墨垫计数满了，需要清零**。

## 这事其实就两件事

后来我确认下来，这类问题大概率绕不开两步：

1. **物理处理**：打开打印机后面的废墨仓，把里面的海绵（废墨垫）换新。淘宝上能买到配套海绵。
2. **软件清零**：把打印机固件里「废墨垫计数」清零（不然它会一直以为“满了”）。

如果只做软件清零不换海绵，机器是能继续打，但废墨垫真的可能溢出来——我不想把家里搞成案发现场，所以两件事我都做了。

## 最大的麻烦：清零软件只有 Windows 版

一般修打印机的店里都有那个清零软件（常见叫法是 *Adjustment Program* 一类），但我手边只有 Mac。

我一开始的想法是“凑合用一下 Windows 就行”，于是找来我老婆很久没用的一台 **Intel 的 MacBook Air**，插上我很久以前做的 **Win2Go U 盘**，准备临时救个急。

结果 Win2Go 一登录就开始疯狂闪烁，Explorer 反复重启。我查了下，基本就是显卡驱动不兼容。进安全模式倒是不闪了，但安全模式下 **压根识别不到打印机**。我折腾了半天，还是没走通这条路。

最后我只好在自己的 **MacBook M1 Pro** 上装了 **Parallels Desktop**，在虚拟机里跑 Windows，才把清零软件跑起来。

## 清零步骤（我这次用的路径）

软件打开后，先选 `Particular adjustment mode`：

![](/wp-content/uploads/2026/epson-l220-adjustment-particular-mode.png)

接着最关键的一步是 **Port**。

Port 要选到打印机对应的端口。我这里是 `USB001`。如果它一直显示 `Auto`，基本就意味着系统没识别到打印机——这种情况下你就算硬选了，后面也大概率连接失败。

![](/wp-content/uploads/2026/epson-l220-adjustment-port-usb001.png)

进入后会看到一个列表，在 `Maintenance` 里找到 `Waste ink pad counter`：

![](/wp-content/uploads/2026/epson-l220-waste-ink-pad-counter-menu.png)

勾选 `Main pad count`，先点右下角 `Check`，软件会读出当前计数和百分比。一般到这一步，数值都已经很接近上限了（很多人看到都是 95%+）。

![](/wp-content/uploads/2026/epson-l220-waste-ink-pad-check.png)

然后点 `Initialize` 执行清零。清零后 `point` 会变成 `0`。

我做完清零、重启打印机，三灯闪烁的故障就消失了，打印也恢复正常。

## 更换废墨垫海绵（物理部分）

虽然清零后已经能用了，但我还是下单换了新的海绵，免得哪天真的溢出来。

更换不复杂，我大致按这个顺序做的：

找到 EPSON L220 背后的一颗螺丝，拧下来，然后把打印机立起来：

![](/wp-content/uploads/2026/epson-l220-back-screw.jpg)

将底部向右推，就能打开废墨仓：

![](/wp-content/uploads/2026/epson-l220-open-waste-ink-compartment.jpg)

拿出旧的海绵，放入新的海绵，装回去，再把螺丝拧上：

![](/wp-content/uploads/2026/epson-l220-sponge-replacement.jpg)

## 写在最后

这次的结论很朴素：遇到三灯狂闪，别急着判死刑，先看看是不是废墨垫计数满了。
我折腾的过程挺绕，但好在最终解决了，打印机也算“续命成功”，孩子的资料也没耽误。

请我一杯咖啡吧！

赞赏

![tortorse 微信](/assets/images/wechat.png)
微信

* **原作者：** 愆伏
* **本文链接：**
  [https://www.tortorse.com/archives/epson-l220-waste-ink-pad-reset/](https://www.tortorse.com/archives/epson-l220-waste-ink-pad-reset/ "EPSON L220 三灯狂闪：我怎么把废墨垫计数清零并换掉海绵")
* **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/zh-CN) 许可协议。转载请注明出处！

[# 打印机](/tags/%E6%89%93%E5%8D%B0%E6%9C%BA/)
[# EPSON](/tags/EPSON/)
[# L220](/tags/L220/)
[# 废墨垫](/tags/%E5%BA%9F%E5%A2%A8%E5%9E%AB/)
[# Parallels](/tags/Parallels/)
[# Windows](/tags/Windows/)

[南京/远程：我在寻找兼职与外包合作](/archives/nanjing-remote-part-time-outsourcing/ "南京/远程：我在寻找兼职与外包合作")

© 2003 –
2026

tortorse

265k

8:02

0%

Theme NexT works best with JavaScript enabled