---
title: The Burrows-Wheeler Transform 块排序压缩算法
url: https://blog.mauve.icu/2026/01/01/notebook/Burrows-Wheeler-Transform/
source: Shiroha白羽的博客
date: 2026-01-01
fetch_date: 2026-01-02T03:33:36.348894
---

# The Burrows-Wheeler Transform 块排序压缩算法

[**Mauve**](/)

* [Home](/)
* [归档](/archives/)
* 分类

  [ACM-算法](/categories/ACM-%E7%AE%97%E6%B3%95/) [学习-开发-实现](/categories/%E5%AD%A6%E4%B9%A0-%E5%BC%80%E5%8F%91-%E5%AE%9E%E7%8E%B0) [学习笔记](/categories/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0) [杂项](/categories/%E6%9D%82%E9%A1%B9)
* [标签](/tags/)
* [好友](/links/)
* [RSS](/rss.xml)
* [开往](https://www.travellings.cn/go.html)
* [关于](/about/)
* [终页](/last/page)

Shiroha  2026年1月1日 晚上

812 字 7 分钟  浏览  次

# The Burrows-Wheeler Transform 块排序压缩算法

# 前言

最近发现了一个非常有意思的算法：The Burrows-Wheeler Transform（块排序压缩算法），所以稍微花点时间简单记录一下

说是一个压缩算法，实际上这个算法主要做的事情是把数据重排序，实际上并没有减少数据的长度（通常反而因为增加了行尾标识而增长了）

但是它完成了一个非常有意思的效果：在几乎不增加字符串长度的情况下，把一个字符串的重复的部分尽可能聚合到一块了

而 Gzip 压缩算法基于Deflate算法，通过结合LZ77算法（查找并替换重复字符串）和霍夫曼编码，通过将接近的字符串聚合到一块，能够有效增加压缩率

# 算法操作方式

我做了一个简单的演示工具，我们就用经典的 `banana` 作为示例

在原串后添加结束结束符号$，且此符号认为是最小的字符

banana\$

生成字符串的全部循环序列

banana\$
anana\$b
nana\$ba
ana\$ban
na\$bana
a\$banan
\$banana

将这几个字符串排序

\$banana
a\$banan
ana\$ban
anana\$b
banana\$
na\$bana
nana\$ba

取出最后一列字符串

\$banana
a\$banan
ana\$ban
anana\$b
banana\$
na\$bana
nana\$ba

得到结果

annb\$aa

下面将进行还原操作

annb\$aa

将结果排成一列

a
n
n
b
\$
a
a

排序

\$
a
a
a
b
n
n

在当前的列之前添加 BWT 结果

a\$
na
na
ba
\$b
an
an

再次排序

\$b
a\$
an
an
ba
na
na

重复上述步骤

a\$b
na\$
nan
ban
\$ba
ana
ana

再次排序

\$ba
a\$b
ana
ana
ban
na\$
nan

重复上述步骤

a\$ba
na\$b
nana
bana
\$ban
ana\$
anan

再次排序

\$ban
a\$ba
ana\$
anan
bana
na\$b
nana

重复上述步骤

a\$ban
na\$ba
nana\$
banan
\$bana
ana\$b
anana

再次排序

\$bana
a\$ban
ana\$b
anana
banan
na\$ba
nana\$

重复上述步骤

a\$bana
na\$ban
nana\$b
banana
\$banan
ana\$ba
anana\$

再次排序

\$banan
a\$bana
ana\$ba
anana\$
banana
na\$ban
nana\$b

重复上述步骤

a\$banan
na\$bana
nana\$ba
banana\$
\$banana
ana\$ban
anana\$b

再次排序

\$banana
a\$banan
ana\$ban
anana\$b
banana\$
na\$bana
nana\$ba

回到最初的矩阵

\$banana
a\$banan
ana\$ban
anana\$b
banana\$
na\$bana
nana\$ba

上一步下一步

# 算法原理

觉得这个算法有意思的地方，可能并不是它的实用价值。这里有一个非常有意思：为什么这样排序了几次之后，就会回到最初的矩阵

这里蕴藏了一个非常有意思的字符串排序逻辑。

通常情况下，我们会使用字符串从第一个字符开始比较，如果相同则比下一个字符

而在这个问题下，假定所有字符串长度相同，那其实完全可以从最后一位比起，然后逐次比较新增加的字符，可以实现类似桶排序的方式，达成最终排序结果

由于后排序的结果会覆盖先排序的结果，使得实际上达成了“从第一个字符开始比较，如果相同则比下一个字符”的效果

---

[杂项](/categories/%E6%9D%82%E9%A1%B9/)

[#算法](/tags/%E7%AE%97%E6%B3%95/)

The Burrows-Wheeler Transform 块排序压缩算法

https://blog.mauve.icu/2026/01/01/notebook/Burrows-Wheeler-Transform/

作者

Shiroha

发布于

2026年1月1日

许可协议

[Summer Pockets 动画完结 下一篇](/2025/10/04/bangumi/Summer-Pockets/ "Summer Pockets 动画完结")

Please enable JavaScript to view the comments

目录

#### 搜索

×

关键词

[Copyright ♡ Mauve](https://github.com/Hukeqing/)
[萌ICP备20240210号](https://icp.gov.moe/?keyword=20240210)

总访问量  次 总访客数  人

博客在允许 JavaScript 运行的环境下浏览效果更佳