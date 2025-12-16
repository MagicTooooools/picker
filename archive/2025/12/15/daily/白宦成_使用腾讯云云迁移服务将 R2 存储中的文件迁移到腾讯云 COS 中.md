---
title: 使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中
url: https://www.ixiqin.com/2025/12/15/use-tencent-cloud-migration-service-to-migrate-files-from/
source: 白宦成
date: 2025-12-15
fetch_date: 2025-12-16T03:27:35.653756
---

# 使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中

[跳至正文](#content)

# [白宦成](https://www.ixiqin.com/)

## Just Keep Shipping

 菜单

* [首页](https://www.ixiqin.com/)
* [写作](https://www.ixiqin.com/life/%E5%86%99%E4%BD%9C/)
* [投资](https://www.ixiqin.com/life/%E6%8A%95%E8%B5%84/)
* [技术](https://www.ixiqin.com/tech/)
* [联系我](https://www.ixiqin.com/to-contact-me/)
* [作品集](https://www.ixiqin.com/portfolio/)
* [归档](https://www.ixiqin.com/the-article-archive/)
* [打赏](https://www.ixiqin.com/exceptional/)
* [关于](https://www.ixiqin.com/aboutme/)
* [Ask Me Anything!](https://wn80ogjf8mt19kjy.mikecrm.com/drNLq2q)

# 使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中

[发表评论](https://www.ixiqin.com/2025/12/15/use-tencent-cloud-migration-service-to-migrate-files-from/#respond)

最近把这个域名重新备案了一下，就可以利用起我在腾讯云上的闲置服务器。既然要迁移服务器，不妨将图床一并迁移，这样后续使用起来也方便，国内的读者加载起来速度也快。

不过，这些年大量使用，我的文件还是挺多的….足足有 13GB 的文件，手动一个个搬迁可就累死了；于是乎，我决定试试腾讯云的迁移服务，来帮助我把 [R2](https://www.cloudflare.com/developer-platform/products/r2/) 上的文件迁移过来。

![image](https://files.ixiqin.com/2025/12/15085424/image.png "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 1")

## 获取配置信息

想要使用腾讯云提供的云迁移（CMG）服务，则需要获取一些配置信息，具体包括：

* Cloudflare R2 的 Access ID 和 Secret Key
* 腾讯云的 Access ID 和 Access Key，创建好的 Bucket（要迁移的目标）

R2 的相关配置可以在 CloudFlare R2的配置页面找到；如果没有的话，你就创建一个新的。

![image](https://files.ixiqin.com/2025/12/15085814/image.png "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 2")![image](data:image/svg+xml... "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 2")

腾讯云的则可以在[腾讯云密钥管理](https://console.cloud.tencent.com/cam)中获取，建议创建一个新的用户，并授予 `QcloudMSPFullAccess` 及 `QcloudCOSAccessForMSPRole` 策略，点击子账号可以看到如下图的两个权限。

![image](https://files.ixiqin.com/2025/12/15132234/image.png "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 3")![image](data:image/svg+xml... "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 3")

## 配置云迁移

完成账号的确认后，接下来就是配置云迁移。打开云迁移中的「对象存储迁移」，或者直接打开[这个链接](https://console.cloud.tencent.com/msp/v2file)，就直接进入云迁移的页面。

![image](https://files.ixiqin.com/2025/12/15132337/image.png "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 4")![image](data:image/svg+xml... "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 4")

### 源站配置

接下来配置云迁移的具体配置，点击新建人数，在新的页面中，输入你的 CloudFlare 配置信息，具体可以参考下面的截图:

![image](https://files.ixiqin.com/2025/12/15132713/image.png "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 5")![image](data:image/svg+xml... "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 5")

* AK/SK： 你从 Cloudflare 获取的相关参数；
* 桶名称：你的 R2 Bucket 的名称；
* 空间域名：你的 R2 的域名，是 `uid.r2.cloudflarestoage.com`，比如我的是 `https://24071135c3ad9d9196e7e45e33948d28.r2.cloudflarestorage.com`。
* 桶的所在地：比如我的是亚洲，就选 `apac`。

源站中的其他选项可以根据需要选择，如果你是完整迁移，和我保持一致即可。

### 目标站点配置

接下来是配置迁移目标，这里指标支持迁移到腾讯云自家的 COS 上；填入你的 Secret ID 和 Secret Key，然后可以直接在下面输入具体的 Bucket 名称，或者填完后点击下拉框右侧的刷新按钮后，选择合适的。

![image](https://files.ixiqin.com/2025/12/15133033/image.png "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 6")![image](data:image/svg+xml... "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 6")

其他的选项，如果你和我一样是整个 Bucket 迁移，则可以保持相同的配置，直接整个迁移。

配置完成后，点击最下方的新建并启动，就会启动搬迁。接下来就回到任务列表等刷新即可，等待他自己搬迁完即可。实测搬迁速度很快，13G 的文件，8 分钟就搬迁完成了（还是我限制了搬迁的带宽），如果是不限制，估计 2 分钟就能搬迁完成。

![image](https://files.ixiqin.com/2025/12/15133245/image.png "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 7")![image](data:image/svg+xml... "使用腾讯云云迁移服务将 R2 存储中的文件迁移到腾讯云 COS 中 - 7")

如果你需要和我一样，从外部的 S3 将文件搬迁到腾讯云的 COS 上，不妨试试看这个方法~

 本条目发布于[2025年12月15日](https://www.ixiqin.com/2025/12/15/use-tencent-cloud-migration-service-to-migrate-files-from/ "下午9:34")。属于[技术](https://www.ixiqin.com/tech/)分类，被贴了 [WordPress](https://www.ixiqin.com/tag/wordpress/)、[腾讯云](https://www.ixiqin.com/tag/%E8%85%BE%E8%AE%AF%E4%BA%91/)、[随笔](https://www.ixiqin.com/tag/essay/) 标签。作者是[白宦成](https://www.ixiqin.com/author/bestony/ "查看所有由白宦成发布的文章")。

### 文章导航

[← 我的独立开发者书单 2025 版](https://www.ixiqin.com/2025/12/13/book-list-of-indiehacker/)

### 发表回复 [取消回复](/2025/12/15/use-tencent-cloud-migration-service-to-migrate-files-from/#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

[ ]  在此浏览器中保存我的显示名称、邮箱地址和网站地址，以便下次评论时使用。

[x] 如果有人回复我的评论，请通过电子邮件通知我。

Δ

Hi, 你好，我是**白宦成**，一个独立开发者、软件工程师。

搜索

[Django](https://www.ixiqin.com/tag/django/) [Docker](https://www.ixiqin.com/tag/docker/) [dokuwiki](https://www.ixiqin.com/tag/dokuwiki/) [github](https://www.ixiqin.com/tag/github/) [golang](https://www.ixiqin.com/tag/golang/) [Javascript](https://www.ixiqin.com/tag/javascript/) [Kindle](https://www.ixiqin.com/tag/kindle/) [macOS](https://www.ixiqin.com/tag/macos/) [micro:bit](https://www.ixiqin.com/tag/microbit/) [node.js](https://www.ixiqin.com/tag/node-js/) [PHP](https://www.ixiqin.com/tag/php/) [PTA](https://www.ixiqin.com/tag/PTA/) [python](https://www.ixiqin.com/tag/python/) [Rust](https://www.ixiqin.com/tag/rust/) [vue](https://www.ixiqin.com/tag/vue/) [WordPress](https://www.ixiqin.com/tag/wordpress/) [个人品牌](https://www.ixiqin.com/tag/%E4%B8%AA%E4%BA%BA%E5%93%81%E7%89%8C/) [书摘](https://www.ixiqin.com/tag/%E4%B9%A6%E6%91%98/) [作品](https://www.ixiqin.com/tag/my-works/) [健康](https://www.ixiqin.com/tag/%E5%81%A5%E5%BA%B7/) [公众号推荐](https://www.ixiqin.com/tag/%E5%85%AC%E4%BC%97%E5%8F%B7%E6%8E%A8%E8%8D%90/) [写作](https://www.ixiqin.com/tag/writing/) [前端](https://www.ixiqin.com/tag/%E5%89%8D%E7%AB%AF/) [印象笔记](https://www.ixiqin.com/tag/%E5%8D%B0%E8%B1%A1%E7%AC%94%E8%AE%B0/) [小程序](https://www.ixiqin.com/tag/%E5%B0%8F%E7%A8%8B%E5%BA%8F/) [工作](https://www.ixiqin.com/tag/%E5%B7%A5%E4%BD%9C/) [开发经验](https://www.ixiqin.com/tag/%E5%BC%80%E5%8F%91%E7%BB%8F%E9%AA%8C/) [思考](https://www.ixiqin.com/tag/%E6%80%9D%E8%80%83/) [技术](https://www.ixiqin.com/tag/%E6%8A%80%E6%9C%AF/) [投资](https://www.ixiqin.com/tag/%E6%8A%95%E8%B5%84/) [旅行](https://www.ixiqin.com/tag/%E6%97%85%E8%A1%8C/) [月度总结](https://www.ixiqin.com/tag/%E6%9C%88%E5%BA%A6%E6%80%BB%E7%BB%93/) [灵感](https://www.ixiqin.com/tag/%E7%81%B5%E6%84%9F/) [独立开发](https://www.ixiqin.com/tag/%E7%8B%AC%E7%AB%8B%E5%BC%80%E5%8F%91/) [独立开发者](https://www.ixiqin.com/tag/%E7%8B%AC%E7%AB%8B%E5%BC%80%E5%8F%91%E8%80%85/) [生活](https://www.ixiqin.com/tag/%E7%94%9F%E6%B4%BB/) [电影](https://www.ixiqin.com/tag/%E7%94%B5%E5%BD%B1/) [硬件](https://www.ixiqin.com/tag/%E7%A1%AC%E4%BB%B6/) [租房](https://www.ixiqin.com/tag/%E7%A7%9F%E6%88%BF/) [美国之旅](https://www.ixiqin.com/tag/journey-to-the-united-states/) [职场](https://www.ixiqin.com/tag/%E8%81%8C%E5%9C%BA/) [观后感](https://www.ixiqin.com/tag/%E8%A7%82%E5%90%8E%E6%84%9F/) [设计](https://www.ixiqin.com/tag/%E8%AE%BE%E8%AE%A1/) [读书](https://www.ixiqin.com/tag/%E8%AF%BB%E4%B9%A6/) [随笔](https://www.ixiqin.com/tag/essay/)

#### 文档目录

* [个人品牌](https://www.ixiqin.com/%E4%B8%AA%E4%BA%BA%E5%93%81%E7%89%8C/) (8)
* [个人检视](https://www.ixiqin.com/check/) (9)
* [产品](https://www.ixiqin.com/product/) (6)
* [学习](https://www.ixiqin.com/study/) (4)
* [情感](https://www.ixiqin.com/feeling/) (20)
* [技术](https://www.ixiqin.com/tech/) (152)
* [生活](https://www.ixiqin.com/life/) (57)
* [设计](https://www.ixiqin.com/design/) (6)
* [随笔](https://www.ixiqin.com/essay/) (668)

[自豪地采用WordPress](https://cn.wordpress.org/ "优雅的个人发布平台")