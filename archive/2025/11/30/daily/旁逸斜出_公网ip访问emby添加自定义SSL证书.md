---
title: 公网ip访问emby添加自定义SSL证书
url: https://www.mihu.live/archives/491/
source: 旁逸斜出
date: 2025-11-30
fetch_date: 2025-12-01T03:39:25.843838
---

# 公网ip访问emby添加自定义SSL证书

[![旁逸斜出](data:image/gif;base64...)](https://www.mihu.live/ "旁逸斜出")

[首页](https://www.mihu.live/ "首页")
[归档](https://www.mihu.live/archive.html "归档")
[关于](https://www.mihu.live/about.html "关于")
[友情链接](https://www.mihu.live/links.html "友情链接")

Search

[1
clash for windows允许局域网连接，TAP和TUN模式
111,416 阅读](https://www.mihu.live/archives/208/ "clash for windows允许局域网连接，TAP和TUN模式")
[2
使用emby打造个人影视媒体库
26,651 阅读](https://www.mihu.live/archives/19/ "使用emby打造个人影视媒体库")
[3
解决kodi的emby插件无法找到文件...相对路径、绝对路径问题
11,521 阅读](https://www.mihu.live/archives/17/ "解决kodi的emby插件无法找到文件...相对路径、绝对路径问题")
[4
Ubuntu to go/Linux to go/将linux系统安装到u盘或移动硬盘
9,669 阅读](https://www.mihu.live/archives/68/ "Ubuntu to go/Linux to go/将linux系统安装到u盘或移动硬盘")
[5
魔改版rclone挂载世纪互联onedrive
5,389 阅读](https://www.mihu.live/archives/9/ "魔改版rclone挂载世纪互联onedrive")

公网ip访问emby添加自定义SSL证书

[技术](https://www.mihu.live/category/%E6%8A%80%E6%9C%AF/ "技术")
[软件](https://www.mihu.live/category/%E8%BD%AF%E4%BB%B6/ "软件")
[文章](https://www.mihu.live/category/%E6%96%87%E7%AB%A0/ "文章")
[其他](https://www.mihu.live/category/%E5%85%B6%E4%BB%96/ "其他")

[登录](https://www.mihu.live/admin/login.php)

Search

标签搜索

* [sql](https://www.mihu.live/tag/sql/)
* [代理](https://www.mihu.live/tag/%E4%BB%A3%E7%90%86/)
* [sqlserver](https://www.mihu.live/tag/sqlserver/)
* [Oracle](https://www.mihu.live/tag/Oracle/)
* [onedrive](https://www.mihu.live/tag/onedrive/)
* [软件](https://www.mihu.live/tag/%E8%BD%AF%E4%BB%B6/)
* [magisk](https://www.mihu.live/tag/magisk/)
* [vps](https://www.mihu.live/tag/vps/)
* [emby](https://www.mihu.live/tag/emby/)
* [ftp](https://www.mihu.live/tag/ftp/)
* [TrafficMonitor](https://www.mihu.live/tag/TrafficMonitor/)
* [TranslucentTB](https://www.mihu.live/tag/TranslucentTB/)
* [nfo](https://www.mihu.live/tag/nfo/)
* [qBittorrent](https://www.mihu.live/tag/qBittorrent/)
* [emby for kodi插件](https://www.mihu.live/tag/emby-for-kodi%E6%8F%92%E4%BB%B6/)
* [emby插件](https://www.mihu.live/tag/emby%E6%8F%92%E4%BB%B6/)
* [Transmission Remote GUI](https://www.mihu.live/tag/Transmission-Remote-GUI/)
* [transmission](https://www.mihu.live/tag/transmission/)
* [优选ip](https://www.mihu.live/tag/%E4%BC%98%E9%80%89ip/)
* [世纪互联rclone](https://www.mihu.live/tag/%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94rclone/)

![侧边栏壁纸]()

![博主昵称](data:image/png;base64...)

[旁逸斜出](https://www.mihu.live/about.html)

* 累计撰写 **38** 篇文章
* 累计收到 **92** 条评论

* [首页](https://www.mihu.live/ "首页")
* 栏目
  + [技术](https://www.mihu.live/category/%E6%8A%80%E6%9C%AF/ "技术")
  + [软件](https://www.mihu.live/category/%E8%BD%AF%E4%BB%B6/ "软件")
  + [文章](https://www.mihu.live/category/%E6%96%87%E7%AB%A0/ "文章")
  + [其他](https://www.mihu.live/category/%E5%85%B6%E4%BB%96/ "其他")
* 页面
  + [归档](https://www.mihu.live/archive.html "归档")
  + [关于](https://www.mihu.live/about.html "关于")
  + [友情链接](https://www.mihu.live/links.html "友情链接")

* [首页](https://www.mihu.live/ "首页")
* /
* [技术](https://www.mihu.live/category/%E6%8A%80%E6%9C%AF/ "技术")
* /
* 正文

[技术](https://www.mihu.live/category/%E6%8A%80%E6%9C%AF/ "技术")

# 公网ip访问emby添加自定义SSL证书

![who](data:image/png;base64...)

[who](https://www.mihu.live/author/1/ "who")

2025-11-30
/
0 评论
/
5 阅读
/
正在检测是否收录...

11/30

当我们用ip：端口号的形式直接访问emby的时候，http明文传输不安全(当然有人用域名解析，申请域名的ssl证书，这也是很好的方案),下面演示如何添加emby自定义SSL证书

# 生成PKCS＃12 文件

在linux下（windows自行百度在线生成网站）

```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 3650 -nodes -subj "/CN=MyEmby"
```

* openssl req -x509: 告诉系统，“我要申请一张证书，而且我自己签字生效”（这就是自签名）。
* newkey rsa:4096: “给我造一把新的私钥，加密强度要最高级别的 4096 位”
* keyout key.pem: “把私钥保存为 key.pem”。
* out cert.pem: “把公钥（身份证）保存为 cert.pem”。
* days 3650: “这张身份证的有效期是 3650 天（大约 10 年）”
* nodes: “私钥文件不要加密”。这意味着 Emby 读取它时不需要你每次手动输入密码，方便自动启动。
* subj "/CN=MyEmby": “证书的主人名字叫 MyEmby”。这避免了系统问你一堆问题（比如你叫什么、在哪个国家、哪个城市等），直接填好

执行完上面的命令后就会生成cert.pem和key.pem

接下来生成emby需要的格式证书

```
openssl pkcs12 -export -out emby.p12 -inkey key.pem -in cert.pem -passout pass:123dfas
```

* openssl pkcs12 -export: “我要把刚才那两个文件打包导出”。
* in cert.pem 和 -inkey key.pem: “要把刚才生成的身份证和钥匙放进去”。
* out emby.p12: “打包好的文件名字叫 emby.p12”。
* **passout pass:123456: “给这个包裹上把锁，密码是 123dfas”，密码自行修改**

执行完会生成**emby.p12**

# emby服务器配置

emby服务器-**设置**-**网络**，**自定义SSL证书路径**选择刚才生成的emby.p12（脚本执行在哪个路径，就会生成在哪个路径）,**证书密码**填上一步时的密码，点击页面下方的保存，重启服务器
emby默认https访问端口号8920，可以在浏览器测试<https://xxx.xxx.xxx.xxx:8920>，因为这个是自定义证书，浏览器会报警，但是是加密的，点击继续访问即可
如果是客户端，连接时会弹出提示，接受证书即可

0

[emby](https://www.mihu.live/tag/emby/)

版权属于：
who

本文链接：

<https://www.mihu.live/archives/491/>

作品采用：

《
[署名-非商业性使用-相同方式共享 4.0 国际 (CC BY-NC-SA 4.0)](//creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
》许可协议授权

相关推荐

[![使用emby打造个人影视媒体库](https://npm.elemecdn.com/typecho-joe-latest/assets/img/lazyload.jpg)

###### 使用emby打造个人影视媒体库](https://www.mihu.live/archives/19/ "使用emby打造个人影视媒体库")

* [下一篇](https://www.mihu.live/archives/475/ "Cloudflare+SaaS回源优选IP加速大陆用户访问")

### 评论 (0)

画图模式
文本模式

* 细
* 中
* 粗

取消
发送评论

![博主栏壁纸](data:image/gif;base64...)

![博主头像](data:image/png;base64...)
[旁逸斜出](https://www.mihu.live/about.html)

38
文章数

92
评论量

热门文章

1. [*1*
   ![clash for windows允许局域网连接，TAP和TUN模式](https://npm.elemecdn.com/typecho-joe-latest/assets/img/lazyload.jpg)

   ###### clash for windows允许局域网连接，TAP和TUN模式

   111416 阅读 - 01/19](https://www.mihu.live/archives/208/ "clash for windows允许局域网连接，TAP和TUN模式")
2. [*2*
   ![使用emby打造个人影视媒体库](https://npm.elemecdn.com/typecho-joe-latest/assets/img/lazyload.jpg)

   ###### 使用emby打造个人影视媒体库

   26651 阅读 - 05/28](https://www.mihu.live/archives/19/ "使用emby打造个人影视媒体库")
3. [*3*
   ![解决kodi的emby插件无法找到文件...相对路径、绝对路径问题](https://npm.elemecdn.com/typecho-joe-latest/assets/img/lazyload.jpg)

   ###### 解决kodi的emby插件无法找到文件...相对路径、绝对路径问题

   11521 阅读 - 04/12](https://www.mihu.live/archives/17/ "解决kodi的emby插件无法找到文件...相对路径、绝对路径问题")

标签云

[关于](https://www.mihu.live/about.html) [站点地图](https://www.mihu.live/sitemap.xml) [RSS订阅](https://www.mihu.live/feed)