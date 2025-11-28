---
title: 矩陣的 Modified Gram Schmidt 方法
url: https://blog.aeilot.top/2025/11/27/matrix-mqr/
source: 滿五的部落格
date: 2025-11-27
fetch_date: 2025-11-28T03:15:24.016429
---

# 矩陣的 Modified Gram Schmidt 方法

[滿五的部落格](/)

* [首頁](/index.html)
* [關於](/about/)
* [標籤](/tags/)
* [分類](/categories/)
* [歸檔](/archives/)
* [English](https://en.blog.aeilot.top/)
* 搜尋

* 文章目錄
* 本站概要

1. [1. 經典的 Gram-Schmidt](#%E7%B6%93%E5%85%B8%E7%9A%84-gram-schmidt)
   1. [1.1. 數學證明](#%E6%95%B8%E5%AD%B8%E8%AD%89%E6%98%8E)
   2. [1.2. 誤差分析](#%E8%AA%A4%E5%B7%AE%E5%88%86%E6%9E%90)
2. [2. 改進的 Gram-Schmidt (MGS)](#%E6%94%B9%E9%80%B2%E7%9A%84-gram-schmidt-mgs)
   1. [2.1. 誤差分析](#%E8%AA%A4%E5%B7%AE%E5%88%86%E6%9E%90-1)
3. [3. 總結](#%E7%B8%BD%E7%B5%90)

![滿五](/images/gravatar.jpeg)

滿五

Stay Hungry. Stay Foolish.

[59
文章](/archives/)

[12
分類](/categories/)

[47
標籤](/tags/)

[GitHub](https://github.com/aeilot "GitHub → https://github.com/aeilot")

E-Mail

[StackOverflow](https://stackoverflow.com/13011108 "StackOverflow → https://stackoverflow.com/13011108")

[Instagram](https://instagram.com/louisaeilot "Instagram → https://instagram.com/louisaeilot")

[Bilibili](https://space.bilibili.com/378981479 "Bilibili → https://space.bilibili.com/378981479")

[![Creative Commons](https://cdnjs.cloudflare.com/ajax/libs/creativecommons-vocabulary/2020.11.3/assets/license_badges/small/by_nc_sa.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/zh-TW)

連結

* [Mr.Will](https://mrwillcom.now.sh/ "https://mrwillcom.now.sh")
* [ChungZH](https://chungzh.cn/ "https://chungzh.cn")
* [SJX](https://shaojiaxu.github.io/ "https://shaojiaxu.github.io")
* [CLCK](http://clckblog.space/ "http://clckblog.space/")
* [静かな森](https://innei.in/ "https://innei.in/")

# 矩陣的 Modified Gram Schmidt 方法

發表於
2025-11-27

分類於

[程式設計](/categories/%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88/)

閱讀次數：

Waline：

文章字數：
1.6k

矩陣的 QR 分解在電腦運算中可能造成誤差，本文探討一下一種改進版本的
Gram-Schmidt 正交化方法。

經典的 Gram-Schmidt
方法可能造成數值不穩定性。在電腦中，舍入誤差可能會累積，造成得到的正交基並不具有正交性。

## 經典的 Gram-Schmidt

我們先來回顧一下經典的 Gram-Schmidt 方法：

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 ``` | ``` for j = 1 : n     v_j = x_j     for k = 1 : j - 1         v_j = v_j - ( (v_k^T x_j) / (v_k^T v_k) ) * v_k     endfor endfor ``` |

最終我們將 *v**j*
歸一化得到標準正交基 *q**j* = (*v**j*)/(|*v**j*|)
。

注意：CGS 始終使用原始向量 *x**j*
與之前的基向量計算投影

### 數學證明

簡單進行數學證明

定義第 *j* 步生成的向量 *v**j*

$$v\_j = x\_j - \sum\_{k=1}^{j-1} ( (v\_k^T
x\_j) / (v\_k^T v\_k) ) v\_k$$

選取任意一個之前的基向量 *v**m*（其中 1 ≤ *m* < *j*），計算它與
*v**j* 的內積
*v**m**T**v**j*：
$$v\_m^T v\_j = v\_m^T ( x\_j - \sum\_{k=1}^{j-1}
((v\_k^T x\_j) / (v\_k^T v\_k)) v\_k )$$

利用內積的線性性質，將 *v**m**T*
乘進去：

$$v\_m^T v\_j = v\_m^T x\_j - \sum\_{k=1}^{j-1}
((v\_k^T x\_j) / (v\_k^T v\_k)) (v\_m^T v\_k)$$

由於前提假設 *v*1, …, *v**j* − 1
是兩兩正交的，所以在求和符號 ∑ 中：

* 當 *k* ≠ *m*
  時，*v**m**T**v**k* = 0
  。
* 當且僅當 *k* = *m*
  時，*v**m**T**v**k* = *v**m**T**v**m* ≠ 0
  。

因此，求和項中只剩下 *k* = *m* 這一項：

*v**m**T**v**j* = *v**m**T**x**j* − ((*v**m**T**x**j*)/(*v**m**T**v**m*))(*v**m**T**v**m*)

分子分母中的標量 *v**m**T**v**m*
相互抵消：

*v**m**T**v**j* = *v**m**T**x**j* − *v**m**T**x**j*

*v**m**T**v**j* = 0

證畢

### 誤差分析

如果在 CGS（經典格拉姆-施密特正交化）中計算 *q*2 時發生誤差，導致 *q*1*T**q*2 = *δ*
是一個很小但非零的數值，這個誤差將不會在隨後的任何計算中被修正：

*v*3 = *x*3 − (*q*1*T**x*3)*q*1 − (*q*2*T**x*3)*q*2

*q*2*T**v*3 = *q*2*T**x*3 − (*q*1*T**x*3)*δ* − (*q*2*T**x*3) = −(*q*1*T**x*3)*δ*

*q*1*T**v*3 = *q*1*T**x*3 − (*q*1*T**x*3) − (*q*2*T**x*3)*δ* = −(*q*2*T**x*3)*δ*

我們可以看出 *v*3
與 *q*1 或 *q*2 都不正交。

## 改進的 Gram-Schmidt (MGS)

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 ``` | ``` for j = 1 : n     v_j = x_j endfor  for j = 1 : n     q_j = v_j / ||v_j||_2     for k = j + 1 : n         v_k = v_k - (q_j^T v_k) q_j     endfor endfor ``` |

最終我們將 *v**j*
歸一化得到標準正交基 *q**j* = (*v**j*)/(|*v**j*|)
。

區別在於，一旦算出來一個向量 *q*，立即用它去更新後面所有的向量（減去向量在
*q*
上的投影），使得後面所有的向量與這個向量 *q* 正交。

### 誤差分析

假設 MGS 出現：*q*1*T**q*2 = *δ*
很小但非零。

對於第三個向量 *v*3 ：

* 初始狀態：*v*3(0) = *x*3
* *j* = 1 : *v*3(1) = *v*3(0) − (*q*1*T**v*3(0))*q*1
* *j* = 2 : *v*3 = *v*3(1) − (*q*2*T**v*3(1))*q*2

此時 *v*3
是第三個向量在歸一化之前的最終形式。

讓我們檢查正交性，假設不再產生其他誤差：

*q*2*T**v*3 = *q*2*T**v*3(1) − (*q*2*T**v*3(1)) = 0

所以，我們保留了對 *q*2 的正交性。

接下來看 *q*1：

*q*1*T**v*3 = *q*1*T**v*3(1) − (*q*2*T**v*3(1))*δ*

*q*2*T**v*3(1) = *q*2*T**v*3(0) − (*q*1*T**v*3(0))*δ*

*q*1*T**v*3(1) = *q*1*T**v*3(0) − (*q*1*T**v*3(0)) = 0

由此可得： *q*1*T**v*3 = −(*q*2*T**v*3(0) − (*q*1*T**v*3(0))*δ*)*δ* = −*q*2*T**v*3(0)*δ* + *q*1*T**v*3(0)*δ*2

由於 *δ* 非常小，*δ*2 就更小了。因此 *q*1*T**v*3
中的誤差和 CGS 差不多，但我們消除了 *q*2*T**v*3
中的誤差，使得誤差更小。

## 總結

在電腦中，由於浮點運算的特殊性，有許多數學方法需要重新考慮，儘量提高數值穩定性。這使得我們有必要重新審視我們學習過的數學知識，針對電腦的特殊性進行重新設計與優化。

* **本文作者：** 滿五
* **文章連結：**
  [https://blog.aeilot.top/2025/11/27/matrix-mqr/](https://blog.aeilot.top/2025/11/27/matrix-mqr/ "矩陣的 Modified Gram Schmidt 方法")
* **版權聲明：** 本網誌所有文章除特別聲明外，均採用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/zh-TW) 許可協議。轉載請註明出處！

歡迎關注我的其它發布渠道

[Twitter](https://twitter.com/aeilot)

[Telegram](https://t.me/aeilot_post)

[RSS](/index.xml)

[知乎](https://www.zhihu.com/people/louis-80-27)

[數學](/tags/%E6%95%B8%E5%AD%B8/)
 [線性代數](/tags/%E7%B7%9A%E6%80%A7%E4%BB%A3%E6%95%B8/)

[聊一聊位掩碼（Bit Mask）](/2025/10/20/bitmask/ "聊一聊位掩碼（Bit Mask）")

© 2015 –
2025

滿五

0%

Theme NexT works best with JavaScript enabled