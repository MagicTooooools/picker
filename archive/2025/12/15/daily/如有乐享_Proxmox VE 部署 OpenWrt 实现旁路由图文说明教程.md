---
title: Proxmox VE 部署 OpenWrt 实现旁路由图文说明教程
url: https://51.ruyo.net/19234.html
source: 如有乐享
date: 2025-12-15
fetch_date: 2025-12-16T03:27:55.011394
---

# Proxmox VE 部署 OpenWrt 实现旁路由图文说明教程

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/mobile-header.svg)

* [首页](https://51.RUYO.net/ "首页")
* [技术教程](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/ "技术教程")
  + [开源代码](https://51.ruyo.net/category/jishu/open-source)
  + [技术分享](https://51.ruyo.net/category/jishu/)
  + [学生认证](https://51.ruyo.net/?s=%E5%AD%A6%E7%94%9F%E8%AE%A4%E8%AF%81)
  + [OneDrive](https://51.ruyo.net/tag/onedrive/)
  + [Windows激活](https://51.ruyo.net/tag/windows%E6%BF%80%E6%B4%BB%E4%B9%8B%E8%B7%AF/)
* [软件工具](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6 "软件工具")
  + [系统软件](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/os-system)
  + [游戏软件](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/game)
  + [开发软件](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/dev-software)
  + [Chrome扩展](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/chrome-extension)
* [云服务器](https://51.ruyo.net/category/vps "云服务器")
  + [国内VPS](https://51.ruyo.net/category/vps/china-vps)
  + [国外VPS](https://51.ruyo.net/category/vps/intal-vps)
  + [主机评测](https://51.ruyo.net/category/vps/zhujipingce "主机评测")
  + [甲骨文云](https://51.ruyo.net/tag/oracle-cloud/ " 甲骨文云")
* [网络资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90 "网络资源")
  + [国外号码](https://51.ruyo.net/tag/%E5%A2%83%E5%A4%96%E7%94%B5%E8%AF%9D%E5%8D%A1/)
  + [域名资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/domain)
  + [邮箱资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/youxiang "邮箱资源")
  + [教育邮箱](https://51.ruyo.net/tag/edu%E9%82%AE%E7%AE%B1/)
  + [建站教程](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/build-website)
  + [教程资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/jiaocheng)
  + [网盘资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/wangpan)

[![如有乐享 - 分享是一种态度，是一种美德](https://img13.360buyimg.com/ddimg/jfs/t1/215482/37/23392/31259/637648ccE4d101455/544f3506f65472ee.png)](https://51.ruyo.net "如有乐享 - 分享是一种态度，是一种美德")

## 搜索内容

搜索

* [首页](https://51.RUYO.net/ "首页")
* [技术教程](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/ "技术教程")
  + [开源代码](https://51.ruyo.net/category/jishu/open-source)
  + [技术分享](https://51.ruyo.net/category/jishu/)
  + [学生认证](https://51.ruyo.net/?s=%E5%AD%A6%E7%94%9F%E8%AE%A4%E8%AF%81)
  + [OneDrive](https://51.ruyo.net/tag/onedrive/)
  + [Windows激活](https://51.ruyo.net/tag/windows%E6%BF%80%E6%B4%BB%E4%B9%8B%E8%B7%AF/)
* [软件工具](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6 "软件工具")
  + [系统软件](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/os-system)
  + [游戏软件](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/game)
  + [开发软件](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/dev-software)
  + [Chrome扩展](https://51.ruyo.net/category/%E8%BD%AF%E4%BB%B6/chrome-extension)
* [云服务器](https://51.ruyo.net/category/vps "云服务器")
  + [国内VPS](https://51.ruyo.net/category/vps/china-vps)
  + [国外VPS](https://51.ruyo.net/category/vps/intal-vps)
  + [主机评测](https://51.ruyo.net/category/vps/zhujipingce "主机评测")
  + [甲骨文云](https://51.ruyo.net/tag/oracle-cloud/ " 甲骨文云")
* [网络资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90 "网络资源")
  + [国外号码](https://51.ruyo.net/tag/%E5%A2%83%E5%A4%96%E7%94%B5%E8%AF%9D%E5%8D%A1/)
  + [域名资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/domain)
  + [邮箱资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/youxiang "邮箱资源")
  + [教育邮箱](https://51.ruyo.net/tag/edu%E9%82%AE%E7%AE%B1/)
  + [建站教程](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/build-website)
  + [教程资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/jiaocheng)
  + [网盘资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/wangpan)

[登录](https://51.ruyo.net/wp-login.php?redirect_to=https%3A%2F%2F51.ruyo.net%2F19234.html)

- [主页](https://51.ruyo.net)
- [教程资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/jiaocheng)
- [网络资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90)

# Proxmox VE 部署 OpenWrt 实现旁路由图文说明教程

[我是小马甲~](https://51.ruyo.net/author/admin)
•

2025-12-15
•
[教程资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90/jiaocheng), [网络资源](https://51.ruyo.net/category/%E7%BD%91%E7%BB%9C%E8%B5%84%E6%BA%90) •
695 阅读

![Proxmox VE 部署 OpenWrt 实现旁路由图文说明教程](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

系列文章：[家庭服务器](https://51.ruyo.net/tag/%E5%AE%B6%E5%BA%AD%E6%9C%8D%E5%8A%A1%E5%99%A8)

前几天打算把家里小主机的服务升级一下！因为更新过程中需要访问Github等海外线上服务。但是！你懂的，经常莫名其妙无法访问！升级中断后导致源码出现了问题！

**虽然博主之前分享过很多解决方案：[GitHub国内加速](https://51.ruyo.net/tag/github%E5%9B%BD%E5%86%85%E5%8A%A0%E9%80%9F/)，但在Linux上使用还是不太方便！**

于是周末折腾了一下利用 [Proxmox](https://51.ruyo.net/tag/Proxmox) 安装OpenWrt，然后OpenWrt旁路由为其他内网服务器加速！

更多内容：[家庭IDC](https://51.ruyo.net/tag/%E5%AE%B6%E5%BA%AD%E6%9C%8D%E5%8A%A1%E5%99%A8) / [Proxmox](https://51.ruyo.net/tag/Proxmox)

## OpenWrt选择

* Lean 的 OpenWrt 源码（Lienol）

> 该源码库在 OpenWrt 社区影响力巨大，但其主仓库地址可能会根据开发者策略和平台变化而调整。
>
> 项目定位： 这是一个用于自行编译的 OpenWrt 源码，其中包含了大量的中文用户常用插件。
>
> 主流仓库地址：
>
> GitHub： 您可以在 GitHub 上搜索 Lean openwrt 或 openwrt-lede 来找到目前活跃的仓库。由于原作者的仓库有时会关闭或变动，建议寻找社区中维护活跃且 Star 数量高的 Fork 或镜像仓库。

* ImmortalWrt（不朽固件）

> 这是 OpenWrt 在国内社区的一个重要分支项目，旨在更好地服务中国用户，提供更多的本地化功能和硬件支持。
>
> GitHub 源码地址：https://github.com/immortalwrt/immortalwrt
>
> 说明： 这是 ImmortalWrt 项目的官方源码仓库，可用于自行编译。
>
> 官方固件下载地址：https://downloads.immortalwrt.org/
>
> 说明： 这里提供了 ImmortalWrt 的稳定版（如 24.10.4 系列）和开发版快照。

* KoolShare 固件

> KoolShare 曾经是国内最知名的路由器固件社区之一，其固件以集成“软件中心”方便安装插件而闻名。
>
> 项目定位： 曾经主要以论坛和官方网站发布，现在固件下载和讨论主要集中在相关社区和论坛。
>
> 寻找方式： 由于版权和网络环境变化，KoolShare 官方网站的固件下载渠道现在非常不稳定。建议您在路由器相关的技术论坛或贴吧（如恩山无线论坛）中搜索关键词 “KoolShare OpenWrt” 或 “KoolClash”，通常会有热心网友分享针对特定机型的编译版本或下载链接。

博主使用了一个23年收藏的一个版本，由于作者不允许共享，这里就不多说明了！虽然这么多版本，但是操作上差不了太多！

## 部署过程

### 创建虚拟机

1，Proxmox 新建虚拟机，名称随意，**注意 VM ID（后面会用到）**

点击【高级】，选中 【开机自启动】（要不然选择这个做旁路由的主机可能无法上网~）

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

2，操作系统，选择【不使用任何介质】

**因为OpenWrt不用ISO镜像安装，直接格式化成硬盘文件即可！**

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

3，磁盘保持默认设置即可（**因为后面会删除掉，不用担心浪费空间**）

CPU设置，按照自己的情况选择！如果只做旁路由2核 + 1G内存就足够了！

CPU权重，建议该值要比其他虚拟机高一些，数值越大优先级越高！

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

4，网络如图默认即可，然后提交保存即可

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

5，提交成功后，然后进行下一步

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

6，硬件 - 硬盘(scsi0) - 分离

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

7，移除【CD/DVD启动器】和 【硬盘(scsi0)】

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

### 上传镜像文件

访问 【local(pve)】 - 【ISO镜像】 - 上传 （选择童鞋下载的OpenWrt镜像文件）

**镜像文件名称需要留意下，后面会用到！**

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

### 导入镜像文件

点击 【pve】 - 【Shell】准备执行命令

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

以下命令得注意几个参数：

102  为 VM ID

xxxxxx.img 为上传的ISO镜像文件名称

替换下面的内容然后执行

```
qm importdisk 102 /var/lib/vz/template/iso/xxxxxx.img local-lvm
```

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

### 挂载硬盘

虚拟机控制台 - 硬件 - 未使用的磁盘 - 编辑

总线/设备  修改为 SATA

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

### 添加开机引导

按图将硬盘添加到引导启动中

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

### 配置OpenWRT

1，启动虚拟机后，可见控制台显示已经启动，但是IP没有

![](https://51.ruyo.net/wp-content/themes/CorePress-Pro/static/img/loading/doublering.svg)

2，控制台输入 vi /etc/config/network 修改 lan 节点的 IP为童鞋要设置的...