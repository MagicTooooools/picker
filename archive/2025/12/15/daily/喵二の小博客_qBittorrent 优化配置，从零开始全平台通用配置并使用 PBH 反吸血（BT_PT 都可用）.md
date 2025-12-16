---
title: qBittorrent 优化配置，从零开始全平台通用配置并使用 PBH 反吸血（BT/PT 都可用）
url: https://www.miaoer.net/posts/blog/qbittorrent
source: 喵二の小博客
date: 2025-12-15
fetch_date: 2025-12-16T03:27:44.615507
---

# qBittorrent 优化配置，从零开始全平台通用配置并使用 PBH 反吸血（BT/PT 都可用）

# qBittorrent 优化配置，从零开始全平台通用配置并使用 PBH 反吸血（BT/PT 都可用）

# qBittorrent 优化配置，从零开始全平台通用配置并使用 PBH 反吸血（BT/PT 都可用）

因为最近又想折腾 NAS 了，给 PVE 搞了个 fnOS 飞牛影视挂载到 SMB（注：NFS有问题）看大片，qB 和 PBH 则安装在给了特权的 Debian LXC 容器上使用了 1Panel 进行配置，那么一个好的 BT/PT 下载器就很有必要了。

当然本博客上的 qB 配置上也适合 PT，不过 PT 不需要考虑 qB 增强版和 PBH 的配置，请忽略！

部分操作我会进行简化不会讲的太细，部分内容结合网上的整理，总结得出，并不一定是正确的。

![](https://cdn.miaoer.net/blog/25-12-15/qb_banner.webp)

以下 transmission 简称 TR 或 tr，
qBittorrent 简称 qB 或 qb，
PeerBanHelper 简称 PBH，
qBittorrent-Enhanced-Edition 简称 qB 增强版，qbee。

这里指的全平台统一是指配置部分会根据 OS 不同而异，可以大致参考一下。

参考：

<https://blog.baiiylu.org/posts/2023/qBittorrent_CIFS_IO_Performance_Issues>

<https://www.chiphell.com/forum.php?mod=viewthread&tid=2513842&extra=page%3D1&ordertype=2&mobile=no>

## 安装

安装这里大同小异，qB 是全平台的软件基于 QT 跨平台构建，也有 Docker 版。

### 1Panel

1Panel 直接在商店下载即可，切记需要编辑以下 compose 文件，避免无法利用主机 IPv6。

非 1panel 的可以根据 <https://hub.docker.com/r/linuxserver/qbittorrent> 的自诉文件中的 compose 文件进行参考，也是可以使用的。

将 1panel-network 段删除，将 network 改成 `network_mode: host` 如下

YAML

```
services:
    qbittorrent:
        container_name: ${CONTAINER_NAME}
        deploy:
            resources:
                limits:
                    cpus: ${CPUS}
                    memory: ${MEMORY_LIMIT}
        environment:
            - PUID=1000
            - PGID=1000
            - UMASK_SET=022
            - TZ=${TIME_ZONE}
            - WEBUI_PORT=${PANEL_APP_PORT_HTTP}
            - TORRENTING_PORT=${PANEL_TORRENTING_PORT}
        image: linuxserver/qbittorrent:5.1.4
        labels:
            createdBy: Apps
        network_mode: host
        restart: always
        volumes:
            - ./config:/config
            - ./data:/downloads
            - /mnt/XXX:/XXX
```

Copy

### Windows&MacOS

可以到[官网](https://www.qbittorrent.org/download)提供的 sourceforge 中进行下载。

Windows: <https://sourceforge.net/projects/qbittorrent/files/qbittorrent-win32/>

Mac: <https://sourceforge.net/projects/qbittorrent/files/qbittorrent-mac/qbittorrent-5.0.5/qbittorrent-5.0.5.dmg/download>

### Linux

可以到 flatpak 商店或各包管理器上进行下载。

## 增强版和 PBH

现在 BT 环境，也是越来越糟糕了，~~出现了很多搞刷流的，把上下行流量拉平以防止被运营商检测，~~

对于迅雷 qBee 增强版依然可以拦截，但是对其他行为并无太多用处，
需要使用 [PeerBanHelper](https://docs.pbh-btn.com) 以下简称 PBH，
当然如果你之前使用的增强版也可以加上 PBH 来优化反吸血。

![](https://cdn.miaoer.net/blog/25-12-15/2025-12-15-17-08-50.webp)

缺点就是 PBH 规则强大由Java 编写的，会相对额外占用一些内存，如果你不想被运营商制裁，这似乎别无选择。

---

我们通过 [安装部署 | PeerBanHelper](https://docs.pbh-btn.com/docs/category/%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2) 进行安装即可

在安装完成后运行，再记住初始化的 Token，如果没找到的话可以在配置文件里面寻找到，这里不再赘述。

这个 Token 比较长，登录的时候用到，可以用密码管理器（chrome 自带的也可以）记住。

进到 PBH 里面只需要做几件事

1. 绑定下载器

这里下载器类型选择好 qb

名字随意

地址这里填写 <http://127.0.0.1:8181> 你 qb 运行的服务器客户端的 IP 地址，
因为 PBH 是和 qB 运行在一个机器上的，这里的端口我未进行更改 8181 就是默认端口

再输入 qB WebUI 的用户&密码

2. 绑定 BTN sparkle（星火） 平台

[文档](https://docs.pbh-btn.com/docs/category/btn-%E5%A8%81%E8%83%81%E9%98%B2%E6%8A%A4%E7%BD%91%E7%BB%9C)

我们进入官网 [Sparkle (花火) - BTN Instance](https://sparkle.pbh-btn.com/) 登录上 Github 并创建好用户应用

得到两串 UUID，分别填写进 PBH “设置” -> “基础设置” 中即可，这里文档填写的很详细，可以直接参考这里简单带过。

---

## 配置优化

这里演示与 WeiUI 部分设置会因 OS 不同，版本号差别而不同，每个设置并非适合你的最佳值，请作为参考。

### Vuetorrent

> 之前的博客介绍过 Vuetorrent 的配置，非 WebUI 用户不需要操作。

[^1](#1)

启用彩色的分享率：启用后比较好看

分页大小：改为 `无限滚动`

qBittorrent API 刷新：文件刷新间隔可以不更改

语言：设为 `简体中文`

### 行为

启用日志文件：默认启用，开启可能会增加机器负担（硬盘读写），但也更好排查错误不建议关闭，除非你的环境相对稳定不出错。

记录性能警告：默认关闭，可以开启排查性能问题。

### 下载

#### 添加种子时

种子内容布局：默认原始布局，如果你经常下载爱分类，建议选为 `创建子文件夹` 这样会相对下载目录来说会舒服点，但是可能需要多按几次鼠标，对刮削不影响～

如果种子已存在，则合并 Tracker：默认关闭，可以打开，字面意思适合需要更新 Tracker 的。

为所有文件预分配磁盘空间：开启后可以避免缓解磁盘碎片，与文件系统和分享协议相关，可以开启

为未完成的文件添加 .!qB 后缀名：可以避免下载时被刮削，但是也会影响边下边播

> 如果你需要边下边播，需要在下载时勾选 `顺序下载` & `首尾优先级`

#### 保存管理

> 需要按照 compose 文件中映射的路径或本机下载路径进行设置

默认保存路径（完成时）：设为你的下载库中，需要根据你的习惯和路径进行选择。

将已完成下载的 .torrent 文件复制到：设为你的需要保存的文件目录，建议开启，
可以保留相对完整的种子文件，即便你删除了下载文件的本体依然可以通过种子下载回来，而不是磁力链接（相对更快）。

### 速度

全局速率限制&备用速率限制：可以根据你的需要进行限制，不建议将上传设为较低值，可能违背 BT 下载精神。

定期可以根据你的需要进行定时限制

通过合理使用设置一个合理的数值可以做到正常使用或避免高峰期用网时卡顿两不误。

> 和速度相关的还有高级功能中的连接数

#### 速率限制设置

全选，默认只有 将速率限制应用于 µTP 协议 & 将速率限制应用于本地网络上的用户 开启。

### BITTORRENT

#### 隐私

BT 下载全开，PT 下载全关，BT/PT 混合下载全开（不建议混合）

> PT 是通过 tracker 寻找发现用户的，所以不需要开启，BT 需要开启是因为可以发现更多用户

加密模式：`允许加密`

最大校验种子数：默认 `1`，建议保持默认，本地下载到 SSD 硬盘或全闪 NAS 可以更改。

#### 种子队列

默认开启，建议`关闭`，如果你知道你在做什么才开启。

#### 做种限制

默认关闭，BT 热门种子可以到一定时间或分享率进行限制，但这样也会影响到后续下载的用户，可能违背 BT 下载精神。

#### 自动将这些 Tracker 添加到新下载

默认禁用，建议开启并填写，这些都是精心收集的独立或集合 Tracker：

```
https://tracker.adysec.com/announce
https://tracker.ghostchu-services.top/announce
```

Copy

开启后更容易发现用户

### 自动添加 Tracker 列表

Automatically append trackers from URL to new downloads

默认禁用，建议开启并填写：

```
https://cf.trackerslist.com/all.txt
```

Copy

开启后更容易发现用户

## RSS

RSS 可以完成自动追番下载或自动下免费包，根据实际填写，或寻找教程例如 <https://www.bilibili.com/video/BV1dbxfeEEn3/>

## WEBUI

> 如果你是桌面系统（Windows/MacOS/Linus）客户端，且不需要 PBH 不需要操作

这里监听所有的 IP，默认星号 `*`，端口默认 `8181` 老版本默认 `8080` 如你喜欢可以更改为其他，更改后 PBH 需要重新进行绑定

### 身份验证

默认 `admin`，密码可以更具自己需要进行修改，如果你通过 docker compose（1panel）进行安装并且通过日志拿到临时密码成功登录，请务必修改，更改后 PBH 需要重新进行绑定。

不建议开启跳过认证

## 标签和分类

看个人习惯，玩 PT 的可以打个标签和分类标记你下载的片，略过。

## 高级

![](https://cdn.miaoer.net/blog/25-12-15/2025-12-15-17-05-10.webp)

### qBittorrent 部分

物理内存：默认 `512` Mib，保持默认，此项不太影响。

.torrent 文件大小：默认 `100` Mib，保持默认，没遇到不能导入的。

完成后重新校验种子：默认关闭，一般用不到除非你数据存储的不好，比如 U 盘和长时间不开机的电脑（特别是 SSD）。

界面刷新间隔：默认 `1500` ms，保持默认够用了。

解析用户所在国家：默认 `开启`，保持默认。

当 IP 或端口变更时重新向所有 Tracker 汇报：默认关闭，建议 `开启`，开启后会允许重新汇报。

#### 网络

网络接口&绑定的可选 IP 地址，保持默认，除非你知道你在干什么。

#### 内置 Tracker

字面意思，拿你的电脑当 Tracker 服务器，不需要开启，除非你需要发布种子，才可能需要开而且也能使用公共的 Tracker。

为下载的文件启用网络标记 (MOTW)：默认开启，同上。

#### python

一般用不上，除非你知道自己在做什么。

### libtorrent 部分

#### 线程

异步 I/O 线程：默认 `10`，建议改为 CPU 核心数的 1/2 或 1/3 保证能正常使用电脑流畅我这设的是 `4`，可以这么理解线程越低资源占用约少，相对的软件也没这么流畅。

哈希校验线程数 (libtorrent >= 2.0)：默认 `1` ，建议保持默认，除非你是 SSD 或全闪 NAS。

文件池大小：默认 `100`，建议保持默认，如果你有更好的性能可以改为不大于 300，超过可能会导致内存泄漏导致程序崩溃。

校验时内存使用扩增量：默认忘了可能在 100 左右，建议 `1024` Mib，调大一点以便快速恢复种子进度。

#### 磁盘

磁盘缓存 (libtorrent < 2.0)：默认 `-1`，建议保持默认，则自动。

磁盘缓存过期时间间隔 (libtorrent < 2.0)：默认 60，建议保持默认。

磁盘队列大小：默认 1024，建议 `62500`，细调具体看情况。

磁盘 IO 类型 (libtorrent >= 2.0; 需要重启)：默认 `默认`，建议保持默认

磁盘 IO 读取模式：默认 `启用系统缓存`，建议保持默认。

启用系统缓存：默认 `启用系统缓存`，建议保持默认。

合并读写 (libtorrent < 2.0)：默认关闭，建议 `开启`，优化性能。

启用相连文件块下载模式：默认 `关闭`，建议保持默认，单独设置即可。

发送分块上传建议：默认开启，建议保持默认。

下限上限大小略……无修改。

每秒传出连接数：默认 `30`，建议保持默认，目前没有遇到瓶颈。

#### 网络

μTP-TCP 混合模式策略，建议 `优先使用 TCP`。

#### 安全

启用国际化域名 (IDN) 支持：默认关闭，建议 `启用`。

允许来自同一 IP 地址的多个连接：默认关闭，可能会被吸血模拟，PBH 可能会关闭此选项。

验证 HTTPS Tracker 证书：默认开启，建议保持默认。

服务器端请求伪造 (SSRF) 缓解：默认开启，建议保持默认。

禁止连接到特权端口上的 Peer：默认关闭，建议保持默认。

上传窗口策略：默认 `固定窗口数`。

上传连接策略：默认 `最快上传`。

总是向同级的所有 Tracker 汇报，忘了，建议 `开启`。

总是向所有等级的 Tracker 汇报，忘了，建议 `开启`。

最大并行 HTTP 发布数：默认 `50`，建议保持默认，如果你知道你在做什么。

Peer 进出断开百分比，默认 `4` %。

Peer 进出阈值百分比，默认 `90` %。

Peer 进出断开间隔，默认 `300` s。

单一 Peer 的最大未完成请求，默认 `500`。

DHT 引导节点：默认 `dht.libtorrent.org:25401, dht.transmissionbt.com:6881, router.bittorrent.com:6881`，建议保持默认。

I2P 略过……未知

## PT

PT 的话不需要做前面的增强版&PBH 的操作，而且这里推荐是使用 qBittoorent 下载，transmission 保种。

因为 qB 他比较占内存，当然他占内存也不是白吃你的，比如对于缓存有优化，上面对 qB 配置就是缓解或优化 qB 的配置。

TR 的话，占用内存特别小，CPU 占用也很小，但是功能相比 qB 少太多了！
如果是种子长期读写，qB 的缓存能不能对硬盘友好还不知道，但 TR 一定不会太友好，在 TR 中没有缓存相关的选项。

---

1: <https://www.miaoer.net/posts/blog/1panel-vuetorrent>