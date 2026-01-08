---
title: 黑客侦察能力训练营
url: https://mp.weixin.qq.com/s/sc65mewHQoFrZVJdneNaZQ
source: Doonsec's feed
date: 2026-01-07
fetch_date: 2026-01-08T03:30:49.426337
---

# 黑客侦察能力训练营

![cover_image](https://mmbiz.qpic.cn/mmbiz_jpg/niaMibh6Pic6Raxc3mq0JyxOiaoGpAObIME9GrTWuvTuqz4eGGwFo6Uq6SbVq7JicicRDgias7nFLYg9y8Hat75d8q0RA/0?wx_fmt=jpeg)

# 黑客侦察能力训练营

原创

破天KK

KK安全说

![]()

在小说阅读器中沉浸阅读

我就直说了吧：大多数白帽子侦察能力都很差。🤷‍♂️

没错，我亲眼目睹了无数才华横溢的黑客——比我聪明得多的人——因为在侦察阶段犯了同样的根本性错误，白白浪费了几个小时（有时甚至是几天）。

## “工具越多=结果越好”的陷阱🪤

通常情况下，黑客会这样行动：他们找到一个目标，比如说，然后`target.com`兴奋起来。他们立即启动终端，开始执行攻击计划：

```
subfinder -d target.com -o subdomains.txt
amass enum -d target.com >> subdomains.txt
assetfinder --subs-only target.com >> subdomains.txt
findomain -t target.com -u findomain_results.txt
chaos -d target.com -o chaos_subs.txt
```

然后他们进行排序、去重，并运行httpx：

```
cat *.txt | sort -u | httpx -threads 200 -o live_hosts.txt
```

他们恢复了 3400 个活跃的子域名，对所有内容运行内核：

```
nuclei -l live_hosts.txt -t ~ /nuclei-templates/ -o nuclei_results.txt
```

然后……一片寂静。🦗

他们有数据，很多数据，但没有发现任何漏洞，也没有理解其中缘由。只有一大堆他们不知道该如何处理的数据。

## 我到底经历了什么😅

去年，我参与了一个金融科技项目。项目规模大，收益丰厚，竞争也很激烈。我照例做了——动用了我所有的侦察工具：

```
# 我以前的“乱枪打鸟”式方法 🔫
 subfinder -d fintech-target.com -all -o subs.txt
amass enum -passive -d fintech-target.com -o amass.txt
cat subs.txt amass.txt | sort -u | httpx -silent -threads 200 | tee live.txt
cat live.txt | nuclei -t cves/ -t exposures/ -o nuclei.txt
```

我收集了大量数据，运行了自动扫描程序，然后开始随机探索。

两周过去了，我竟然一只虫子都没发现。一个都没有。😭

与此同时，另一位黑客在三天内就发现了该公司合作伙伴门户网站中的一个**严重IDOR漏洞。当我问他是怎么发现的（我们在同一个Discord群组里），他的回答让我震惊不已：**

*“我只查看了五个子域名。但我确实仔细查看了它们。我用 Burp 测试了它们，映射了每个端点，理解了它们的逻辑。”* 🎯

那感觉完全不一样。

## 误区：重广度轻深度📊

90% 的黑客都犯了一个错误：他们优先考虑**攻击范围而不是理解攻击内容**。

他们想在了解任何事情之前，先扫描所有内容。流程大致如下：

```
# 典型的故障工作流程❌
subdomains → httpx → nuclei → maybe ffuf → ???
```

但却没有人理解，没有人分析，只有自动化，然后是一片混乱。🤔

## 真正的优秀侦察是什么样的🔍

## 第一步：聚焦子域名发现🎯

我不会运行五个工具，最多只用一两个：

```
我主要使用 subfinder 来查找特定来源的
subfinder -d target.com -sources crtsh,alienvault -o subs_initial.txt
```

```
# 有时我会添加被动式
amass 枚举 -passive -d target.com -o amass_passive.txt# 合并和去重 ✨
cat subs_initial.txt amass_passive.txt | sort -u | tee all_subs.txt
```

这通常会给我带来 50-200 个子域名。管理起来很方便，不会让人感到不知所措。👌

## 步骤二：智能过滤🧠

我不会把所有东西都用 HTTPX 发送出去。实际上，我会筛选出有趣的内容：

```
# 查看当前在线服务并获取技术栈信息 🔧
cat all_subs.txt | httpx -silent -tech-detect -status-code -title -o live_detailed.txt
```

```
# 寻找有趣的模式 🔎
cat live_detailed.txt | grep -iE "admin|staging|dev|test|api|internal|vpn|jenkins|gitlab" | tee interesting.txt
```

现在我大概找到了 10 到 20 个真正值得调查的目标。🎲

## 步骤 3：深度端点发现 🕸️

大多数人都会在这里犯错。他们找到`admin-panel.target.com`SQL 注入漏洞后就立刻尝试，却从未事先了解过有哪些端点存在。🤦‍♂️

我这样做：

```
# 使用 gospider 抓取并查找端点 🕷️
 gospider -s "https://admin-panel.target.com" -o gospider_output -c 10 -d 3
```

```
# 提取 URL 和参数
cat gospider_output/* | grep -Eo "(http|https)://[a-zA-Z0-9./?=_-]*" | sort -u | tee endpoints.txt# 查找 JavaScript 文件 📜
cat gospider_output/* | grep "\.js" | tee js_files.txt# 运行 GAU（获取所有 URL）以获取历史端点 ⏰
echo "admin-panel.target.com" | gau --blacklist png,jpg,gif,css | tee gau_urls.txt
```

现在我看到了真正的攻击面。不仅仅是域名，还有**端点**。🗺️

## 第四步：JavaScript 分析（这才是关键⚡）

大多数黑客都会忽略这一点。这是个巨大的错误。🚫 JS 文件会泄露 API 接口、硬编码的密钥、逻辑缺陷——所有信息都会泄露。

```
# 下载所有 JS 文件 📥
cat js_files.txt | while read url; do wget -q " $url " -P js_files/; done
```

```
# 在 JS 中查找 API 端点 🔍
grep -r -E "api|endpoint|/v1/|/v2/" js_files/ | tee api_endpoints.txt# 寻找密钥 🔑
grep -r -iE "api_key|apikey|secret|token|password|aws_access" js_files/ | tee secrets.txt# 查找有趣的参数 🎛️
grep -r -E "\?[a-zA-Z_]+=|&[a-zA-Z_]+=" js_files/ | tee parameters.txt
```

## 第五步：使用 Burp 进行手动探索 🔥

这就是奇迹发生的地方。我会将所有请求都通过 Burp Suite 进行代理，并且真正地使用这个应用程序：

```
#将Burp 设置为系统代理（在Linux 系统上）🐧
export http_proxy=http: //127.0.0.1:8080
export https_proxy=http: //127.0.0.1 :8080
```

然后我就……到处点点。创建账号。尝试各种功能。用 Burp 查看 HTTP 历史记录。👀

我在找：

* ✅ 响应中的隐藏参数
* ✅ 未记录的 API 端点
* ✅ 身份验证检查不一致
* ✅ 有趣的标题或 cookie

## 一个使用命令的真实示例💰

举个具体的例子。我当时在研究一家SaaS公司的漏洞赏金计划。以下是我具体操作的步骤：

```
# 步骤 1：基本侦察 🎯
 subfinder -d saas-company.com -o subs.txt
cat subs.txt | httpx -silent -status-code -title | tee live.txt
```

```
# 发现了一个有趣的子域名：api-internal.saas-company.com
# 大多数人都会略过它，但我没有。😎# 步骤 2：创建账户并通过 Burp 进行代理 🔍
# 注意到 API 调用发送到 api-internal.saas-company.com/v2/# 步骤 3：使用 ffuf 发现端点 💥
ffuf -w ~/wordlists/api-endpoints.txt -u https://api-internal.saas-company.com/v2/FUZZ -mc 200,401,403已找到：/v2/users、/v2/teams、/v2/admin/reports 📋# 步骤 4：测试 /v2/admin/reports 无需身份验证
curl -X GET "https://api-internal.saas-company.com/v2/admin/reports" \
  -H "Content-Type: application/json"# 返回结果：401 未授权 ❌# 第 5 步：尝试使用我的常规用户令牌 🎫
curl -X GET "https://api-internal.saas-company.com/v2/admin/reports" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLC..." \
  -H "Content-Type: application/json"# BOOM：所有用户PII数据均已通过验证💣
# 授权失败 = 严重IDOR ✅
```

## 我当前的侦察脚本🛠️

我编写了一个简单的 bash 脚本来体现这种理念：

```
#!/bin/bash
#focused_recon.sh 🎯
```

```
TARGET=$1if [ -z "$TARGET" ]; then
    echo "Usage: ./focused_recon.sh target.com"
    exit 1
fiecho "[+] Starting focused recon on $TARGET 🚀"# Subdomain discovery 🔍
echo "[+] Finding subdomains..."
subfinder -d $TARGET -silent -o subs.txt# Check live hosts with tech detection 💻
echo "[+] Checking live hosts..."
cat subs.txt | httpx -silent -tech-detect -status-code -title -o live.txt# Filter interesting ones 🎯
echo "[+] Filtering interesting targets..."
cat live.txt | grep -iE "admin|staging|dev|test|api|internal" | tee interesting.txt# Crawl each interesting target 🕷️
echo "[+] Crawling interesting targets..."
while read url; do
    echo "[+] Crawling $url"
    gospider -s "$url" -o crawl_output -c 10 -d 2 -t 10
done < interesting.txt# Extract JS files 📜
echo "[+] Extracting JS files..."
grep -r "\.js" crawl_output/ | grep -Eo "(http|https)://[a-zA-Z0-9./?=_-]*\.js" | sort -u | tee js_urls.txt# Download and analyze JS 🔎
echo "[+] Analyzing JavaScript files..."
mkdir -p js_files
cat js_urls.txt | while read js_url; do
    wget -q "$js_url" -P js_files/
doneecho "[+] Looking for secrets in JS... 🔑"
grep -r -iE "api_key|apikey|secret|token|password" js_files/ | tee secrets_found.txtecho "[+] Looking for API endpoints... 🗺️"
grep -r -E "api/|/v1/|/v2/|endpoint" js_files/ | tee api_endpoints.txtecho "[+] Recon complete! ✅ Check the outputs:"
echo "    - interesting.txt (focus here first! 🎯)"
echo "    - secrets_found.txt 🔑"
echo "    - api_endpoints.txt 🗺️"
```

用法：

```
chmod +x focused_recon.sh
./focused_recon.sh target.com
```

这样我就得到了一个重点突出、值得深入调查的清单，而不是被海量数据淹没。💪

## 我使用的高级技巧 🔥

## 1. 使用 Arjun 进行参数发现 🎯

当我发现一个有趣的端点时，我会使用 Arjun 来发现隐藏的参数：

```
arjun -u https://api.target.com/v1/users/profile -m GET -o arjun_params.txt
```

这发现了很多导致漏洞的隐藏参数。🐛

## 2. 使用 ffuf 进行模糊测试💥

API枚举：

```
# Fuzz API 版本 🔢
 ffuf -w <( seq 1 10) -u https://api.target.com/vFUZZ/users -mc 200,401,403
```

```
# 模糊测试端点 🎲
ffuf -w ~/wordlists/api_endpoints.txt -u https://api.target.com/v2/FUZZ -mc all -fc 404# 模糊测试参数 🎛️
ffuf -w ~/wordlists/parameters.txt -u "https://target.com/api/user?FUZZ=test" -mc all -fr "error|invalid"
```

## 3. 互联网档案馆：历史终点站⏰

```
# 获取所有历史 URL 📚
echo "target.com" | waybackurls | tee wayback.txt
```

```
# 筛选有趣的模式 🔍
cat wayback.txt | grep -E "\.json|\.xml|\.conf|\.sql|\.bak|admin|api" | tee wayback_interesting.txt# 测试它们是否仍然有效 ✅
cat wayback_interesting.txt | httpx -silent -status-code -mc 200
```

## 4. GitHub Dorking 泄露的秘密 🔑

```
#  Use github-search tool 🔎
github-search -d target.com -t $GITHUB_TOKEN -o github_results.txt
```

```
# Or manual dorks
# Search: "target.com" api_key 🔑
# Search: "target.com" password 🔒
# Search: "target.com" filename:.env 📄
```

```
你需要转变的思维模式🧠
```

别再想着：“我能找到多少个子域名？”❌

开始思考：“我对这个子域名究竟了解多少？” ✅

你的终端命令应该体现出理解，而不仅仅是数据收集：

**糟糕的做法：** 😵

```
huge_tool_output.txt → ?? ?
```

**好方法：** 😎

```
focused_discovery.txt → manual_analysis → testing → profit 💰
```

## 实际操作流程

## 1. 运行聚焦子域名发现（5-10分钟）⏱️

```
subfinder -d target.com -o subs.txt
cat subs.txt | httpx -silent -tech-detect | grep -iE "admin|api|dev" | tee interesting.txt
```

## 2. 选择最有趣的子域名🎯

（基于关键词、技术栈、状态码）

## 3. 深度潜水 1-2 小时：🏊‍♂️

```
# 彻底爬取 🕸️
 gospider -s "https://interesting-sub.target.com" -d 3 -c 10 -o crawl/
```

```
# 提取并分析 JS 代码 📜
# 下载 JS 文件 📥
# 使用 grep 命令查找密钥和端点 🔍# 手动尝试应用👆
# 查看 Burp HTTP 历史记录👀
# 地图功能🗺️
```

## 4. 记录一切📝

```
# 我真的只用了一个简单的...