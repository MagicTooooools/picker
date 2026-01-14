---
title: 精打细算VPS扫除
url: https://blog.est.im/2026/stdout-02
source: est の 输入 输出和出入
date: 2026-01-13
fetch_date: 2026-01-14T03:39:20.571304
---

# 精打细算VPS扫除

# [est の 输入 输出和出入](/)

* [Home](/)
* [RSS](https://feeds.feedburner.com/initiative)
* [About](/about)
* [Category](/category)

## 精打细算VPS扫除

Posted 2026-01-13 | stdout

2022年买的VPS一直没怎么管，今天想跑点东西发现大户 warp-cli 真是吃资源啊。果断删掉

公司的服务器都是SA管理，自己的一般很少去折腾，这次也是闲的，好奇系统里杂七杂八都是啥玩意儿，挨个找AI审问一遍

`systemctl list-units --type=service --state=running`

* `blk-availability` `udisks2` 插拔优盘的
* `fwupd` 固件更新
* `ModemManager`
* `multipathd` `open-iscsi` `iscsid` 存储用的
* `packagekit` GUI包管理器
* `polkit` GUI 策略kit
* `snapd` `snapd.apparmor` `snapd.autoimport` GUI里的 App store
* `lvm2-monitor`
* `upower` `thermald` 电源和温度传感器
* `cloud-init*` `cloud-config*` 云配置器
* `apport*` Crash reporting

这些都没用！直接 `sudo systemctl disable --now XXX` 禁用

其中 snapd 直接 `sudo apt purge snapd` 斩草除根！

最后看一下 `free -h` `used=110Mi` 感觉好多了。

顺便把文件也清理下 `sudo journalctl --vacuum-size=500M`，发现比较大，编辑 `vi /etc/systemd/journald.conf`

```
[Journal]
SystemMaxUse=500M
```

然后 `sudo systemctl restart systemd-journald`

为什么要整理VPS？因为大善人Cloudflare 和 Vercel 都有 request buffering 导致一个 hobby project 做不下去了

## Comments