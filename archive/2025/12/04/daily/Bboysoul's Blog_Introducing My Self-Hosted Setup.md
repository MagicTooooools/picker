---
title: Introducing My Self-Hosted Setup
url: https://www.bboy.app/2025/12/04/introducing-my-self-hosted-setup/
source: Bboysoul's Blog
date: 2025-12-04
fetch_date: 2025-12-05T03:21:07.360700
---

# Introducing My Self-Hosted Setup

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
[Email](/cdn-cgi/l/email-protection#fb9999948288948e97bb99999482d59a8b8b)
[Telegram](https://t.me/bboyapp)

---

# Introducing My Self-Hosted Setup

December 4, 2025
本文有 1539 个字
需要花费 8 分钟阅读

![](/oss/20251204-1.webp)

## Introduction

After years of self-hosting practice, I’ve gradually formed a stable and reliable deployment solution. This article mainly introduces the cloud deployment part, while the on-premises environment is mainly used for backup and learning/testing. The entire architecture follows these core principles:

* **Simplicity First**: Avoid over-engineering, use simple solutions when possible
* **Reliability Priority**: Stable operation is more important than fancy features
* **Regular Maintenance**: Update all services to the latest version every Friday
* **Single-node Deployment**: Focus on reliability and fast recovery rather than high availability
* **Daily Backups**: Automated backups with multi-location redundancy
* **Fast Recovery**: Ensure services can be restored from backups in a short time

## Infrastructure

### System Choice

The underlying system uses Ubuntu, with all services running on k3s. The reasons for choosing k3s are simple:

1. **Lightweight**: Compared to full Kubernetes, k3s is more suitable for single-node deployment
2. **Simple Configuration**: A single config file can quickly replicate the same environment on a new server
3. **Mature and Stable**: Fully validated and suitable for production environments

### K3s Configuration

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

This configuration has been running stably for years and rarely needs modification. Several key points:

**Unified Data Directory Management**
All important data is stored in the `/data` directory, which has several benefits:

* Easy to see what data the system has at a glance
* Only need to focus on this one directory when backing up
* More convenient when migrating servers

**Storage Solution**
Uses k3s built-in `local-path` StorageClass, specifying PV data path via `default-local-storage-path: /data/storage`. This is the simplest storage solution, perfect for single-node deployments.

**Final Directory Structure**
After setup, there are only two core directories under `/data` to focus on:

* `/data/k3s` - k3s cluster data
* `/data/storage` - All PV persistent data

## Deployment and Operations

### GitOps Automated Deployment

All services are deployed and managed through ArgoCD, including ArgoCD itself (except for initial installation). All Kubernetes YAML configurations are stored in a self-hosted Gitea repository, fully following the GitOps philosophy.

**Advantages:**

* Configuration as code, all changes are recorded
* Auto-sync, push to Git and it automatically deploys
* Easy rollback, revert Git commits when issues occur

### Traffic Ingress and Reverse Proxy

**Technology Stack Evolution**
Initially used Ingress-Nginx, later switched to Traefik. Traefik has noticeably better performance and more concise configuration.

**Network Architecture**

```
External Traffic → Cloudflare CDN → Traefik (hostport) → Backend Services
```

* **Traefik**: Uses hostport mode, single-node deployment is simple and efficient enough
* **Cloudflare**: Acts as the first layer of protection, automatically filters malicious traffic and attacks, can block malicious IPs directly at the Cloudflare level

### Database Selection

**Migrating from MySQL to PostgreSQL**
The database is the core of self-hosted services. Initially used MySQL, but performance wasn’t ideal. Later migrated all services to PostgreSQL, with almost no code changes needed, and saw visible performance improvements.

**Version Evolution**
Upgraded from PostgreSQL 14 to 18. For my use case, the performance improvement isn’t obvious, but keeping up-to-date helps with security and stability.

**Data Storage Solution**

* **PostgreSQL**: All services requiring a relational database
* **Redis**: Caching and queues

**Deployment Principle**: If a project doesn’t support PostgreSQL, I’d rather not deploy it. Maintaining multiple database systems greatly increases operational complexity.

## Service List

> **Namespace Management Strategy**: All services are deployed in the `app` namespace, because most are single-pod services, so there’s no need to create multiple namespaces and increase management complexity.

### Development and Code Management

* **Gitea** - Self-hosted Git service, managing all code and configurations, including deployment project pipelines
* **Wakapi** - Coding time statistics, understanding work habits
* **Registry** - Private Docker image registry

### Blog and Content

* **Umami** - Blog analytics
* **Umami API** - Self-developed project, periodically fetches page views and caches to Redis, improving blog loading speed and reducing Umami pressure
* **Waline** - Blog comment system

### Productivity and Tools

* **Atuin** - Shell command history sync, cross-device usage
* **Memos** - Lightweight notes, recording temporary thoughts, though I don’t take many notes usually
* **Linkwarden** - Read-it-later management, bookmarking valuable links
* **FreshRSS** - RSS feed aggregation

### AI and Automation

* **Open WebUI** - ChatGPT client, unified AI interaction interface
* **One-Hub** - API aggregation gateway, providing multi-source API support for Open WebUI
* **MCPO** - Enables Open WebUI to support MCP protocol

### Monitoring and Operations

* **Telemonitor** <https://github.com/bboysoulcn/telemonitor> - Self-written Telegram bot for real-time server monitoring, more lightweight than Prometheus + Grafana. For self-hosting scenarios, CPU, memory, and disk info is sufficient
* **AdGuard Home** - DNS server, providing DoH encrypted queries
* **TGPush** - Automatically pushes news to Telegram channel, by the way you can follow my channel <https://t.me/bboyapp>

### Security and Authentication

* **Bitwarden** - Password manager
* **PocketID** - Unified identity authentication center, implementing single sign-on. Most things are already integrated, eliminating a lot of password typing hassle, highly recommended

### Infrastructure

* **PostgreSQL** - Primary database
* **Redis** - Caching and queues
* **PGBackWeb** - PostgreSQL backup management, doesn’t do full database backup, but sufficient for use

### Other Services

* **HubProxy** - GitHub and Docker Hub proxy acceleration
* **GeoIP** - aka realip.cc, IP geolocation query
* **Wallos** - Subscription service management, tracking various paid subscriptions
* **Paperless-ngx** - Document management system, though not frequently used, very convenient when needed

## Backup Strategy

Backup is the most important aspect of self-hosting. My backup solution pursues simplicity and reliability, ensuring data safety and fast recovery.

### Scheduled Tasks

Automatically executes backup every day at 1 AM:

```
0 1 * * * cd /backup && bash main.sh
```

### Backup Script

```
#!/bin/bash
set -e
time=$(date "+%Y-%m-%d")-$RANDOM
pg_filename=$time-pg.sql

echo "Starting Gitea backup"
k3s kubectl exec -it gitea-0 -n app -- su - git -c '/app/gitea/gitea dump -c /data/gitea/conf/app.ini --skip-log --skip-package-data'

echo "Starting PostgreSQL backup"
cd /data/storage/pgbackup && expo...