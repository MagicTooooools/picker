---
title: PVE 安装 Debian LXC 容器踩坑点以及与虚拟机的不同点
url: https://www.miaoer.net/posts/blog/install-debian-lxc-container-on-pve
source: 喵二の小博客
date: 2025-12-15
fetch_date: 2025-12-16T03:27:41.547610
---

# PVE 安装 Debian LXC 容器踩坑点以及与虚拟机的不同点

# PVE 安装 Debian LXC 容器踩坑点以及与虚拟机的不同点

# PVE 安装 Debian LXC 容器踩坑点以及与虚拟机的不同点

最近是因为内存大涨了吗，我的 PVE(Proxmox VE) 的 32G 也因为早期安装的 Debian 虚拟机太重了，应用多，也经过多轮调试最终不堪重负。

想体验迁移虚拟机 Debian 到 LXC ct 容器的 Debian，这样可以获得内存的自由调节足够的轻量等……

这里给大家做个省流：

为什么选择 Debian LXC 容器：

* **资源弹性**，内存自由，随便调没有虚拟机这么难回收
* **性能好**，运行效率最接近宿主机，损耗低
* **开启快**，秒级开机
* **易挂载**，搞挂了系统也可以通过一个命令挂载提取文件

不太推荐这些使用环境体验 Debian LXC 容器：

* 需要直通设备或特殊硬件，不推荐，部分硬件依赖内核模块的支持
* 需要运行特殊应用（不可信），强隔离等场景
* 想深入学习 Linux 相关的，LXC 无引导内核分区文件系统，相对虚拟机来说局限，在体验完完整 Linux 后可以尝试

## 配置

在磁盘中下载 Debian 12 的 LXC 容器模板

![](https://cdn.miaoer.net/blog/25-12-15/2025-12-15-19-52-00.webp)

设置好 ID，IPv4，IPv6，硬盘，Password，DNS server 等……

配置好后先别着急启动，我们准备进入 PVE 的 SSH 终端，

使用 nano 或 vim 修改 /etc/pve/lxc/ID.conf 例如我这里设的 ID: 233

将无特权容器从 1 修改为 `0`

`nano /etc/pve/lxc/233.conf`

CONF

```
...
arch: amd64
cores: 12
cpuunits: 80
features: mount=nfs,nesting=1
...
unprivileged: 0
```

Copy

然后再回去 PVE 后台将功能进行编辑，确定嵌套，NFS，SMB/CIFS，打开着。（我这不需要挂载 SMB,NFS 在 Linux 下更优秀）

这边建议配置好这些之前和之后都不要去修改特权和这些功能，避免遇到难以解决的问题。

![](https://cdn.miaoer.net/blog/25-12-15/2025-12-15-19-48-37.webp)

## 初始化

通过 PVE 的终端连入 Debian，我这里先进行更新软件包索引，再进行换源到物理距离最近最快的大学源，避免安装等太久，再然后安装 1Panel（docker）

<https://supermanito.github.io/LinuxMirrors/use/>

<https://1panel.cn/>

需要分次执行排查问题！

```
apt update

apt install curl

bash <(curl -sSL https://linuxmirrors.cn/main.sh) --edu

bash -c "$(curl -sSL https://resource.fit2cloud.com/1panel/package/v2/quick_start.sh)"
```

Copy

## 挂载 NAS

我这里在功能里面选择了 NFS 直接执行命令进行挂载测试，需要先创建挂载目录

```
mkdir -p /mnt/XXX       ## 挂载前先创建
mount -t nfs 192.168.23.33:/mnt/Pool/XXX /mnt/XXX       ## 挂载测试
```

Copy

执行命令可以通过，我们可以写一个 Systemd 服务（这里你们可以按照 AI 的来），让他启动的时候就自动挂载，因为 NAS 系统也是运行在 PVE 的所以还有加上启动延迟避免 immich & qB 比 NAS 服务器先启动，所带来的问题。

```
sudo tee /etc/systemd/system/nas-mount.service <<'EOF'
[Unit]
Description=Mount NFS Share from 192.168.23.33:/mnt/Pool/XXX
Requires=network-online.target rpcbind.service nfs-client.target
After=network-online.target rpcbind.service nfs-client.target
ConditionPathIsDirectory=/mnt/XXX

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/bin/mount -t nfs 10.0.0.2:/mnt/Pool/XXX /mnt/XXX -o vers=4,hard,intr,noatime,rsize=8192,wsize=8192
ExecStop=/bin/umount /mnt/XXX
TimeoutStartSec=30
TimeoutStopSec=30

[Install]
WantedBy=multi-user.target
EOF
```

Copy

BASH

```
sudo systemctl daemon-reload
sudo systemctl start nas-mount.service
systemctl status nas-mount.service
```

Copy

## 迁移和备份

我这里保存了虚拟机的 compose 文件夹，直接导入到新的 Debian 里面保存运行就可以了，但是像 immich 这种我已经经过了多轮备份，和迁移也算是损失了几张别人通过互传分享的照片作为代价，以及踩了一天的嵌套特权容器的沉默成本。目前还在调试 LXC 的这个 Debian……还需要优化一下……

愿你不会出现 `Condition: ConditionPathIsDirectory=/mnt/XXX was not met`