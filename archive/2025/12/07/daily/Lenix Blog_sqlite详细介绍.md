---
title: sqlite详细介绍
url: https://blog.p2hp.com/archives/11841
source: Lenix Blog
date: 2025-12-07
fetch_date: 2025-12-08T03:24:43.197773
---

# sqlite详细介绍

### [Lenix Blog](https://blog.p2hp.com/ "Lenix Blog")

记录－交流－Web开发知识分享

* [博客声明](https://blog.p2hp.com/licence)
* [书单](https://blog.p2hp.com/%E4%B9%A6%E5%8D%95)
* [标签云](https://blog.p2hp.com/tags)
* [链接](https://blog.p2hp.com/links)
* [留言本](https://blog.p2hp.com/guestbook)
* [关于我](https://blog.p2hp.com/about)
* 推荐
  + [PHP框架最新性能压力测试比较](https://blog.p2hp.com/archives/6906)
  + [现代PHP编程指导](https://blog.p2hp.com/archives/3634)
* 站点
  + [全栈AI助手](https://aichat.p2hp.com/)
  + [极资讯](https://tech.p2hp.com/)
  + [AI导航](https://tool.p2hp.com/nav_ai.html)
* [全栈开发网](https://www.p2hp.com)
* [关于付费](https://blog.p2hp.com/aboutpay)

所有分类
Apache
Go
Linux
Mac OS
Mysql
Nginx
Nosql
PHP
Vim
web前端
Wordpress
个人日志
云计算
人工智能
创业
工具
建站
开源
收费
架构
概念
物联网
移动互联网
编程
趋势
音视频

# [sqlite详细介绍](https://blog.p2hp.com/archives/11841)

[admin](https://blog.p2hp.com/archives/author/admin)
 [2025年12月7日](https://blog.p2hp.com/archives/11841 "09:24")
[概念](https://blog.p2hp.com/archives/category/gainian)
[暂无评论](https://blog.p2hp.com/archives/11841#respond)  1 浏览

[SQLite](https://sqlite.p2hp.com/) 是一个 **轻量级、零配置、嵌入式的关系型数据库**。它非常小巧但功能完整，被广泛用于移动端、桌面应用、嵌入式设备、浏览器、IoT、服务器软件等各种场景。

下面用简单清晰的方式介绍 SQLite。

---

# ✅ **SQLite 是什么？**

SQLite 是一个 **基于文件** 的关系型数据库系统：

* **不需要安装服务器**
* **所有数据都保存在一个普通文件中（.db / .sqlite）**
* **支持标准 SQL 语法**
* 线程安全、高性能、跨平台
* 程序可以直接嵌入 SQLite 引擎，无需网络通信

它不像 [MySQL](https://mysql.p2hp.com/)、[PostgreSQL](https://postgresql.p2hp.com/) 那样是“守护进程型数据库”，而更像一个“数据库文件 + 操作库”，非常轻量。

---

# ✅ **SQLite 的特点（为什么这么流行）**

### **1. 零配置，开箱即用**

无需安装、无需启动服务、无需管理权限，复制数据库文件即可使用。

### **2. 单个文件保存所有数据**

你可以简单地把数据库文件：

* 打包
* 备份
* 复制到另一台设备
* 放到 USB、移动硬盘等

非常便携。

### **3. 高性能**

虽然轻量，但性能不弱：
在单机读写方面，有时甚至比 MySQL/PG 更快（没有网络 IO）。

### **4. 可靠性极高**

SQLite 用在飞行器、手机系统、浏览器、智能家电等严苛环境，具备：

* 事务支持（ACID）
* 崩溃恢复机制
* filesystem-independent

可靠性经过全球大量应用验证。

### **5. 广泛应用**

几乎每个设备都用 SQLite：

* **[Android](http://android.p2hp.com/) / iOS** 内部数据库
* **Chrome、Firefox** 浏览器
* **微信 / QQ 的本地消息记录**
* **Photoshop / Office** 配置缓存
* **嵌入式系统、智能柜、无人机**

全世界每天有数十亿次 SQLite 的部署和读写操作。

---

# ✅ **SQLite 的核心技术点**

### **1. 标准 SQL 支持**

支持大部分 SQL92/SQL99，包括：

* SELECT、INSERT、UPDATE、DELETE
* JOIN（支持左外连接）
* 事务（BEGIN/COMMIT）
* 视图、触发器
* 索引、唯一约束、外键（需要开启）

### **2. 数据类型系统（灵活）**

SQLite 使用 **动态类型系统**：

SQLite 并非严格强类型，而是：

* 列可以声明类型，但并不强制存储类型
* 一个列声明 INT，也可以存 TEXT（不推荐，但 SQLite 允许）

这种设计使 SQLite 更灵活，也能减少 schema 变更麻烦。

### **3. 并发模型**

SQLite 使用 **写锁定** 机制：

* **读并发高**：多个读同时进行
* **写只能单个进行**：但写非常快，通常问题不大
  （也可以使用 WAL 模式改善写性能）

---

# ✅ **适用场景**

### ✓ 非常适合

* 移动 APP 本地数据库
* 桌面应用本地保存数据
* 中小规模网站的数据缓存
* 单机工具/脚本
* 嵌入式设备（IoT、智能硬件）
* 临时数据库、测试用数据库
* 只需一个文件的高可靠数据存储

### ✗ 不适合

大规模并发写、需要多台服务器共享的大型项目，比如：

* 高并发网站后台
* 金融交易系统
* 分布式数据库需求

此时应使用 MySQL、PostgreSQL 等。

---

# ✅ 简单示例

### 创建数据库并建表

`CREATE TABLE users (
id INTEGER PRIMARY KEY AUTOINCREMENT,
name TEXT,
age INTEGER
);`

### 插入数据

`INSERT INTO users (name, age) VALUES ('Alice', 20);`

### 查询数据

`SELECT * FROM users WHERE age > 18;`

### 修改数据

`UPDATE users SET age = 21 WHERE id = 1;`

### 删除数据

`DELETE FROM users WHERE id = 1;`

---

#

### 相关博文

* [![静态网站如何启用数据库查询](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/26.jpg)](https://blog.p2hp.com/archives/8049)[静态网站如何启用数据库查询](https://blog.p2hp.com/archives/8049)
* [![免费数据库（SQLite、Berkeley DB、PostgreSQL、MySQL、Firebird、mSQL、MSDE、DB2 Ex](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/3.jpg)](https://blog.p2hp.com/archives/697)[免费数据库（SQLite、Berkeley DB、PostgreSQL、MySQL、Firebird、mSQL、MSDE、DB2 Ex](https://blog.p2hp.com/archives/697)
* [![一个小时内学习 SQLite 数据库](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/13.jpg)](https://blog.p2hp.com/archives/495)[一个小时内学习 SQLite 数据库](https://blog.p2hp.com/archives/495)
* [![app广告管理功能设计](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/12.jpg)](https://blog.p2hp.com/archives/5365)[app广告管理功能设计](https://blog.p2hp.com/archives/5365)
* [![关于H.264的码率，720P、1080P输出比特率设置](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/20.jpg)](https://blog.p2hp.com/archives/9617)[关于H.264的码率，720P、1080P输出比特率设置](https://blog.p2hp.com/archives/9617)
* [![php并行运行多任务,daemon守护进程](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/25.jpg)](https://blog.p2hp.com/archives/1713)[php并行运行多任务,daemon守护进程](https://blog.p2hp.com/archives/1713)
* [![apache、lighttpd、nginx三者的区别](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/24.jpg)](https://blog.p2hp.com/archives/68)[apache、lighttpd、nginx三者的区别](https://blog.p2hp.com/archives/68)
* [![干货分享：如何从朋友圈带百万流量](https://blog.p2hp.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/18.jpg)](https://blog.p2hp.com/archives/1133)[干货分享：如何从朋友圈带百万流量](https://blog.p2hp.com/archives/1133)

sqlite详细介绍

赞 0 赏 分享

标签: [SQLite](https://blog.p2hp.com/archives/tag/sqlite)

* [← 大语言模型中，role为user、assistant、system有什么区别](https://blog.p2hp.com/archives/11831)

### 最新更新

* [sqlite详细介绍](https://blog.p2hp.com/archives/11841) ( 2025年12月7日)
* [H5解决m3u8视频直播流问题](https://blog.p2hp.com/archives/5656) ( 2025年3月12日)
* [大语言模型中，role为user、assistant、system有什么区别](https://blog.p2hp.com/archives/11831) ( 2024年10月28日)
* [ai大模型对话的历史数量多少最合适](https://blog.p2hp.com/archives/11826) ( 2024年10月28日)
* [大模型的 temperature设为多少比较好](https://blog.p2hp.com/archives/11828) ( 2024年10月28日)
* [Alibaba Cloud Linux 3 yum 安装 PHP8.1](https://blog.p2hp.com/archives/11808) ( 2024年10月17日)
* [解决 nginx etag和last-modified头消失问题](https://blog.p2hp.com/archives/9740) ( 2024年4月9日)
* [nginx 用try\_files 时,gzip\_static不起作用,如何解决](https://blog.p2hp.com/archives/11794) ( 2024年3月3日)
* [使用静态 gzip 后，Nginx 速度更快！设置方法和压缩方法说明](https://blog.p2hp.com/archives/11783) ( 2024年2月1日)
* [简单一招竟把 nginx 服务器性能提升 50 倍](https://blog.p2hp.com/archives/11776) ( 2024年2月1日)

### 月度热门文章

* [http、https、RTSP、RTMP等常见默认端口大...](https://blog.p2hp.com/archives/9519 "http、https、RTSP、RTMP等常见默认端口大全")
  77 views
* [HTML网页跳转代码大全 网页自动跳转代码](https://blog.p2hp.com/archives/11602)
  72 views
* [JavaScript 读取Cookie的方法](https://blog.p2hp.com/archives/9201)
  57 views
* [大语言模型中，role为user、assistant、s...](https://blog.p2hp.com/archives/11831 "大语言模型中，role为user、assistant、system有什么区别")
  47 views
* [PHP获取汉字的拼音(全部与首字母)](https://blog.p2hp.com/archives/345)
  41 views
* [关于H.264的码率，720P、1080P输出比特率设置](https://blog.p2hp.com/archives/9617)
  31 views
* [流媒体协议之RTSP详解](https://blog.p2hp.com/archives/10472)
  28 views
* [在html5页面上播放 RTSP 的 7 种方法](https://blog.p2hp.com/archives/8626)
  26 views
* [大模型的 temperature设为多少比较好](https://blog.p2hp.com/archives/11828)
  24 views
* [使用OBS-Studio软件推流直播教程，支持RTMP及...](https://blog.p2hp.com/archives/8572 "使用OBS-Studio软件推流直播教程，支持RTMP及RTSP两种协议")
  22 views

2025年 12月

| 一 | 二 | 三 | 四 | 五 | 六 | 日 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 2 | 3 | 4 | 5 | 6 | [7](https://blog.p2hp.com/archives/date/2025/12/07) |
| 8 | 9 | 10 | 11 | 12 | 13 | 14 |
| 15 | 16 | 17 | 18 | 19 | 20 | 21 |
| 22 | 23 | 24 | 25 | 26 | 27 | 28 |
| 29 | 30 | 31 |  | | | |

[« 10月](https://blog.p2hp.com/archives/date/2024/10)

### 标签

[AJAX](https://blog.p2hp.com/archives/tag/ajax)
[APP](https://blog.p2hp.com/archives/tag/app)
[Composer](https://blog.p2hp.com/archives/tag/composer)
[cookie](https://blog.p2hp.com/archives/tag/cookie)
[CSS](https://blog.p2hp.com/archives/tag/css)
[GIT](https://blog.p2hp.com/archives/tag/git)
[HTML5](https://blog.p2hp.com/archives/tag/html5)
[http](https://blog.p2hp.com/archives/tag/http)
[http2](https://blog.p2hp.com/archives/tag/http2)
[HTTPS](https://blog.p2hp.com/archives/tag/https)
[HTTP协议](https://blog.p2hp.com/archives/tag/http%E5%8D%8F%E8%AE%AE)
[javascript](https://blog.p2hp.com/archives/tag/javascript)
[Laravel](https://blog.p2hp.com/archives/tag/laravel)
[Linux](https://blog.p2hp.com/archives/tag/linux)
[mongodb](https://blog.p2hp.com/archives/tag/mongodb)
[MQTT](https://blog.p2hp.com/archives/tag/mqtt)
[Mysql](https:/...