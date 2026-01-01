---
title: VPS Manager Pro (Web SSH Terminal)
url: https://ednovas.xyz/2025/12/31/vps-manager-pro/
source: EdNovas的小站
date: 2025-12-31
fetch_date: 2026-01-01T03:38:58.741818
---

# VPS Manager Pro (Web SSH Terminal)

![avatar](https://cdn.jsdelivr.net/gh/wdm1732418365/CDN/New%20folder/icon.png)

[Articles

296](/archives/)

[Tags

369](/tags/)

[Categories

14](/categories/)

---

[主页 | Home](https://navigate.ednovas.xyz)

[随机 | Random](/random.html)

[目录 | Categories](/categories/)

[回忆 | Archives](/archives/)

[关于我 | About](/about/)

[朋友圈 | Link](/link/)

[打赏支持 | Support](/%E6%94%AF%E6%8C%81/)

[EdNovas的小站](/)

Search

[主页 | Home](https://navigate.ednovas.xyz)

[随机 | Random](/random.html)

[目录 | Categories](/categories/)

[回忆 | Archives](/archives/)

[关于我 | About](/about/)

[朋友圈 | Link](/link/)

[打赏支持 | Support](/%E6%94%AF%E6%8C%81/)

# VPS Manager Pro (Web SSH Terminal)

Created2025-12-31|Updated2025-12-31|[VPS](/categories/VPS/)[编程](/categories/VPS/%E7%BC%96%E7%A8%8B/)[软件](/categories/VPS/%E7%BC%96%E7%A8%8B/%E8%BD%AF%E4%BB%B6/)

|Word count:945|Reading time:4min|Post View:

# Sample Website 演示网站

[https://vps.ednovas.tech](https://vps.ednovas.tech/)

![Snipaste_2025-12-04_22-32-11](https://github.com/user-attachments/assets/53cfb7b2-d51d-40d3-b63c-82b285f828c8)
![Snipaste_2025-12-04_22-30-24](https://github.com/user-attachments/assets/67959879-2f62-45c7-ad11-0ea91e2220cf)
![Snipaste_2025-12-04_22-31-45](https://github.com/user-attachments/assets/e0f8725b-bf5d-46e1-9d2f-d35261e687a2)

# VPS Manager Pro (Web SSH Terminal)

[English](#english) | [中文说明](#chinese)

## 🇬🇧 English

**VPS Manager Pro** is a powerful, web-based SSH terminal that allows you to manage multiple VPS connections from your browser. It features multi-user support, session saving via MySQL, and intelligent detection of proxy configurations (VMess/SOCKS5) directly from the terminal output.

### ✨ Key Features

* **Web-based SSH:** Full interactive terminal using `xterm.js` and `socket.io`.
* **Multi-User System:** Secure login/logout system with password hashing (`bcrypt`). Users can only see their own saved sessions.
* **Session Management:** Save, edit, and label your VPS connection details for quick access.
* **Smart Detection:** Automatically detects VMess links and SOCKS5 credentials in the terminal output and generates QR codes/copy buttons.
* **Quick Tools:** Integrated buttons for common VPS scripts (EdNovas Toolbox, VMess/TCP installation).

### 📂 File Structure

* `server.js`: The main Node.js backend handling API routes, SSH socket connections, and authentication.
* `database.js`: MySQL connection pool and logic for user/session management.
* `vps_manager.html`: The frontend single-page application (React + Tailwind CSS via CDN).
* `package.json`: Project dependencies.

### 🚀 Installation & Setup

#### 1. Prerequisites

* Node.js (v14+)
* MySQL Server

#### 2. Database Setup

Create a database in MySQL (e.g., via phpMyAdmin or aaPanel):

|  |
| --- |
| ``` CREATE DATABASE vps_manager; ``` |

*Note: The application will automatically create the necessary tables (`users` and `sessions`) when it starts.*

#### 3. Configuration

Open `server.js` and edit the configuration section at the top:

|  |
| --- |
| ``` // Change this to a secure key for creating users const ADMIN_SECRET_KEY = 'MySecretAdminKey123';   const dbConfig = {     host: '127.0.0.1',     user: 'your_db_username',           password: 'your_db_password',       database: 'vps_manager'    }; ``` |

#### 4. Install Dependencies

Run the following command in the project directory:

|  |
| --- |
| ``` npm install ``` |

#### 5. Run the Server

|  |
| --- |
| ``` node server.js ``` |

The server will start on `http://localhost:3000`.

### 🛡️ How to Create Users (Admin)

Since this system is private, there is no public registration. You must create users via the Admin Panel.

1. Open the website.
2. On the Login screen, click the small link: **“Admin: Create User”**.
3. Enter the **Admin Secret Key** (configured in step 3).
4. Enter a generic Username and Password.
5. Go back to the Login screen and sign in with the new credentials.

## 🇨🇳 中文说明

**VPS Manager Pro** 是一个功能强大的网页版 SSH 终端，允许您通过浏览器管理多个 VPS 连接。它支持多用户系统、MySQL 会话存储，并且能够智能识别终端输出中的代理配置（VMess/SOCKS5）并生成二维码。

### ✨ 主要功能

* **网页 SSH 终端:** 使用 `xterm.js` 和 `socket.io` 提供完整的交互式终端体验。
* **多用户系统:** 安全的登录/注销系统，采用 `bcrypt` 加密。用户只能看到自己保存的服务器会话。
* **会话管理:** 保存、编辑和标记您的 VPS 连接信息，以便快速访问。
* **智能识别:** 自动检测终端输出中的 VMess 链接和 SOCKS5 账号密码，并生成二维码或复制按钮。
* **快捷工具:** 内置常用的 VPS 脚本按钮（EdNovas 工具箱，一键安装 VMess/TCP 等）。

### 📂 文件结构

* `server.js`: Node.js 后端主程序，处理 API 路由、SSH Socket 连接和用户验证。
* `database.js`: MySQL 连接池以及用户/会话管理的数据库逻辑。
* `vps_manager.html`: 前端单页应用（使用 CDN 加载 React 和 Tailwind CSS）。
* `package.json`: 项目依赖文件。

### 🚀 安装与设置

#### 1. 环境要求

* Node.js (v14+)
* MySQL 数据库

#### 2. 数据库设置

在 MySQL 中创建一个数据库（例如通过 phpMyAdmin 或 aaPanel）：

|  |
| --- |
| ``` CREATE DATABASE vps_manager; ``` |

*注意：程序启动时会自动创建所需的表（`users` 和 `sessions`）。*

#### 3. 修改配置

打开 `server.js` 并编辑顶部的配置部分：

|  |
| --- |
| ``` // 修改此密钥，用于创建新用户时的验证 const ADMIN_SECRET_KEY = 'MySecretAdminKey123';   const dbConfig = {     host: '127.0.0.1',     user: 'your_db_username',      // 你的数据库用户名     password: 'your_db_password',  // 你的数据库密码     database: 'vps_manager'        // 数据库名称 }; ``` |

#### 4. 安装依赖

在项目根目录下运行以下命令：

|  |
| --- |
| ``` npm install ``` |

#### 5. 启动服务器

|  |
| --- |
| ``` node server.js ``` |

服务器将在 `http://localhost:3000` 启动。

### 🛡️ 如何创建用户 (管理员)

为了安全起见，本系统没有公开注册功能。您必须通过管理员面板创建用户。

1. 打开网站。
2. 在登录界面，点击下方的小链接：\*\*”Admin: Create User”\*\*。
3. 输入 **Admin Secret Key**（在第 3 步中配置的密钥）。
4. 输入新的用户名和密码。
5. 返回登录界面，使用新创建的账号登录即可。

Author: EdNovas

Link: <https://ednovas.xyz/2025/12/31/vps-manager-pro/>

Copyright Notice: All articles in this blog are licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) unless stating additionally.

[VPS](/tags/VPS/)[Node.js](/tags/Node-js/)[SSH](/tags/SSH/)[Web Terminal](/tags/Web-Terminal/)[React](/tags/React/)

[![](https://cdn.jsdelivr.net/gh/wdm1732418365/CDN/New%20folder/Snipaste_2021-11-20_17-48-37.png)

### EdNovas云————高性价比，节点覆盖80+国家，多地区奈飞迪士尼等流媒体解锁](https://ednovas.me/)

[![cover of next post](https://github.com/user-attachments/assets/15392a90-9078-45b6-828d-829402669950)

Next Post

E视界 (DongguaTV Enhanced Edition)](/2025/12/31/dongguatv-enhanced/)

Related Articles

[![cover](https://cdn.jsdelivr.net/gh/ednovas/CDN/New%20folder/gigabit1.png)

2024-04-25

Server Gigabit Network](/2024/04/25/gigabit/ "Server Gigabit Network")

[![cover](https://cdn.jsdelivr.net/gh/ednovas/CDN/New%20folder/BlendHosting.jpg)

2023-12-14

Blend Hosting VPS](/2023/12/14/blendhosting/ "Blend Hosting VPS")

[![cover](https://cdn.jsdelivr.net/gh/wdm1732418365/CDN/New%20folder/cloudfront.png)

2022-10-28

使用 Cloudfront CDN](/2022/10/28/cloudfront/ "使用 Cloudfront CDN")

[![cover](https://cdn.jsdelivr.net/gh/wdm1732418365/CDN/New%20folder/v2-cdf7751ea1ef9db8d3073a857f493c4e_720w.jpg)

2022-10-28

使用 Gcore CDN](/2022/10/28/gcore/ "使用 Gcore CDN")

[![cover](https://cdn.jsdelivr.net/gh/wdm1732418365/CDN/New%20folder/bash-logo.jpg)

2022-01-02

EdNovas VPS脚本工具箱](/2022/01/02/ednovastool/ "EdNovas VPS脚本工具箱")

[![cover](https://cdn.jsdelivr.net/gh/wdm1732418365/CDN/New%20folder/vpsbg.webp)

2021-12-21

Vollcloud VPS测评](/2021/12/21/vollcloud/ "Vollcloud VPS测评")

---

Comment

Catalog

1. [1. Sample Website 演示网站](#Sample-Website-%E6%BC%94%E7%A4%BA%E7%BD%91%E7%AB%99)
2. [2. VPS Manager Pro (Web SSH Terminal)](#VPS-Manager-Pro-Web-SSH-Terminal)
   1. [2.1. 🇬🇧 English](#%F0%9F%87%AC%F0%9F%87%A7-English)
      1. [2.1.1. ✨ Key Features](#%E2%9C%A8-Key-Features)
      2. [2.1.2. 📂 File Structure](#%F0%9F%93%82-File-Structure)
      3. [2.1.3. 🚀 Installation & Setup](#%F0%9F%9A%80-Installation-amp-Setup)
         1. [2.1.3.1. 1. Prerequisites](#1-Prerequisites)
         2. [2.1.3.2. 2. Database Setup](#2-Database-Setup)
         3. [2.1.3.3. 3. Configuration](#3-Configuration)
         4. [2.1.3.4. 4. Install Dependencies](#4-Install-Dependencies)
         5. [2.1.3.5. 5. Run the Server](#5-Run-the-Server)
      4. [2.1.4. 🛡️ How to Create Users (Admin)](#%F0%9F%9B%A1%EF%B8%8F-How-to-Create-Users-Admin)
   2....