---
title: 介绍下我自托管的一些东西
url: https://www.bboy.app/2025/12/04/%E4%BB%8B%E7%BB%8D%E4%B8%8B%E6%88%91%E8%87%AA%E6%89%98%E7%AE%A1%E7%9A%84%E4%B8%80%E4%BA%9B%E4%B8%9C%E8%A5%BF/
source: Bboysoul's Blog
date: 2025-12-04
fetch_date: 2025-12-05T03:21:07.679344
---

# 介绍下我自托管的一些东西

[![](/oss/head.webp)](/)

# [Bboysoul's Blog](/)

|  |  |  |  |
| --- | --- | --- | --- |
| [首页](/) | [公告](/gonggao.html) | [项目](/projects.html) | [RSS](https://www.bboy.app/atom.xml) |

---

⬇️⬇️⬇️ 欢迎关注我的 telegram 频道和 twitter ⬇️⬇️⬇️

---

联系方式:
[Twitter](https://twitter.com/bboysoulcn)
[Github](https://github.com/bboysoulcn)
[Email](/cdn-cgi/l/email-protection#167474796f6579637a567474796f38776666)
[Telegram](https://t.me/bboyapp)

---

# 介绍下我自托管的一些东西

December 4, 2025
本文有 2773 个字
需要花费 6 分钟阅读

![](/oss/20251204-1.webp)

## 简介

经过多年的自托管实践，我逐渐形成了一套稳定可靠的部署方案。本文主要介绍云上部署的部分，云下环境主要用于备份和学习测试。整个架构遵循以下核心原则：

* **简单至上**：避免过度设计，能用简单方案就不用复杂方案
* **可靠性优先**：稳定运行比花哨功能更重要
* **定期维护**：每周五统一更新所有服务到最新版本
* **单机部署**：不追求高可用，专注于可靠性和快速恢复
* **每日备份**：自动化备份，多地冗余存储
* **快速恢复**：确保服务可以在短时间内从备份恢复

## 基础架构

### 系统选择

底层系统使用 Ubuntu，所有服务都运行在 k3s 之上。选择 k3s 的原因很简单：

1. **轻量级**：相比完整的 Kubernetes，k3s 更适合单机部署
2. **配置简单**：一个配置文件就能在新服务器上快速复制相同环境
3. **成熟稳定**：经过充分验证，适合生产环境使用

### K3s 配置

```
cluster-init: true
docker: false
data-dir: /data/k3s
disable:
  - traefik
  - servicelb
  - metrics-server
token: xxxxxxxxxxxxxxxxxx
service-node-port-range: 79-30124
kubelet-arg:
  - cgroup-driver=systemd
kube-proxy-arg:
  - proxy-mode=ipvs
  - ipvs-strict-arp=true
disable-cloud-controller: true
tls-san: xxxxxxxxxxxxxxx
default-local-storage-path: /data/storage
etcd-snapshot-schedule-cron: 0 */5 * * *
etcd-snapshot-retention: 20
etcd-snapshot-dir: /data/storage/etcd
```

这个配置已经稳定运行多年，几乎不需要改动。几个关键点：

**数据目录统一管理**
所有重要数据都存放在 `/data` 目录下，这样做有几个好处：

* 一眼就能看清系统有哪些数据
* 备份时只需关注这一个目录
* 迁移服务器时更加方便

**存储方案**
使用 k3s 内置的 `local-path` StorageClass，通过 `default-local-storage-path: /data/storage` 指定 PV 数据路径。这是最简单的存储方案，非常适合单机部署。

**最终目录结构**
处理完成后，`/data` 下只有两个核心目录需要关注：

* `/data/k3s` - k3s 集群数据
* `/data/storage` - 所有 PV 持久化数据

## 部署与运维

### GitOps 自动化部署

所有服务都通过 ArgoCD 进行部署管理，包括 ArgoCD 自身（除了首次安装）。所有 Kubernetes YAML 配置都存放在自建的 Gitea 仓库中，完全遵循 GitOps 理念。

**优势：**

* 配置即代码，所有变更都有记录
* 自动同步，推送到 Git 即可自动部署
* 易于回滚，出问题直接回退 Git 提交

### 流量入口与反向代理

**技术栈演进**
最初使用 Ingress-Nginx，后来切换到 Traefik。Traefik 性能明显更好，配置也更简洁。

**网络架构**

```
外网流量 → Cloudflare CDN → Traefik (hostport) → 后端服务
```

* **Traefik**：使用 hostport 模式，单节点部署足够简单高效
* **Cloudflare**：作为第一层防护，自动过滤恶意流量和攻击，可以直接在 Cloudflare 层面封禁恶意 IP

### 数据库选型

**从 MySQL 迁移到 PostgreSQL**
数据库是自托管服务的核心。最初使用 MySQL，但性能不够理想。后来将所有服务迁移到 PostgreSQL，几乎不需要修改代码，性能就有了肉眼可见的提升。

**版本演进**
从 PostgreSQL 14 升级到 18，对于我的使用场景性能提升不明显，但保持新版本有助于安全性和稳定性。

**数据存储方案**

* **PostgreSQL**：所有需要关系型数据库的服务
* **Redis**：缓存和队列

**部署原则**：如果一个项目不支持 PostgreSQL，我宁可不部署。维护多个数据库系统会大大增加运维复杂度。

## 服务清单

> **Namespace 管理策略**：所有服务都部署在 `app` namespace 下，因为大部分都是单 Pod 服务，没必要创建多个 namespace 增加管理复杂度。

### 开发与代码管理

* **Gitea** - 自建 Git 服务，管理所有代码和配置，包括部署项目的流水线
* **Wakapi** - 编码时间统计，了解自己的工作习惯
* **Registry** - Docker 私有镜像仓库

### 博客与内容

* **Umami** - 博客访问统计分析
* **Umami API** - 自研项目，定时抓取访问量缓存到 Redis，提升博客加载速度并减轻 Umami 压力
* **Waline** - 博客评论系统

### 效率与工具

* **Atuin** - Shell 历史命令同步，跨设备使用
* **Memos** - 轻量级笔记，记录临时想法，虽然我平时不怎么记录笔记
* **Linkwarden** - 稍后阅读管理，收藏有价值的链接
* **FreshRSS** - RSS 订阅聚合

### AI 与自动化

* **Open WebUI** - ChatGPT 客户端，统一的 AI 交互界面
* **One-Hub** - API 聚合网关，为 Open WebUI 提供多源 API 支持
* **MCPO** - 让 Open WebUI 支持 MCP 协议

### 监控与运维

* **Telemonitor** <https://github.com/bboysoulcn/telemonitor> - 自己写的 Telegram 机器人实时监控服务器，比 Prometheus + Grafana 更轻量。对于自托管场景，CPU、内存、磁盘信息足矣
* **AdGuard Home** - DNS 服务器，提供 DoH 加密查询
* **TGPush** - 自动推送新闻到 Telegram 频道，看到这里顺便可以关注下我的频道 <https://t.me/bboyapp>

### 安全与认证

* **Bitwarden** - 密码管理器
* **PocketID** - 统一身份认证中心，实现单点登录，目前大部分东西都已经接入了，少了很多输入密码的麻烦，很推荐

### 基础设施

* **PostgreSQL** - 主数据库
* **Redis** - 缓存与队列
* **PGBackWeb** - PostgreSQL 备份管理，没有整库备份，但是足够使用

### 其他服务

* **HubProxy** - GitHub 和 Docker Hub 代理加速
* **GeoIP** - 即 realip.cc，IP 地理位置查询
* **Wallos** - 订阅服务管理，追踪各种付费订阅
* **Paperless-ngx** - 文档管理系统，虽然不常用但有需要时很方便

## 备份策略

备份是自托管最重要的环节。我的备份方案追求简单可靠，确保数据安全和快速恢复。

### 定时任务

每天凌晨 1 点自动执行备份：

```
0 1 * * * cd /backup && bash main.sh
```

### 备份脚本

```
#!/bin/bash
set -e
time=$(date "+%Y-%m-%d")-$RANDOM
pg_filename=$time-pg.sql

echo "开始备份 Gitea"
k3s kubectl exec -it gitea-0 -n app -- su - git -c '/app/gitea/gitea dump -c /data/gitea/conf/app.ini --skip-log --skip-package-data'

echo "开始备份 PostgreSQL"
cd /data/storage/pgbackup && export PGPASSWORD="xxxxxxxx" && pg_dumpall -h 127.0.0.1 -p 5432 -U postgres -w > $pg_filename

path=/data/storage
echo "开始备份" $path
dirname=`dirname $path`
basename=`basename $path`
start=$(date +%s)

# 创建备份目录
backupdir=/backup/$basename/$time
mkdir -p $backupdir
cd $dirname

# 使用 zstd 高压缩率打包
tar -cvf - $basename | zstd -15 -T16 > $backupdir/$basename-$time.tar.zst

# 切分为 1GB 的文件块，方便传输和存储
split -d -b 1024m $backupdir/$basename-$time.tar.zst $backupdir/$basename-$time.tar.zst.
rm -rf $backupdir/$basename-$time.tar.zst

end=$(date +%s)
take=$(( end - start ))
echo "$path 备份完成，耗时 $take 秒"

# 清理过期备份
cd /backup/ && bash delete.sh
cd /data/storage/pvc-c9d030dd-5657-45f9-89f3-c1be2e6bdae4_app_data-gitea-0/git && bash delete.sh
cd /data/storage/pgbackup && bash delete.sh

# 同步到异地服务器
rsync -pgoav --progress --delete /backup [email protected]:/backup
rsync -pgoav --progress --delete /backup [email protected]:/backup
```

### 备份流程说明

**1. 关键服务单独备份**

* **Gitea**：使用内置的 dump 命令导出完整数据
* **PostgreSQL**：使用 pg\_dumpall 全库备份（另外 PGBackWeb 也会独立备份一份）

**2. 整体数据打包**

* 使用 `zstd -15 -T16` 进行高压缩率打包（多线程加速）
* 将大文件切分为 1GB 的块，便于网络传输和存储管理

**3. 清理过期备份**
保留最近 3 天的备份，自动清理旧文件

**4. 多地异地备份**
通过 rsync 同步到两台异地服务器，实现地理冗余

### 清理脚本

自动删除 3 天前的备份文件：

```
#!/bin/bash

directory="/backup/storage"
current_time=$(date +%s)
THREE_DAYS=$(($current_time - 60*60*24*3))

for file in "$directory"/*; do
    file_time=$(stat -c %Y "$file")
    if [ "$file_time" -lt "$THREE_DAYS" ]; then
        rm -rf "$file"
        echo "Deleted: $file"
    fi
done
```

### 备份存储策略

采用 **3-2-1 备份原则**的变体：

1. **生产服务器**：实时数据 + 本地备份
2. **异地服务器 A**：备份文件同步（独立服务器）
3. **异地服务器 B**：备份文件同步（独立服务器）
4. **家庭 NAS**：定时从云端拉取备份

**共 4 份备份**，分布在不同地理位置，即使单点故障也能快速恢复。两台异地服务器专门用于存放备份，不运行其他服务，最大程度保证备份数据的安全性。

## 自托管心得

经过多年的实践，我总结了一些自托管的核心理念：

### 可靠性 > 高可用性

对于个人或小团队的自托管服务，**不需要追求高可用**，而要**追求高可靠**。

* 服务短暂宕机是可以接受的（用户主要是自己和家人）
* 但数据必须完整，系统必须能快速恢复
* 与其花精力搭建复杂的 HA 架构，不如把时间用在完善备份和恢复流程上

### 项目选型原则

部署前需要评估：

1. **社区活跃度**：GitHub Star 数量，Issue 处理速度
2. **长期维护性**：作者是否持续更新，项目是否成熟
3. **技术栈兼容**：是否支持 PostgreSQL（个人原则）
4. **避免玩具项目**：那些昙花一现的项目不值得投入精力

### 定期更新的意义

每周五统一更新所有服务，主要目的是：

* **安全性**：及时修复已知漏洞
* **稳定性**：获得 bug 修复
* 新功能反而是次要的

### 技术栈建议

**如果你不熟悉 Kubernetes**：

* 直接使用 **Docker + Docker Compose**
* 不要为了学习而强行使用 K8s
* Docker Compose 完全够用，而且更简单
* 一定要用 `docker-compose.yml` 管理配置，不要手动运行 `docker run`

**如果你熟悉 Kubernetes**：

* K3s 是很好的选择，轻量且功能完整
* 配合 GitOps 可以实现自动化运维

### 总结

自托管的本质是**为自己打造一套可控、可靠的数字基础设施**。保持简单，注重可靠性，做好备份，就能长期稳定运行。不要被花哨的技术栈迷惑，适合自己的才是最好的。

欢迎关注我的博客 [www.bboy.app](https://www.bboy.app/)

Have Fun

---

Tags:

* [Selfhost](https://www.bboy.app/tags/selfhost/);

* [简介](#简介)
* [基础架构](#基础架构)
  + [系统选择](#系统选择)
  + [K3s 配置](#k3s-配置)
* [部署与运维](#部署与运维)
  + [GitOps 自动化部署](#gitops-自动化部署)
  + [流量入口与反向代理](#流量入口与反向代理)
  + [数据库选型](#数据库选型)
* [服务清单](#服务清单)
  + [开发与代码管理](#开发与代码管理)
  + [博客与内容](#博客与内容)
  + [效率与工具](#效率与工具)
  + [AI 与自动化](#ai-与自动化)
  + [监控与运维](#监控与运维)
  + [安全与认证](#安全与认证)
  + [基础设施](#基础设施)
  + [其他服务](#其他服务)
* [备份策略](#备份策略)
  + [定时任务](#定时任务)
  + [备份脚本](#备份脚本)
  + [备份流程说明](#备份流程说明)
  + [清理脚本](#清理脚本)
  + [备份存储策略](#备份存储策略)
* [自托管心得](#自托管心得)
  + [可靠性 > 高可用性](#可靠性--高可用性)
  + [项目选型原则](#项目选型原则)
  + [定期更新的意义](#定期更新的意义)
  + [技术栈建议](#技术栈建议)
  + [总结](#总结)

---

|  |  |
| --- | --- |
| 本站总访问量  次 | 本站总访客数  人 |

2016 - 2025
Bboysoul