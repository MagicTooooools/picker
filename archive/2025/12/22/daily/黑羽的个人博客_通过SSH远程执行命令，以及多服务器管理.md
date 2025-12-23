---
title: 通过SSH远程执行命令，以及多服务器管理
url: https://blog.thetbw.xyz/archives/manage-multi-server-using-rpcssh
source: 黑羽的个人博客
date: 2025-12-22
fetch_date: 2025-12-23T03:28:46.799753
---

# 通过SSH远程执行命令，以及多服务器管理

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

![通过SSH远程执行命令，以及多服务器管理](https://thetbw-hk.cos.thetbw.xyz/blog/1ca6c6ef130f912cce794453ef4b6ce4_1766395582485.jpg)

通过SSH远程执行命令，以及多服务器管理

132 次访问

发布: 2025-12-22

最后编辑: 2025-12-22

SSH 除了可以直接连接远程服务器，以终端形式显示外，也可以直接用来执行命令，只需要把命令放到 `ssh` 的参数后面即可，例如：
```shell
ssh root@example "echo Hello"
```
这对于执行一些临时命令，例如运行备份之类的，还挺有用。
对于很长的或者多个命令，可以使用 `<<` 多行文本，例如
```shell
ssh root@example << HERE
echo "HELLO"
echo $USER
HERE
```
但是对于上面的第二个命令，`$USER`，存在转义问题，`$USER` 会在本地执行，而不是在服务器上。
解决方式是可以添加引号来避免本地提前解释执行。
```shell
ssh root@example << 'HERE'
echo "HELLO"
echo $USER
HERE
```
此处是来自 POSIX 的规范，linux 和 mac 同样适用，具体出处我也不清楚，可以参考文章底部最后一个链接，和自行 ai 搜索
---
还有一种解决方法，也是我想说的，来自今天搜索 `rpcssh`，发现的一个十多年的帖子。
`declare` 命令可以返回变量或者方法的定义，使用这种方法，就可以自己不用写相关转义了。例如本地有个下面的方法：
```shell
hello() {
echo "Hello, world, I'm coming from $(uname -n)."
}
```
可以这样执行
```shell
ssh root@example"$(declare -f hello); hello"
```
`$(declare -f hello)` 会自动把本地定义的这个`hello`方法的内容获取出来，并且发送到目标服务器时自动转义，然后在远程服务器执行这个`hello`方法。
> 注意这种方法只能用于 shell 中定义的方法，不能是二进制文件
原作者写了一个叫`rpcsh`的方法，用于把本地方法发送到远程，挺简洁方便的，我就直接放在下面了
```shell
# rpcsh -- Runs a function on a remote host
# This function pushes out a given set of variables and functions to
# another host via ssh, then runs a given function with optional arguments.
# Usage:
# rpcsh -h remote\_host [ -p ssh-port ] -u remote\_login -v "variable list" \
# -f "function list" -m mainfunc
#
# The "function list" is a list of shell functions to push to the remote host
# (including the main function to execute, and any functions that it calls)
# Use the "variable list" to send a group of variables to the remote host.
# Finally "mainfunc" is the name of the function (from "function list")
# to execute on the remote side. Any additional parameters specified gets
# passed along to mainfunc.
rpcsh() {
if ! args=("$(getopt -l "rmthost:,rmthostport:,rmtlogin:,pushvars:,pushfuncs:,rmtmain:" -o "h:p:u:v:f:m:A" -- "$@")")
then
exit 1
fi
sshvars=( -q -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null )
eval set -- "${args[@]}"
while [ -n "$1" ]
do
case $1 in
-h|--rmthost) rmthost=$2; shift; shift;;
-p|--rmtport) sshvars=( "${sshvars[@]}" -p $2 ); shift; shift;;
-u|--rmtlogin) rmtlogin=$2; shift; shift;;
-v|--pushvars) pushvars=$2; shift; shift;;
-f|--pushfuncs) pushfuncs=$2; shift; shift;;
-m|--rmtmain) rmtmain=$2; shift; shift;;
-A) sshvars=( "${sshvars[@]}" -A ); shift;;
-i) sshvars=( "${sshvars[@]}" -i $2 ); shift; shift;;
--) shift; break;;
esac
done
rmtargs=( "$@" )
ssh ${sshvars[@]} ${rmtlogin}@${rmthost} "
$(declare -p rmtargs 2>/dev/null)
$([ -n "$pushvars" ] && declare -p $pushvars 2>/dev/null)
$(declare -f $pushfuncs 2>/dev/null)
$rmtmain \"\${rmtargs[@]}\"
"
}
```
> 原地址 https://gist.github.com/derekp7/9978986
---
有什么用呢，如果你有多个服务器，需要维护什么备份脚本什么，按照传统的方法，比如把脚本上传到服务器，或者git，当你的脚本更新时候，所有的服务器上的文件都需要更新，你需要一个个登录过去获取最新文件，这显然不是很方便。
当然你可以使用类似宝塔面板这种，可以在一个地方控制好几个服务器，这显然又增加了复杂性，增加了依赖。
还有就是一个干净服务器，通常需要装一些东西，进行初始化，例如我想安装`docker`，`fail2ban`，然后配置相关参数，每次都要这么设置都挺费事的，就可以用上面的方法。
例如下面的脚本
```shell
#!/bin/bash
set -euo pipefail
# 定义远程服务器列表（可根据实际需求修改）
REMOTE\_SERVERS=("192.168.2.1" "192.168.2.2")
# 默认SSH端口
DEFAULT\_SSH\_PORT=22
# 默认SSH登录用户
DEFAULT\_SSH\_USER="root"
# ===================== 核心远程执行函数 =====================
rpcsh() {
local rmthost="" rmtport="${DEFAULT\_SSH\_PORT}" rmtlogin="${DEFAULT\_SSH\_USER}"
local pushvars="" pushfuncs="" rmtmain="" sshvars=() rmtargs=()
local use\_ssh\_agent=0 ssh\_identity=""
# 解析命令行参数
if ! args=$(getopt -o h:p:u:v:f:m:Ai: --long rmthost:,rmthostport:,rmtlogin:,pushvars:,pushfuncs:,rmtmain: -- "$@"); then
echo "ERROR: 参数解析失败" >&2
return 1
fi
eval set -- "${args}"
sshvars=( -q -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ConnectTimeout=10 )
while [[ $# -gt 0 ]]; do
case "$1" in
-h|--rmthost)
rmthost="$2"
shift 2
;;
-p|--rmtport)
rmtport="$2"
sshvars+=( "-p" "$2" )
shift 2
;;
-u|--rmtlogin)
rmtlogin="$2"
shift 2
;;
-v|--pushvars)
pushvars="$2"
shift 2
;;
-f|--pushfuncs)
pushfuncs="$2"
shift 2
;;
-m|--rmtmain)
rmtmain="$2"
shift 2
;;
-A)
use\_ssh\_agent=1
sshvars+=( "-A" )
shift
;;
-i)
ssh\_identity="$2"
sshvars+=( "-i" "$2" )
shift 2
;;
--)
shift
rmtargs=( "$@" )
break
;;
\*)
echo "ERROR: 未知参数 $1" >&2
return 1
;;
esac
done
# 校验必要参数
if [[ -z "${rmthost}" || -z "${rmtmain}" ]]; then
echo "ERROR: 必须指定远程主机(--rmthost/-h)和执行函数(--rmtmain/-m)" >&2
return 1
fi
# 构建远程执行脚本
local remote\_script=""
# 传递参数数组
remote\_script+="$(declare -p rmtargs 2>/dev/null); "
# 传递指定变量
if [[ -n "${pushvars}" ]]; then
remote\_script+="$(declare -p ${pushvars} 2>/dev/null); "
fi
# 传递指定函数
if [[ -n "${pushfuncs}" ]]; then
remote\_script+="$(declare -f ${pushfuncs} 2>/dev/null); "
fi
# 执行主函数
remote\_script+="${rmtmain} \"\${rmtargs[@]}\""
# 执行SSH远程命令
echo "INFO: 连接到 ${rmtlogin}@${rmthost}:${rmtport} 执行 ${rmtmain}"
ssh "${sshvars[@]}" "${rmtlogin}@${rmthost}" "${remote\_script}"
}
# ===================== 业务功能函数 =====================
# 安装Docker（含Docker Compose）
install\_docker() {
echo "===== 开始安装Docker ====="
if command -v docker &>/dev/null; then
echo "INFO: Docker已安装，跳过安装步骤"
return 0
fi
# 卸载旧版本
apt-get remove -y docker docker-engine docker.io containerd runc || true
# 设置仓库
apt-get update
apt-get install -y ca-certificates curl gnupg lsb-release
mkdir -p /etc/apt/trusted.gpg.d
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/trusted.gpg.d/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb\_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list >/dev/null
# 安装Docker Engine
apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
# 启动并开机自启
systemctl enable --now docker
# 验证安装
if docker --version &>/dev/null; then
echo "SUCCESS: Docker安装完成"
else
echo "ERROR: Docker安装失败" >&2
return 1
fi
}
# 安装fail2ban
install\_fail2ban() {
echo "===== 开始安装fail2ban ====="
if command -v fail2ban-server &>/dev/null; then
echo "INFO: fail2ban已安装，跳过安装步骤"
return 0
fi
apt-get update
apt-get install -y fail2ban
systemctl enable --now fail2ban
if fail2ban-server --version &>/dev/null; then
echo "SUCCESS: fail2ban安装完成"
else
echo "ERROR: fail2ban安装失败" >&2
return 1
fi
}
# 配置fail2ban（SSH防护为例）
config\_fail2ban() {
echo "===== 开始配置fail2ban ====="
local config\_file="/etc/fail2ban/jail.local"
# 备份原有配置
if [[ -f "${config\_file}" ]]; then
cp "${config\_file}" "${config\_file}.bak.$(date +%Y%m%d%H%M%S)"
fi
# 写入基础配置
cat > "${config\_file}" << EOF
[DEFAULT]
ignoreip = 127.0.0.1/8 192.168.0.0/16
bantime = 86400
findtime = 3600
maxretry = 5
banaction = iptables-multiport
backend = systemd
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
EOF
# 重启服务
systemctl restart fail2ban
if systemctl is-active --quiet fail2ban; then
echo "SUCCESS: fail2ban配置完成并已重启"
else
echo "ERROR: fail2ban配置后启动失败" >&2
return 1
fi
}
# 服务器备份（示例：备份/etc目录到/backup）
server\_backup() {
echo "===== 开始服务器备份 ====="
local backup\_dir="/backup/$(date +%Y%m%d)"
local backup\_file="${backup\_dir}/etc\_backup.tar.gz"
# 创建备份目录
mkdir -p "${backup\_dir}"
# 备份/etc目录
tar -zcf "${backup\_file}" /etc \
--exclude=/etc/ssh/ssh\_host\_\* \
--exclude=/etc/machine-id \
--exclude=/etc/resolv.conf
# 校验备份文件
if [[ -f "${backup\_file}" && $(stat -c%s "${backup\_file}") -gt 1024 ]]; then
echo "SUCCESS: 备份完成，文件路径: ${backup\_file}"
# 可选：保留最近7天备份
find /backup -type d -mtime +7 -delete
else
echo "ERROR: 备份失败" >&2
return 1
fi
}
# ===================== 脚本入口/帮助信息 =====================
# 显示帮助信息
show\_help() {
cat << EOF
服务器管理脚本 - server\_manager
用法:
$0 [选项] <命令> [命令参数]
可用命令:
install\_docker 安装Docker及Docker Compose
install\_fail2ban 安装fail2ban
config\_fail2ban 配置fail2ban（SSH防护）
server\_backup 执行服务器备份（默认备份/etc目录）
help 显示此帮助信息
远程执行选项:
-r 远程执行模...