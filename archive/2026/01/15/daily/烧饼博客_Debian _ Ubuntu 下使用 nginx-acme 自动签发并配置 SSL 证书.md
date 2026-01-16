---
title: Debian / Ubuntu 下使用 nginx-acme 自动签发并配置 SSL 证书
url: https://u.sb/debian-nginx-acme/
source: 烧饼博客
date: 2026-01-15
fetch_date: 2026-01-16T03:35:00.406664
---

# Debian / Ubuntu 下使用 nginx-acme 自动签发并配置 SSL 证书

[烧饼博客](/)

[首页](/)[标签](/tags/)[归档](/archives/)[友链](/links/)[关于](/about/)[联系](/contact/)[捐赠](https://nm.sl/)[免责声明](/disclaimer/)

# Debian / Ubuntu 下使用 nginx-acme 自动签发并配置 SSL 证书

Jan 15th, 2026

本文将介绍在 Debian 或 Ubuntu 下使用 nginx-acme 自动签发并配置 SSL 证书的方法。

本文适合 Debian Stable 和 Ubuntu LTS，请使用 root 用户进行操作。

## 1、什么是 nginx-acme

[nginx-acme](https://github.com/nginx/nginx-acme) 是 Nginx 官方开发的基于 ACME 协议自动签发 SSL 证书的模块，隔壁 Caddy 都出了几百年的功能， Nginx 也终于赶上了。

和 Nginx 使用 C 语言开发不同，这个模块是用 Rust 语言开发的，这里就不做评价。

这个模块支持 [RFC8555](https://datatracker.ietf.org/doc/html/rfc8555)、[RFC8737](https://datatracker.ietf.org/doc/html/rfc8737)、[RFC8738](https://datatracker.ietf.org/doc/html/rfc8738) 和 [draft-ietf-acme-profiles](https://datatracker.ietf.org/doc/draft-ietf-acme-profiles/) 等规范，目前我实际测试下来已经基本可以用于生产环境。

## 2、安装 N.WTF

这里我们使用[烧饼博客](https://u.sb/)打包的 [N.WTF](https://n.wtf/)，这个项目已经集成了 `nginx-acme` 模块，可以做到开箱即用。

首先，安装一些必要的软件包：

```
sudo apt update
sudo apt upgrade -y
sudo apt install curl vim wget gnupg dpkg apt-transport-https lsb-release ca-certificates
```

然后加入 N.WTF 的 GPG 公钥和 apt 源：

```
curl -sSL https://n.wtf/public.key | sudo bash -c 'gpg --dearmor > /usr/share/keyrings/n.wtf.gpg'
sudo bash -c 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/n.wtf.gpg] https://mirror-cdn.xtom.com/sb/nginx/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/n.wtf.list'
```

国内机器可以用[清华 TUNA](https://mirrors.tuna.tsinghua.edu.cn/) 的国内源：

```
curl -sSL https://mirrors.tuna.tsinghua.edu.cn/n.wtf/public.key | sudo bash -c 'gpg --dearmor > /usr/share/keyrings/n.wtf.gpg'
sudo bash -c 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/n.wtf.gpg] https://mirrors.tuna.tsinghua.edu.cn/n.wtf/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/n.wtf.list'
```

Debian 下也可以直接使用 [extrepo](/debian-extrepo/):

```
sudo apt update
sudo apt install extrepo -y
sudo extrepo enable n.wtf
```

接着更新系统并安装 Nginx：

```
sudo apt update
sudo apt install nginx-extras -y
```

## 3、nginx-acme 准备工作

准备工作很简单，我们需要建立一个目录来存放 SSL 证书并给予正确的权限，这里我们以 `/var/cache/nginx/letsencrypt` 为演示：

```
sudo mkdir -p /var/cache/nginx
sudo mkdir -p /var/cache/nginx/letsencrypt
sudo chown 33:33 /var/cache/nginx -R
```

然后别忘了把域名解析到你的服务器哦！

## 3、配置 Nginx 站点

我们以 `example.com` 为例，假设你的邮箱是 `[[email protected]](/cdn-cgi/l/email-protection)`，需要配置的域名是 `example.com` 和 `www.example.com`，并且希望访问 `www.example.com` 跳转到 `example.com`：

直接修改 `/etc/nginx/sites-enabled/default` 文件：

```
resolver 8.8.8.8:53 ipv6=off valid=5s;

acme_issuer letsencrypt {
    uri         https://acme-v02.api.letsencrypt.org/directory;
    contact     [email protected];
    state_path  /var/cache/nginx/letsencrypt;
    accept_terms_of_service;
    ssl_trusted_certificate /etc/ssl/certs/ca-certificates.crt;
    ssl_verify off;
}

acme_shared_zone zone=ngx_acme_shared:1M;

server {
    # Listen on port 80 for all IPv4 and IPv6 addresses
    listen 80 default_server;
    listen [::]:80 default_server;

    # Match all domain names
    server_name _;

    location /.well-known/ {
        return 404;
    }

    location / {
        # Redirect all other HTTP requests to HTTPS using 301 permanent redirect
        return 301 https://$host$request_uri;
    }
}

server {
    # Standard TLS listening
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    # HTTP/2 protocol support
    http2 on;

    # HTTP/3 QUIC protocol support
    listen 443 quic reuseport;
    listen [::]:443 quic reuseport;
    add_header Alt-Svc 'h3=":443"; ma=86400' always;
    add_header X-Protocol $server_protocol always;

    server_name example.com;
    root /var/www/html;
    index index.html;

    # modern configuration
    ssl_protocols TLSv1.3;
    ssl_ecdh_curve X25519:prime256v1:secp384r1;
    ssl_prefer_server_ciphers off;

    acme_certificate letsencrypt;

    ssl_certificate       $acme_certificate;
    ssl_certificate_key   $acme_certificate_key;

    # do not parse the certificate on each request
    ssl_certificate_cache max=2;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;

    http2 on;

    listen 443 quic;
    listen [::]:443 quic;

    add_header Alt-Svc 'h3=":443"; ma=86400' always;
    add_header X-Protocol $server_protocol always;

    server_name www.example.com;
    return 301 https://example.com$request_uri;

    ssl_protocols TLSv1.3;
    ssl_ecdh_curve X25519:prime256v1:secp384r1;
    ssl_prefer_server_ciphers off;

    acme_certificate letsencrypt;

    ssl_certificate       $acme_certificate;
    ssl_certificate_key   $acme_certificate_key;

    # do not parse the certificate on each request
    ssl_certificate_cache max=2;
}
```

然后验证 Nginx 配置并重新加载：

```
sudo nginx -t
sudo nginx -s reload
```

此时，在默认的 Nginx 日志文件 `/var/log/nginx/access.log` 中可以看到 Let's Encrypt 验证服务器的请求记录：

```
23.178.112.210 - - [15/Jan/2026:16:08:18 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
34.212.137.78 - - [15/Jan/2026:16:08:18 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
18.222.179.58 - - [15/Jan/2026:16:08:18 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
16.171.19.61 - - [15/Jan/2026:16:08:18 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
13.228.72.222 - - [15/Jan/2026:16:08:18 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
2600:3000:2710:200::81 - - [15/Jan/2026:16:08:20 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
2600:1f14:804:fd02:f7fd:3c68:dec7:a062 - - [15/Jan/2026:16:08:20 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
2600:1f16:269:da00:c9ce:508c:ea69:9c2 - - [15/Jan/2026:16:08:20 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
2a05:d016:39f:3101:8b83:62b8:2603:d15d - - [15/Jan/2026:16:08:20 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
2406:da18:85:1401:1f69:967a:a988:cdc8 - - [15/Jan/2026:16:08:20 +0000] "GET /.well-known/acme-challenge/blablablablablablablablablablablablablablab HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)"
```

等待数秒后即可访问 `https://example.com/` 了。

如果要给 IP 地址签发证书，则需要在 `acme_issuer letsencrypt {}` 段里添加一行 `profile shortlived;`，并且 `server_name` 必须写入完整的 IP 地址，不能直接用 `server_name _` 哦。

不过目前 nginx-acme 最新版本 0.3.1 签发 IP 证书还是会失败，开发版已经修复，应该需要等下一个版本才能使用。

读者们在使用过程中如果遇到问题，可以在 V2EX 交流讨论或在下方评论：

<https://be.st/Mv7j>

Debian / Ubuntu 下使用 nginx-acme 自动签发并配置 SSL 证书

[https://u.sb/debian-nginx-acme/](/debian-nginx-acme/)

无责任编辑

Showfom

发布时间

Jan 15th, 2026

版权协议

[WTFPL](https://u.sb/license.txt)

[#nginx](/tags/nginx/)[#ssl](/tags/ssl/)[#nginx-acme](/tags/nginx-acme/)[#debian](/tags/debian/)[#ubuntu](/tags/ubuntu/)

人過留名，雁過留聲，翺翔的飛鳥總會尋到屬於自己的領地。
記得，要讓簡樸而純粹的心靜靜地感受，你要相信，美好的世界總會自己尋路向你走來。

Copyright © 2026 烧饼博客··

[冀 ICP 备 16001626 号 - 4](https:/...