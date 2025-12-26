---
title: Debian 13 新机初始化记录
url: https://www.himiku.com/archives/debian-sop.html
source: 初之音
date: 2025-12-25
fetch_date: 2025-12-26T03:27:01.358115
---

# Debian 13 新机初始化记录

[初之音](https://www.himiku.com/)
[首页](https://www.himiku.com/)
[追番](https://www.himiku.com/bangumi/)[照片](https://www.himiku.com/photos/)[友链](https://www.himiku.com/links/)[归档](https://www.himiku.com/archives/)[留言](https://www.himiku.com/chat/)[关于](https://www.himiku.com/about/) 分类

* [日常](https://www.himiku.com/category/nichijou/)
* [玩乐](https://www.himiku.com/category/tech/)
* [博客](https://www.himiku.com/category/blog/)
* [动漫](https://www.himiku.com/category/acg/)
* [开箱](https://www.himiku.com/category/show/)
* [游戏](https://www.himiku.com/category/game/)
* [小说](https://www.himiku.com/category/novel/)
* [生物](https://www.himiku.com/category/bio/)
更多

* [编年史](/history)
* [电报频道](https://t.me/mikusa_news)
* [SteinsGate](/tools/steinsgate/)
* [黑塔转圈圈](/tools/heita/)
* [卢浮宫生成器](/tools/one-last-image/)
* [初之图库](https://liulipic.net)
* [捐赠答谢](/thanks)

搜索

Go!

[追番](https://www.himiku.com/bangumi/)[照片](https://www.himiku.com/photos/)[友链](https://www.himiku.com/links/)[归档](https://www.himiku.com/archives/)[留言](https://www.himiku.com/chat/)[关于](https://www.himiku.com/about/)

[日常](https://www.himiku.com/category/nichijou/)[玩乐](https://www.himiku.com/category/tech/)[博客](https://www.himiku.com/category/blog/)[动漫](https://www.himiku.com/category/acg/)[开箱](https://www.himiku.com/category/show/)[游戏](https://www.himiku.com/category/game/)[小说](https://www.himiku.com/category/novel/)[生物](https://www.himiku.com/category/bio/)

[编年史](/history)[电报频道](https://t.me/mikusa_news)[SteinsGate](/tools/steinsgate/)[黑塔转圈圈](/tools/heita/)[卢浮宫生成器](/tools/one-last-image/)[初之图库](https://liulipic.net)[捐赠答谢](/thanks)

Debian 13 新机初始化记录 - 初之音

# Debian 13 新机初始化记录

[mikusa](https://www.himiku.com/author/1/)  •
2025-12-25
 • 0 评论
 • 111 阅读

最近购置了一台 1H1G、20G SSD 的虚拟服务器，为了便于使用，需要配置一下基础环境，例如新建用户、密钥登录、Docker 和 Zsh 等等。

年初参考过《[Debian Server 初始化设置 SOP](https://blog.xm.mk/posts/89da/)》的流程，也整理了一份《[飞牛 fnOS 初始化配置记录](https://www.himiku.com/archives/fnos-sop.html)》，但都不太适合直接套用到这台机子上。部分命令在 Debian 13 上也不再适用。于是，我决定按照自己的需求重新抄写一份，并用这篇笔记为今年的水文画上句号。

~~抄过来就是我自己的东西了！~~

## 系统

**本节命令全部使用 root 用户执行。**

### 更新与升级

官方预装的是 Debian 12，系统相当干净，没有多余的东西，因而无需重装系统。既然是新开通的实例，没有任何业务负担，那么可以放心地进行大版本升级。我决定趁现在就把它更新到最新的 Debian13，满足我积攒已久的升级欲望。

首先，升级系统到最新状态：

```
apt update
apt upgrade -y
apt full-upgrade -y
apt autoremove -y
apt autoclean
```

接着，将软件源切换到 Debian 13（trixie），使用以下命令一键修改 `/etc/apt/sources.list`：

```
sed -i 's/bookworm/trixie/g' /etc/apt/sources.list /etc/apt/sources.list.d/*.{list,sources} 2>/dev/null
```

然后再次升级系统：

```
apt update
apt upgrade -y
apt full-upgrade -y
apt autoclean
apt autoremove -y
```

弹出的选框可以全部按默认选择，待更新完毕后，重启系统：

```
reboot
```

重启后，验证系统版本：

```
lsb_release -a
```

如果显示类似以下信息，说明已经升级成功：

```
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 13 (trixie)
Release:        13
Codename:       trixie
```

### 本地化配置

设置时区与 NTP：

```
timedatectl set-timezone Asia/Hong_Kong
timedatectl set-ntp true
timedatectl status
```

输出类似：

```
System clock synchronized: yes
NTP service: active
```

配置语言环境：

```
sed -i 's/^# *zh_CN.UTF-8 UTF-8/zh_CN.UTF-8 UTF-8/' /etc/locale.gen
sed -i 's/^# *en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
locale-gen
```

输出类似：

```
Generating locales (this might take a while)...
  en_US.UTF-8... done
  zh_CN.UTF-8... done
  en_US.UTF-8... done
Generation complete.
```

### 安装常用工具

安装一些基础工具，涵盖编译环境、系统管理、网络下载、终端增强以及文件处理等~~我可能用得到但又用不到的~~功能：

```
apt install -y build-essential sudo vim curl wget ufw autojump git tmux zsh tree zstd zip unzip lsof fastfetch rsync
```

### 新建用户

先设置一个新用户的环境**变量**，例如 `mikusa`，用于后续复制粘贴快速执行命令：

```
USERNAME=mikusa
```

创建该用户，并将其添加到 sudo 用户组：

```
useradd -m $USERNAME
usermod -aG sudo $USERNAME
```

为用户设置本地密码（可选，以防特殊情况需要，密码尽量复杂）：

```
passwd $USERNAME
```

设置该用户执行 `sudo` 命令时免密：

```
echo "$USERNAME ALL=(ALL:ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/$USERNAME-nopasswd > /dev/null
chmod 440 /etc/sudoers.d/$USERNAME-nopasswd
```

为该用户配置密钥登录，我有现成的公钥，所以这里直接设置一个公钥**变量**：

```
PUBKEY="ssh-ed25519 AAAABBBBBVVVVVVVVVVVVVVVVVVVV"
```

创建用户密钥文件夹并授权：

```
mkdir -p /home/$USERNAME/.ssh
chmod 700 /home/$USERNAME/.ssh
```

导入现有公钥：

```
echo "$PUBKEY" | sudo tee /home/$USERNAME/.ssh/authorized_keys > /dev/null
```

修改权限：

```
chmod 600 /home/$USERNAME/.ssh/authorized_keys
chown -R $USERNAME:$USERNAME /home/$USERNAME/.ssh
```

### 安装 Docker

使用官方命令一键安装：

```
curl -fsSL https://get.docker.com | bash -s docker
```

安装完毕后，使用以下命令验证 docker 和 docker compose：

```
docker info
docker compose version
```

再将个人用户添加到 docker 用户组：

```
usermod -aG docker $USERNAME
```

> 需用户退出当前终端、重新登录后，docker 组权限才会生效。

## 用户

**切换到 mikusa 用户**：

```
su - mikusa
```

才能继续执行下列个人用户相关的配置。

### Zsh 配置

使用脚本一键安装 Oh My Zsh ：

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

安装后会询问是否切换 Shell 为 Zsh，按 `y` 同意。

安装刚需插件：

```
git clone https://github.com/zsh-users/zsh-autosuggestions.git ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
```

修改主题为 `ys`：

```
sed -i 's/^ZSH_THEME=".*"/ZSH_THEME="ys"/' ~/.zshrc
```

配置 Zsh，启用插件：

```
echo ". /usr/share/autojump/autojump.sh" >> ~/.zshrc
sed -i.bak 's/plugins=(\(.*\))/plugins=(\1 autojump zsh-autosuggestions zsh-syntax-highlighting z extract sudo cp aliases docker docker-compose)/' ~/.zshrc
```

添加一些自用 docker 别名：

```
cat <<'EOF' >> ~/.zshrc

# Docker aliases
alias dcu="docker compose up -d --remove-orphans"     ## 启动服务并移除多余容器
alias dcd="docker compose down"                        ## 停止并删除服务容器
alias dcp="docker compose pull"                        ## 拉取镜像
alias dps="docker ps"                                   ## 查看运行中的容器
alias dlogs="docker logs --follow -n 15"               ## 查看容器日志，跟随最新 15 条
alias dclean="docker image prune -a"                   ## 清理未使用镜像
alias acme.sh="docker exec acme.sh acme.sh"            ## 在 acme.sh 容器中执行 acme.sh
alias nginx="docker exec nginx nginx"                  ## 在 nginx 容器中执行 nginx 命令
alias caddy-reload="docker exec -w /etc/caddy caddy sh -c 'caddy fmt --overwrite && caddy reload'"  ## 格式化 Caddyfile 并重载 Caddy
alias caddy="docker exec caddy caddy"                  ## 在 Caddy 容器中执行 caddy 命令

EOF
```

再重载 Zsh：

```
source ~/.zshrc
```

### 其他

可以安装一个 `trash-cli` 作回收站用：

```
sudo apt install -y trash-cli
```

追加别名：

```
cat <<'EOF' >> ~/.zshrc

# trash-cli aliases
alias rm='trash-put'        ## trash-put 将文件或目录移入回收站
alias rmclean='trash-empty' ## trash-empty 清空回收站
alias rmrest='trash-restore' ## trash-restore 还原回收站中的文件
alias rmlist='trash-list'   ## trash-list 列出回收站中的文件
alias rmrm='trash-rm'       ## trash-rm 删除回收站中的单个文件

EOF
```

后续使用 `rm` 命令，便无须担心误操作删除文件了。

## 最后

使用新用户通过密钥连接上实例，并确认一切正常后，再修改 SSH 配置，**避免因误操作而失联**。

先修改默认的 SSH 端口。设置一个端口**变量**，例如：

```
PORT=22033
```

一键替换当前端口：

```
sudo sed -i "s/^#\?Port .*/Port $PORT/" /etc/ssh/sshd_config
```

验证 `/etc/ssh/sshd_config` 配置是否有误：

```
sshd -t
```

没有报错的话，重启 SSH：

```
systemctl reload sshd
```

测试是否已成功绑定到新端口：

```
ss -tlnp | grep ssh
```

有类似以下输出，即表示修改完毕：

```
root@Reze:~# ss -tlnp | grep ssh
LISTEN 0      128          0.0.0.0:22033      0.0.0.0:*    users:(("sshd",pid=733,fd=6))
LISTEN 0      128             [::]:22033         [::]:*    users:(("sshd",pid=733,fd=7))
```

再考虑禁用 root 登录：

```
sudo sed -i 's/^#\?PermitRootLogin .*/PermitRootLogin no/' /etc/ssh/sshd_config
```

也可以禁用密码登录：

```
sudo sed -i 's/^#\?PasswordAuthentication .*/PasswordAuthentication no/' /etc/ssh/sshd_config
```

使用以下命令查看是否已成功修改：

```
sudo sshd -T | grep -E 'permitrootlogin|passwordauthentication'
```

若输出：

```
permitrootlogin no
passwordauthentication no
```

即表示禁止 root 登录，也禁止密码登录生效。

随后重载 SSH：

```
systemctl reload sshd
```

> 使用 reload 而非 restart，可避免因配置错误断开当前连接。

最后，在确认新端口和密钥登录都可用后，再断开当前连接。

## 参考

* 烧饼博客：[Debian 12 Bookworm 升级 Debian 13 Trixie](https://u.sb/debian-upgrade-13/)
* Cyrus's Blog：[Debian Server 初始化设置 SOP](https://blog.xm.mk/posts/89da/)
* [ChatGPT](https://chatgpt.com/)

---

> **本文作者：**[mi...