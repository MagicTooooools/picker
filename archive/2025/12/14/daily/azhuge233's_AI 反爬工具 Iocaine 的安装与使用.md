---
title: AI 反爬工具 Iocaine 的安装与使用
url: https://azhuge233.com/ai-%e5%8f%8d%e7%88%ac%e5%b7%a5%e5%85%b7-iocaine-%e7%9a%84%e5%ae%89%e8%a3%85%e4%b8%8e%e4%bd%bf%e7%94%a8/
source: azhuge233's
date: 2025-12-14
fetch_date: 2025-12-15T03:30:55.091325
---

# AI 反爬工具 Iocaine 的安装与使用

[Skip to content](#content)

# Menu

* [主页](https://azhuge233.com)
* [站外链接](https://azhuge233.com/%E7%AB%99%E5%A4%96%E7%9F%A5%E8%AF%86%E6%B1%87%E6%80%BB/)
* 喜 +1
  + [Demo 频道](https://t.me/azhuge233_FreeGames)
  + [随缘 Key](https://azhuge233.com/plus-1/)
* 网页游戏
  + [2048](https://2048.azhuge233.com)
  + [Jump King](https://jumpking.azhuge233.com/)
  + [Snake](https://snake.azhuge233.com)
  + [Wordle Clone](https://wordle.azhuge233.com/)
* 工具
  + [应用下载链接](https://apps.azhuge233.com)
  + [CyberChef](https://cyberchef.azhuge233.com/)
  + [IT Tools](https://tools.azhuge233.com/)
  + [Omni Tools](https://omnitools.azhuge233.com/)
  + [鼠标双击检测](https://clicktest.azhuge233.com)
  + [Reverse Shell Generator](https://rvrshell.azhuge233.com/)
  + [RPG Maker 解密](https://rpgmvp.azhuge233.com)
* 图一乐
  + [NSFW](https://r18.azhuge233.com)
  + [烟花模拟器](https://fireworks.azhuge233.com/)
  + [The Matrix](http://thematrix.azhuge233.com)

Search for:

Search Submit

Search for:

Search Submit

[![](https://azhuge233.com/wp-content/uploads/2020/11/cropped-IMG_2518-150x150.jpg)](https://azhuge233.com/)

[azhuge233's](https://azhuge233.com/)

Hi

* [Github](https://github.com/azhuge233)
* [Steam](https://steamcommunity.com/id/azhuge233/)

Menu

* [主页](https://azhuge233.com)
* [站外链接](https://azhuge233.com/%E7%AB%99%E5%A4%96%E7%9F%A5%E8%AF%86%E6%B1%87%E6%80%BB/)
* 喜 +1
  + [Demo 频道](https://t.me/azhuge233_FreeGames)
  + [随缘 Key](https://azhuge233.com/plus-1/)
* 网页游戏
  + [2048](https://2048.azhuge233.com)
  + [Jump King](https://jumpking.azhuge233.com/)
  + [Snake](https://snake.azhuge233.com)
  + [Wordle Clone](https://wordle.azhuge233.com/)
* 工具
  + [应用下载链接](https://apps.azhuge233.com)
  + [CyberChef](https://cyberchef.azhuge233.com/)
  + [IT Tools](https://tools.azhuge233.com/)
  + [Omni Tools](https://omnitools.azhuge233.com/)
  + [鼠标双击检测](https://clicktest.azhuge233.com)
  + [Reverse Shell Generator](https://rvrshell.azhuge233.com/)
  + [RPG Maker 解密](https://rpgmvp.azhuge233.com)
* 图一乐
  + [NSFW](https://r18.azhuge233.com)
  + [烟花模拟器](https://fireworks.azhuge233.com/)
  + [The Matrix](http://thematrix.azhuge233.com)

Search...

![](https://azhuge233.com/wp-content/uploads/2025/12/iocaine-logo.png)

# AI 反爬工具 Iocaine 的安装与使用

[2025-12-142025-12-14](https://azhuge233.com/ai-%E5%8F%8D%E7%88%AC%E5%B7%A5%E5%85%B7-iocaine-%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E4%BD%BF%E7%94%A8/) [azhuge233](https://azhuge233.com/author/azhuge233/)

文章目录

Toggle

* [环境](#%E7%8E%AF%E5%A2%83)
* [参考](#%E5%8F%82%E8%80%83)
* [步骤](#%E6%AD%A5%E9%AA%A4)
  + [安装 Iocaine](#%E5%AE%89%E8%A3%85-Iocaine)
  + [配置 Iocaine](#%E9%85%8D%E7%BD%AE-Iocaine)
  + [配置 Nginx](#%E9%85%8D%E7%BD%AE-Nginx)
* [效果](#%E6%95%88%E6%9E%9C)
  + [相关](#%E7%9B%B8%E5%85%B3)

实现 AI 反爬的方式有很多种，Iocaine 内置了一套 AI 判断规则，如果判断出请求是 AI 爬虫发出的，则返回一堆垃圾内容，从长计议，让 AI 主动放弃访问网页内容

比较难绷的是，当前版本的 Iocaine 会将所有 PC Chromium 浏览器的非 HTTPS 请求判断为 AI 爬虫（仓库中的相应 [issue](https://git.madhouse-project.org/iocaine/iocaine/issues/101)）

所以推荐在 HTTPS 环境下使用

## 环境

* Debian 13

## 参考

* [Iocaine 文档](https://iocaine.madhouse-project.org/documentation/)
* [Iocaine 代码仓库](https://git.madhouse-project.org/iocaine/iocaine)
* [Guarding My Git Forge Against AI Scrapers](https://vulpinecitrus.info/blog/guarding-git-forge-ai-scrapers/)

## 步骤

以下步骤使用 Iocaine 提供的 Debian 包安装，使用 systemd 管理服务

Iocaine 也支持容器，只需做好端口和配置文件映射

### 安装 Iocaine

添加并更新 Iocaine 软件源

```
curl -s https://iocaine.madhouse-project.org/_/debian/iocaine.sources \
       -o /etc/apt/sources.list.d/iocaine.sources
apt update
```

安装 Iocaine

```
apt install iocaine
```

安装完毕后，systemd 会自动启动 iocaine，默认监听在 127.0.0.1:42069

![](https://azhuge233.com/wp-content/uploads/2025/12/Snipaste_2025-12-14_09-36-06-768x467.png)

使用 curl 指定 UA 测试 Iocaine，可以发现所有合法的请求会返回 421 状态码，而被判定为 AI 的请求会返回 200 状态码（返回了垃圾内容）

![](https://azhuge233.com/wp-content/uploads/2025/12/Snipaste_2025-12-14_09-40-39.png)

### 配置 Iocaine

可以在上图发现，Iocaine 在启动时输出了一条警告日志，提示没有配置 ai-robots-txt-path，ai-robots-txt-path 是 Iocaine AI 判断规则文件（一份包含了各个 AI 爬虫信息的 json 文件）的路径

Iocaine 内置了一份规则文件，不设置也可以正常运行，但是这份文件只会在 Iocaine 版本更新时更新，所以推荐自行下载并更新此文件

执行命令，下载新文件

```
curl -L https://github.com/ai-robots-txt/ai.robots.txt/raw/refs/heads/main/robots.json \
       -o /etc/iocaine/robots.json
```

编辑 Iocaine 配置文件 /etc/iocaine/config.kdl ，添加 ai-robots-txt-path

```
declare-handler default language=roto

// 将以上内容更改为以下内容

declare-handler default {
        ai-robots-txt-path "/etc/iocaine/robots.json"
}
```

![](https://azhuge233.com/wp-content/uploads/2025/12/Snipaste_2025-12-14_11-16-48.png)

重新启动 Iocaine

```
systemctl restart iocaine.service
```

可以发现警告日志消失

![](https://azhuge233.com/wp-content/uploads/2025/12/Snipaste_2025-12-14_10-01-57-768x310.png)

Iocaine 还支持设置 Prometheus 监控、日志等级、多个 server 和 handler 等功能，详细配置文件可以查看 [Configuring iocaine](https://iocaine.madhouse-project.org/documentation/3/configuration/)

### 配置 Nginx

之前在测试中发现，Iocaine 会对正常请求返回 421 状态码，所以接下来需要利用这点，将 Iocaine 添加到 Web 服务器中

我这里使用全新安装的 Nginx，使用默认的配置文件，仅作演示

编辑 /etc/nginx/sites-enabled/default，将默认的 server 块更改为以下内容

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name _;

        recursive_error_pages on;

        location / {
                proxy_cache off;
                proxy_intercept_errors on;
                proxy_set_header Host $host;
                proxy_pass http://127.0.0.1:42069;

                error_page 421 = @fallback;
        }

        location @fallback {
                root /var/www/html;
                index index.html index.htm index.nginx-debian.html;
                try_files $uri $uri/ =404;
        }
}
```

当 Nginx 接收到请求时，会先发给 Iocaine，让 Iocaine 判断请求是否为 AI：

* 如果是 AI 请求，则返回 Iocaine 生成的垃圾内容
* 如果不是 AI 请求，Iocaine 会返回 421 状态码，由于 421 错误页面通过 error\_page 跳转到了 @fallback （即正常页面），所以 Nginx 会返回正常内容，请求方获得的内容也是正常的

但是，当前版本的 Iocaine 在 HTTP 请求方面存在误判，所以仅推荐在 HTTPS 环境下使用

下图使用了 Edge 浏览器的默认 UA，可以看到删除 Chrome 相关字符后 Iocaine 就放行了请求

![](https://azhuge233.com/wp-content/uploads/2025/12/Snipaste_2025-12-14_10-39-26-768x169.png)

## 效果

![](https://azhuge233.com/wp-content/uploads/2025/12/Snipaste_2025-12-14_10-46-39-768x529.png)

![](https://azhuge233.com/wp-content/uploads/2025/12/Snipaste_2025-12-14_10-47-06-768x530.png)

### *相关*

[AI](https://azhuge233.com/tag/ai/), [Iocaine](https://azhuge233.com/tag/iocaine/), [爬虫](https://azhuge233.com/tag/%E7%88%AC%E8%99%AB/)

## 文章导航

[Previous Previous post: 鸿蒙 NEXT App Gallery 上架审核记录](https://azhuge233.com/%E9%B8%BF%E8%92%99-next-app-gallery-%E4%B8%8A%E6%9E%B6%E5%AE%A1%E6%A0%B8%E8%AE%B0%E5%BD%95/)

### 发表回复 [取消回复](/ai-%E5%8F%8D%E7%88%AC%E5%B7%A5%E5%85%B7-iocaine-%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E4%BD%BF%E7%94%A8/#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

Δ

这个站点使用 Akismet 来减少垃圾评论。[了解你的评论数据如何被处理](https://akismet.com/privacy/)。

## 博客统计

* 141,425 点击次数

### 年度归档

#### 2025 (16)

+ 12-14  [AI 反爬工具 Iocaine 的安装与使用](https://azhuge233.com/ai-%E5%8F%8D%E7%88%AC%E5%B7%A5%E5%85%B7-iocaine-%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E4%BD%BF%E7%94%A8/)
+ 10-30  [鸿蒙 NEXT App Gallery 上架审核记录](https://azhuge233.com/%E9%B8%BF%E8%92%99-next-app-gallery-%E4%B8%8A%E6%9E%B6%E5%AE%A1%E6%A0%B8%E8%AE%B0%E5%BD%95/)
+ 10-22  [OpenWrt (LEDE Lean) 编译记录](https://azhuge233.com/openwrt-lede-lean-%E7%BC%96%E8%AF%91%E8%AE%B0%E5%BD%95/)
+ 10-06  [Cloudflare Browser Rendering 的基本使用](https://azhuge233.com/cloudflare-browser-rendering-%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/)
+ 09-10  [服务器监控平台 Beszel 自托管](https://azhuge233.com/%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9B%91%E6%8E%A7%E5%B9%B3%E5%8F%B0-beszel-%E8%87%AA%E6%89%98%E7%AE%A1/)
+ 08-17  [PS4 BD-JB 蓝光光盘破解](https://azhuge233.com/ps4-bd-jb-%E8%93%9D%E5%85%89%E5%85%89%E7%9B%98%E7%A0%B4%E8%A7%A3/)
+ 08-06  [解决 PVE 8 升级 9 后 local-lvm (pve/data) 无法使用问题](https://azhuge233.com/%E8%A7%A3%E5%86%B3-pve-8-%E5%8D%87%E7%BA%A7-9-%E5%90%8E-local-lvm-pve-data-%E6%97%A0%E6%B3%95%E4%BD%BF%E7%94%A8%E9%97%AE%E9%A2%98/)
+ 07-19  [铭凡 MinisForum UM690S 主板拆卸](https://azhuge233.com/%E9%93%AD%E5%87%A1-minisforum-um6...