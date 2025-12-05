---
title: KASM，还不错的云桌面产品
url: https://blog.thetbw.xyz/archives/kasm-cloud-desktop
source: 黑羽的个人博客
date: 2025-12-04
fetch_date: 2025-12-05T03:21:08.606863
---

# KASM，还不错的云桌面产品

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

![KASM，还不错的云桌面产品](https://thetbw-hk.cos.thetbw.xyz/blog/DSC09306_1764854127860.jpg)

KASM，还不错的云桌面产品

98 次访问

发布: 2025-12-04

最后编辑: 2025-12-04

[· 极客](/categories/geek)

最近在这个叫 `KASM` 的玩意，这个很久之前就见过，感觉很复杂就没弄，最近短暂的有空，搭建了一个玩玩。
`KASM` 和核心功能来自一个叫 `KasmVNC` 的产品，这个是开源的，虽然名字里有 `VNC` 这两个字，但是他并不是传统的 `VNC` ，它比VNC有着更好的性能，支持GPU加速，直接剪贴板，文件上传下载等。
接触这个是因为之前了解的另一个项目，[docker-baseimage-gui](https://github.com/jlesage/docker-baseimage-gui)，一个预装了虚拟屏幕和 noVNC 的 docker 镜像，有一个应用是 [stardew-multiplayer-docker](https://github.com/printfuck/stardew-multiplayer-docker)，在docker上运行的星露谷物语，可以在浏览器上通过 VNC 控制。
想想还挺有意思，如果把程序都隔离到 docker 里面，通过浏览器来访问，这样既不用限定设备，局域网随意访问，内网穿透之后远程也可以访问了，岂不挺好，github 上也有了一些例如 docker-wechat 的项目，就是通过 docker 运行微信 linux 版，然后用 noVNC 在网页上显示，也可以实现微信多开了。
KASM 就是这样一个项目，他打包了很多常用的镜像（可惜并不是国内常用），然后有个管理平台，可以在用户需要的时候创建对应的 docker 容器，通过 web 界面访问；我想要折腾这种东西的一个出发点，就是有时候手上做到一半的东西，可以直接放在那，后面我换个电脑，可以接着继续开始，不用各种同步。
---
![image.png](https://thetbw-hk.cos.thetbw.xyz/blog/image\_1764852211270.png)
![image.png](https://thetbw-hk.cos.thetbw.xyz/blog/image\_1764852299477.png)
配合 PWA 还挺有模有样的。
![image.png](https://thetbw-hk.cos.thetbw.xyz/blog/image\_1764852400341.png)
还有支持游戏手柄，打印机，摄像头等穿透，就可以看出来和传统VNC的不同之处了。
![image.png](https://thetbw-hk.cos.thetbw.xyz/blog/image\_1764852716199.png)
后台就是基本的那一套功能了，添加自定义镜像源，管理用户可以访问的镜像，限制资源，让 docker 扩容等。不过要说的是，他这个后台并不是开源的，只是镜像本省是开源的，不过功能也是免费的就是了。
感觉也可以搞个云游戏之类的东西，把自己的电脑分享给朋友一起玩，之前b站上有过类似的视频，叫什么`多人一机`之类。
这些功能都是基于 web 的，其实不需要安装 kasm 这个后台也可以单独用docker 运行的，例如 https://hub.docker.com/u/kasmweb 仓库下，有很多 kasm 使用的镜像，里面有运行说明，也可以基于这些镜像定制，比如安装自己的软件之类的。
安装的话直接官网的教程就好了
https://docs.kasm.com/docs/install/single\_server\_install
不过要说的几点：
\* 最好单独一个机子上安装，不要有其他东西，因为安装过程中会安装一些docker 插件，还会重启 docker，避免发生一些兼容问题。
\* 最好单独从 docker 官网的源安装 docker，虽然安装脚本会自动安装docker，但是是从包管理器的，有些发行的包管理器内的 docker 太老了。
\* 可以在 wsl 上装，但是不支持 windows 的那个 docker desktop，需要wsl 内单独安装 docker
\* nvidia 的 gpu 加速会很麻烦
---
最后折腾这个的一个感想，就是 nvidia 的 gpu 真的是麻烦，想搞 linux 玩，还是乖乖用 amd 或者 intel 的显卡吧，用 nvidia 就是浪费生命。windows 也真是牛逼，搞了个 GPU-PV 的东西，可以不需要 GPU 虚拟化就把显卡给 WSL 用，不过也是扣扣搜搜，东西只给你一半，折腾起来也是费事，看到github 上有过一个项目 [Linux-GPU-V-Scripts-for-Hyper-V](https://github.com/Nislaco/Linux-GPU-V-Scripts-for-Hyper-V)，把 WSL 内的驱动复制到 Hyper-v 中，实现虚拟机内使用显卡，其实 WSL 也是一个特殊的虚拟机罢了。
最近有看了 intel 最近新出的显卡，`intel b50`，虽然性能不是很强，但是它的定位是专业卡，所有显卡特性基本上全支持了，而且支持虚拟化，不需要折腾任何东西，较新的 linux 内核直接内置驱动了，后面想搞一个玩玩，不过京东这 3k 块钱的售价，还是等什么时候便宜一下吧。
最后一直在折腾这种乱七八糟的小玩意，后面很长一段时间应该不会去折腾了，这是最近最后一个折腾分享了吧，希望是，还是得专注于有价值的事情上面（专心搞钱）。
> 其他：摄影项目因为服务器过期不打算续了，只是当初不知道它跑在那个服务器上，数据我都是定时备份的，后面找个时间迁移一下。

> **Copyright:**  采用 [知识共享署名4.0](https://creativecommons.org/licenses/by/4.0/) 国际许可协议进行许可
>
> **Links:**  </archives/kasm-cloud-desktop>

---

上一篇

#### 无

![docker修改存储位置，以及遇到的问题](/themes/thetbw-xue/source/images/loading.svg)

[Powered
Halo](http://halo.run)

[Theme
Xue](https://github.com/thetbw/halo-theme-xue)

运行

用户

访问