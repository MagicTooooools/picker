---
title: 获得自己在Google眼里的IP
url: https://blog.est.im/2025/stdout-18
source: est の 输入 输出和出入
date: 2025-12-08
fetch_date: 2025-12-09T03:21:45.336386
---

# 获得自己在Google眼里的IP

# [est の 输入 输出和出入](/)

* [Home](/)
* [RSS](https://feeds.feedburner.com/initiative)
* [About](/about)
* [Category](/category)

## 获得自己在Google眼里的IP

Posted 2025-12-08 | stdout

可能是最古老的问题，自己的IP是什么，网上这样的工具一大把，我自己甚至都[搓了个](https://t.est.im/ip)

但是你访问 google 的ip，如果遇到网络配置很复杂，split tunnel 什么的，可能就麻烦了。

### Google

搜了一圈，没想到 gemini 给的线索最有用。它提到可以

`dig TXT +short @ns1.google.com o-o.myaddr.l.google.com`

但是AI 毕竟笨了些，套个 DoH 不就可用了。

```
curl -s "https://dns.google/resolve?name=o-o.myaddr.l.google.com&type=TXT"
{"Status":0,"TC":false,"RD":true,"RA":true,"AD":false,"CD":false,"Question":[{"name":"o-o.myaddr.l.google.com.","type":16}],"Answer":[{"name":"o-o.myaddr.l.google.com.","type":16,"TTL":60,"data":"edns0-client-subnet x.x.x.x/24"},{"name":"o-o.myaddr.l.google.com.","type":16,"TTL":60,"data":"74.114.31.154"}],"Comment":"Response from 216.239.38.10."}
```

返回里的 edns0-client-subnet 就是C类网址。不够精确但是够用。

curl -6 可以获得 ipv6

如果 dns.google 不够好，可以换 dns.google.com

`curl "https://dns.google.com/resolve?type=TXT&name=o-o.myaddr.l.google.com"`

网上关于google这个 trick 没有正式文档支持。我在[`public-dns-discuss` 能搜到](https://groups.google.com/g/public-dns-discuss/c/uyzmMcHQBE0/m/x7o-uUVQFAAJ) 最早 2015年一位叫 Shen Wan 的疑似google员工的问题排查方法。他原话

> our name is "Google Public DNS", not "Open DNS"

所以应该和当年 8.8.8.8 推出有关。

### Gmail 登录历史

[V站](https://v2ex.com/t/1177588) 还有一个偏方说 Gmail 把邮件滚动到底部有个 Last account activity ，点开也有IP。

但是需要登录，而且网址里有个 `ik` 的参数猜不出规律。

### 其它DNS

* `dig whoami.cloudflare ch txt @1.1.1.1 +short`
* `dig +short myip.opendns.com @resolver1.opendns.com`
* `dig whoami.akamai.net. @ns1-1.akamaitech.net. +short`

### aws

然后顺便发现 https://checkip.amazonaws.com/ 有[官方文档](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/get-set-up-for-amazon-ecs.html)说明

### cloudflare

所有 cloudflare 都可以查 /cdn-cgi/trace 比如

* https://www.cloudflare.com/cdn-cgi/trace
* https://x.com/cdn-cgi/trace
* https://chatgpt.com/cdn-cgi/trace

## Comments