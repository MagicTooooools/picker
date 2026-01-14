---
title: 使用burpMCP和kaliMCP自动化渗透
url: https://mp.weixin.qq.com/s/uV_-cHATPpRYxrfQ6cMfMw
source: Doonsec's feed
date: 2026-01-13
fetch_date: 2026-01-14T03:36:52.013793
---

# 使用burpMCP和kaliMCP自动化渗透

![cover_image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVIZLzovmib5FGOLbQ2kbIiahcgSkFfYy6Icmp9TuJgBC82lsV5dhk4jFg/0?wx_fmt=jpeg)

# 使用burpMCP和kaliMCP自动化渗透

原创

低价考证，滴滴→

Z2O安全攻防

![]()

在小说阅读器中沉浸阅读

点击上方[蓝字]，关注我们

**建议大家把公众号“Z2O安全攻防”设为星标，否则可能就看不到啦！**因为公众号现在只对常读和星标的公众号才能展示大图推送。操作方法：点击右上角的【...】，然后点击【设为星标】即可。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuao3T9EnGbUIqxgDhEVicCV8NbH4FiaZ3YIbpXNEr6qFicGkAelnQHKGHsVlfapMGgO3DHA68iaiac0n4Q/640?wx_fmt=png)

# 免责声明

本文仅用于技术讨论与学习，利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，文章作者及本公众号团队不为此承担任何责任。

# 文章正文

本文需要用到的工具：

较高版本burp ：https://pan.quark.cn/s/75512e24ef79

kali mcp : https://github.com/Wh0am123/MCP-Kali-Server

AI IDE ，这里使用trae :   https://www.trae.cn/

## Burp MCP 配置

### burp

burp 扩展商店搜索 mcp server 安装

![image-20260111141505435](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjV4hjUDSlX2Qmpn6LiaicgnQ6kuicEjF5ALFhJndanh0KLRqTeTE3sWlG7A/640?wx_fmt=png&from=appmsg)

然后在MCP标签页启动即可：

![image-20260111141535868](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjV6P57ficJSMJlPy4aIA4TtZVK0bUJhveQyHcxsAV3X2yfVfqD0fH3bBA/640?wx_fmt=png&from=appmsg)

### trae

创建一个智能体

![image-20260111142404952](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVj2Dgp87pl8ujkWmicGoDppLlicgEbFYbWCH57XBCP7R6turMIdRa0LRw/640?wx_fmt=png&from=appmsg)
> ❝
>
> 渗透测试prompt
>
> ```
> 你是一位资深的实战渗透测试专家，严格遵守国家网络安全法律法规，熟悉渗透测试标准流程，精通各种网络攻击手法，能够为企业提供合规、全面、可落地的渗透测试服务。你的所有操作均基于客户正式授权，目标是发现漏洞、评估风险、给出整改建议，而非破坏系统或窃取数据。
> ```
>
> CTF prompt
>
> ```
> 你是一位顶尖的 CTF 渗透攻击专家，精通 Web 安全、二进制安全、逆向工程、密码学、取证分析五大核心题型，熟悉各类漏洞的原理、利用技巧和工具链，能够快速拆解题目、定位漏洞、构造 exp 并提取 Flag。你的所有操作均基于 CTF 竞赛的隔离环境，无需考虑合规性，目标是高效解题。
> ```
>
> ❞

手动添加burp mcp:

```
{

  "mcpServers": {

    "burpsuite": {

      "url": "http://127.0.0.1:9876/sse"

    }

  }

}
```

![image-20260111142536739](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVReIo6edaNvbPRUOmHE1NoCj3YXib5oicVY8ibYtb8yoRdUv89gGhR0wrA/640?wx_fmt=png&from=appmsg)

## Kali MCP 配置

https://github.com/Wh0am123/MCP-Kali-Server

### kali

```
git clone https://github.com/Wh0am123/MCP-Kali-Server.git

cd MCP-Kali-Server

pip install -r requirements.txt

python3 kali_server.py --ip 0.0.0.0
```

![image-20260111142846333](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVrl68k3vILszHrEaJhMnCWZZiaZM4xicsGeK2cvE8cNDbibQB6dvlPibrBg/640?wx_fmt=png&from=appmsg)

### trae所在机器

```
git clone https://github.com/Wh0am123/MCP-Kali-Server.git

cd MCP-Kali-Server

pip install -r requirements.txt
```

在前面创建的智能体添加kali mcp :

```
{

  "mcpServers": {

    "kali-mcp": {

      "command": "cmd",

      "args": [

        "/c",

        "python3",

        "C:\\xxxxx\\MCP-Kali-Server-main\\mcp_server.py",

        "--server",

        "http://192.168.111.164:5000"

      ]

    }

  }

}
```

![image-20260111143314678](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVSxiaaBCf2tN1d9ibbPxFYfgJpz4KugiaZ1beibZzkPrxTDUvcsiaZ5cp0lA/640?wx_fmt=png&from=appmsg)

## 测试

可选模型：

![image-20260111144144122](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVD7BMuSlueibgI4libX4kFEOEUKLOovUibCKANoN6HxYGZk60nk4hzPr9A/640?wx_fmt=png&from=appmsg)

### WEB

```
这是一个CTF web题目，调用burp mcp 和kali mcp 及适当的方法，解题找出flag：

[网鼎杯 2020 青龙组]AreUSerialz1  靶机信息 http://71f99dc6-88d3-4584-9e73-c76624492c5d.node5.buuoj.cn:81
```

![image-20260111153745056](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVuibjCIqOUd4Ux2po4WSyxibfWDNyNxpmZ7Lggbc5d8kp4icarpR0YbmKw/640?wx_fmt=png&from=appmsg)![image-20260111153857737](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVJhMXTPdicHWWOwfMqRIr9sWicialItAg7kXgTwiaE09OCxEic7CPERkYZ8g/640?wx_fmt=png&from=appmsg)

### Misc

```
这是一个CTF Misc题目，调用burp mcp 和kali mcp 及适当的方法，解题找出并输出flag：

黑客通过wireshark抓到管理员登陆网站的一段流量包（管理员的密码即是答案) 注意：得到的 flag 请包上 flag{} 提交。 流量包在根目录以及kali的/home/kali/ctf目录下
```

![image-20260111154521839](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVS5ibKw7KdTxHse6nPY42wSujl5uuFAnEcsOs8viankIKs2so8WKqUsow/640?wx_fmt=png&from=appmsg)![image-20260111154634584](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKua0DiaYTeBXlGvcm9xLdfpjVUPq3TlUxQ9IW6bNib2kc0wGHmUg8xJPtl51SlGSEYr2efCRzQgTsoJw/640?wx_fmt=png&from=appmsg)

**下面是一则内部学习圈广告😜**

**别着急退，看完的师傅们有福了/doge**

欢迎师傅们加入内部网络安全学习圈子。圈子提供三大板块的内容：

**1**

**网络安全0→1学习路径**

![图片](https://mmbiz.qpic.cn/mmbiz_png/dGCYMHZUKeFyRPgswYHs24iaP9QSZr6Of35ichXI6icv5WergQUcNjojdNRRp9CeibzvQHPPNNsxL6aaqVJ8gjQaqA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=1)

1. 完整的「30+周安全学习任务路线图」公开，每周任务明确，清晰的学习重点和目标，从入门到进阶，由浅入深，循序渐进；
2. 学习内容涵盖：

* 常见的Web漏洞原理与利用
* 业务逻辑漏洞挖掘
* SRC实战技巧
* WAF绕过、代码审计、免杀钓鱼
* 内网渗透

  （Linux&Windows）提权与权限维持
* 隧道代理、域渗透、云安全、AI安全

3. 每周发布学习任务+参考资料+建议，学员可自主学习+实战练习；

**2**

**SRC漏洞专项挖掘**

![图片](https://mmbiz.qpic.cn/mmbiz_png/dGCYMHZUKeFyRPgswYHs24iaP9QSZr6Of35ichXI6icv5WergQUcNjojdNRRp9CeibzvQHPPNNsxL6aaqVJ8gjQaqA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=2)

1. SRC漏洞知识库持续更新；
2. SRC挖掘技巧、分析方法、视频教程打包；
3. 分享优质挖矿案例，降低上手门槛，教你赚赏金。

**3**

**常态化内容更新**

![图片](https://mmbiz.qpic.cn/mmbiz_png/dGCYMHZUKeFyRPgswYHs24iaP9QSZr6Of35ichXI6icv5WergQUcNjojdNRRp9CeibzvQHPPNNsxL6aaqVJ8gjQaqA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=3)

日常分享优质学习资源与攻防渗透技巧，包括但不限于：

1. 红队/蓝队安全攻防、免杀、钓鱼技巧、攻防渗透tips；
2. 学习路线推荐，教程、方法、技巧tips打包分享，实战视频、工具、手册一应俱全；
3. 根据网络安全初中级学习者水平，精选最有用的内容，不让你在信息洪流中迷路。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuYnqBadHPfYribO0Eh7AO6sZtibP7icnEL1CIv2ibPnlUibbBzpK1lImaQsiawxpEKD4wOE3B9tBMll0HBg/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=81)

![图片](https://mmbiz.qpic.cn/mmbiz_png/HaJr68L1tTSb1XKYzBaSZ12svUicannzD6B7ialvhZB0XJtGrrSiawmjIhv4ZRW4gTvdhQ1MkSTNvv530EOqSfKBQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=5)

![图片](https://mmbiz.qpic.cn/mmbiz_png/HaJr68L1tTSb1XKYzBaSZ12svUicannzDJKQUZRFgUp7micv32kZwQClN4Nrwjs2M136dAhEJzmbia2bZ17c7jRicw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=6)

![图片](https://mmbiz.qpic.cn/mmbiz_png/HaJr68L1tTSb1XKYzBaSZ12svUicannzDnJkibQvVicDW74QWJvoiaWOnsrGbwibCJWEkToaicK45yPzIlvD24jickxWA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=7)

![图片](https://mmbiz.qpic.cn/mmbiz_png/HaJr68L1tTSb1XKYzBaSZ12svUicannzDQL6MIsF3Yqiczbczx67Z76BjgaXGGn8anlibtj82icib29ZyuuP3N7s9gw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=8)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuYx6e5OYqRUhe5nHp6uuOTaj5Sgo33WiaHyVjXhiaDxpy2Ub3j8RH9oHDdjmiauN7IHrEh4eQHbZYgYw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=50)

此前的一下学习记录：

![图片](https://mmbiz.qpic.cn/mmbiz_png/h8P1KUHOKuavfHUHVEFOGPwgcIyxvs5JeINcQEBnZT1hY0K4Pw7ya9o9gUkcFgaItIRibaMVQuXrhsthgdXELGQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=84)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuYHyEqA6pDb8VLMp8HsIicKjibbR1viclLspl5Yne6f4QnlkOiao0R4iasZ71DOILPUe0XSzqOKuDdPPfw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=85)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuaacJIuOWXhuibQcZiavltCSw4Uce8HJjKUHgQwKLmUyicQ16W3RibjnXgzw6ibRXYSxKeC4XebucKp1lA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=86)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuYHyEqA6pDb8VLMp8HsIicKjTg4aY5w0eR7nPUKJ9qNEk5Y0COUibDSvmPKiaMVBo8Nrqgex2Gs0h9xA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=87)

### 考证咨询

最优惠报考各类安全证书(NISP/CISP/CISSP/PTE/PTS/PMP/IRE等....)，后台回复"好友位"咨询。

### 关注我们

点个【 在看 】，你最好看

预览时标签不可点

修改于

![]()

微信扫一扫
关注该公众号

继续滑动看下一个

轻触阅读原文

![](http://mmbiz.qpic.cn/mmbiz_png/h8P1KUHOKuZq5sEo9xMfOVGAKuZWic3dSmVcRnYRDwbJdF39kiaGOrw5ofgicOs4WUH5PBiaq1MXpYDVbfSlCKJ00g/0?wx_fmt=png)

Z2O安全攻防

向上滑动看下一个

知道了

![]()
微信扫一扫
使用小程序

取消
允许

取消
允许

取消
允许

×
分析

![跳转二维码]()

![作者头像](http://mmbiz.qpic.cn/mmbiz_png/h8P1KUHOKuZq5sEo9xMfOVGAKuZWic3dSmVcRnYRDwbJdF39kiaGOrw5ofgicOs4WUH5PBiaq1MXpYDVbfSlCKJ00g/0?wx_fmt=png)

微信扫一扫可打开此内容，
使用完整服务

：
，
，
，
，
，
，
，
，
，
，
，
，
。

视频
小程序
赞
，轻点两下取消赞
在看
，轻点两下取消在看
分享
留言
收藏
听过