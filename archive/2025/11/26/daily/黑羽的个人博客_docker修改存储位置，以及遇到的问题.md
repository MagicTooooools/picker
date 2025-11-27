---
title: docker修改存储位置，以及遇到的问题
url: https://blog.thetbw.xyz/archives/change-docker-data-root
source: 黑羽的个人博客
date: 2025-11-26
fetch_date: 2025-11-27T16:52:40.176805
---

# docker修改存储位置，以及遇到的问题

* [首页](/)
* [摄影](//gallery.thetbw.xyz/)
* [分类](/categories)
* [归档](/archives)
* [随想](/journals)
* [友链](/links)
* [关于](/s/about)
* [求职中](/s/resume)
* [微信公众号](/s/wechat-channel)
* [个人笔记](https://thetbw.github.io/note/#/)
* [虫洞](https://foreverblog.cn/go.html)

![docker修改存储位置，以及遇到的问题](https://thetbw-hk.cos.thetbw.xyz/blog/1a97aa4ec6dfea280b1adfa035a25749_1764144605508.jpg)

docker修改存储位置，以及遇到的问题

170 次访问

发布: 2025-11-26

最后编辑: 2025-11-26

[· 极客](/categories/geek)

默认docker存储都是放在 `/var/lib/docker` 这个目录下面，随着使用时间越来越大。你可以使用 `docker image prune` 来清理不用的镜像。
做为一个 NAS 玩家，我打算直接把docker文件夹存到 NAS 的存储上。
使用 SMB 挂载目录的话可能会遇到一些问题，因为 SMB 不支持所有的 `POSIX ` 标准，所以我是直接挂载了一个远程硬盘到虚拟机中。系统盘保持足够的小，方便后面备份和快照，而这个数据盘就不需要快照和备份了。
---
更改 docker 存储目录也是很简单。
首先关闭docker
```
service docker stop
```
更改docker配置
```
nano /etc/docker/daemon.json
```
添加data-root配置，类似下面这样，我是放到 `/mnt/docker\_data` 目录
```json
{
"data-root": "/mnt/docker\_data"
}
```
移动原来的docker数据
```shell
rsync -aHAX /var/lib/docker/ /mnt/docker\_data/docker/
# 或使用下面的直接移动:
# mv /var/lib/docker /mnt/docker\_data/
```
然后重启docker
```
service docker start
```
然后这样基本上就完了
---
如果只是这样也没什么好写的了，说下我遇到的其他问题。
之前我是一直用上面的方法，最近用这个方法的时候，用 KASM 拉取镜像的时候，还是提示空间不足，而且`df -h`显示的空间也不对，类似下面这样
> KASM 感觉还蛮有意思的，有兴趣的可以去搜搜，是一个云桌面平台，后面我摸熟了可能会写个文章介绍一下。
| Filesystem | Size | Used | Avail | Use% | Mounted on |
|-------------|------|------|-------|------|-------------------------------------------------|
| udev | 3.9G | 0 | 3.9G | 0% | /dev |
| tmpfs | 795M | 2.2M | 792M | 1% | /run |
| /dev/sda1 | 18G | 13G | 4.1G | 76% | / |
| tmpfs | 3.9G | 0 | 3.9G | 0% | /dev/shm |
| tmpfs | 5.0M | 0 | 5.0M | 0% | /run/lock |
| /dev/sda15 | 124M | 12M | 113M | 10% | /boot/efi |
| /dev/sdb1 | 492G | 3.3G | 463G | 1% | /mnt/docker\_data |
| overlay | 18G | 13G | 4.1G | 76% | /mnt/docker\_data/rootfs/overlayfs/xxx |
| tmpfs | 88M | 0 | 88M | 0% | /run/user/0 |
明明 `/mnt/docker\_data` 显示有 `492G` 空间，为什么 `docker` 的 `overlayfs` 显示和系统根分区一样是 `18G` 呢。
后面也是问了 AI 才知道，原来 `docker` 自身的存储和依赖的 `containerd` 可能是分开的。我看了下，的确系统有个 `containerd` 服务。
按照docker的指引，修改 `containerd` 存储路径。
```shell
nano /etc/containerd/config.toml
```
```toml
root = "/mnt/docker\_data/containerd"
state = "/run/containerd"
```
```
systemctl restart docker
```
---
结尾，如果有遇到相同问题的，恰好搜到我这个文章，可以作为一个思路。如果从零开始，刚好看到我这个文章，不建议照抄命令，还是说只能作为思路的参考。
另外上面的大多数命令，例如 `rsync -aHAX` ，发送给 AI ，他都可以给你解释每一个参数的用处，建议在网上看到的教程，都了解每个参数的含义了，再进行操作。
docker的不同版本，每个发行版，都是不一样的，例如 `docker compose`，有些发行版上是用 `docker compose` 命令，有些是 `docker-compose`，另外发行版自身的包管理器和官方的源，都是有不一样的，建议判断下具体的环境，还有时间，综合做出决定。

> **Copyright:**  采用 [知识共享署名4.0](https://creativecommons.org/licenses/by/4.0/) 国际许可协议进行许可
>
> **Links:**  </archives/change-docker-data-root>

[# 云服务器](/tags/%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8)
[# docker](/tags/docker)
[# NAS](/tags/nas)

---

上一篇

#### 无

![【摘抄】天堂与地狱](/themes/thetbw-xue/source/images/loading.svg)

[Powered
Halo](http://halo.run)

[Theme
Xue](https://github.com/thetbw/halo-theme-xue)

运行

用户

访问