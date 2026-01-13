---
title: Mitmproxy and iOS simulator
url: https://blog.othree.net/log/2026/01/12/mitmproxy-ios/
source: O3noBLOG
date: 2026-01-12
fetch_date: 2026-01-13T03:34:07.163987
---

# Mitmproxy and iOS simulator

[首頁](/)
[彙整](/log/)
[部落滾](/blogroll/ "BLOGROLL")
[About](/about/me/)
[![GitHub](/images/github.svg)](https://github.com/othree "GitHub")

# [O3noBLOG](/ "O3noBLOG")

## 單篇彙整

[上一層](../)

---

01月
12日

### [Mitmproxy and iOS simulator](/log/2026/01/12/mitmproxy-ios/)

之前（兩三年前了）因為工作需求，想要確認 iOS 到底有沒有認真看待 HTTP cache 機制，因為在 app 內，收到的 HTTP status code 總是 200，然後我又看不到 request header，遠端的資料放 CDN 上也不好看到紀錄，轉念一想，我是不是可以改成監看 iOS 模擬器的所有請求呢？

搜尋之後發現有人說可以用 [mitmoroxy](https://www.mitmproxy.org/)，是開源的不像其他 proxy 除錯工具一樣需要付費，稍微測試了一下，發現這東西很好安裝和使用啊，所以花了一點時間研究看看是不是能達到目標，大概步驟如下：

* 安裝`brew install mitmproxy`
* 然後開 iOS 模擬器
* 安裝 mitmoroxy 的 cert 到模擬器裡面

```
xcrun simctl keychain booted add-root-cert ~/.mitmproxy/mitmproxy-ca-cert.pem
```

* 啟動 mitmproxy
* 然後設定你的 MacOS 網路來使用這個 proxy（預設是`http://localhost:8080`），因為 iOS 模擬器和 host OS 是共用網路的

就這麼簡單，不過這方法還是有些限制，像是如果有用 HTTPS pinning 的話，那就還要其它設定；然後 mitmproxy 本身的操作很好上手，這邊就不多花時間說明了。

然後回到一開始的問題，到底 304 有沒有用呢？我這邊是用 React Native 內的 fetch，然後結論就是**有**，在 mitmproxy 確實是看的到 304，不過在 RN 的 context 內就會變成 200 了，其實後來想想也覺得合理，因為應用程式層本來就不應該去處理 HTTP 層的 cache 問題，這樣大大的簡化了複雜度。

參考：

* [Using mitmproxy with the iOS simulator](https://alwold.com/posts/using-mitmproxy-with-ios-simulator/)

[軟體推薦、TIP](/log/software/)

---

---

## 其它資訊

---

### 關於本文章

**Mitmproxy and iOS simulator**發表於 2026-01-12，文章類別為
[軟體推薦、TIP](/log/software/)。

Share

[上一篇：Unicode CLDR 與貨幣數值格式2025-12-16](/log/2025/12/16/unicode-cldr/)

### 關於本網誌

本網誌是 [othree](https://twitter.com/othree) 的個人部落格，主要內容為網路標準、網頁設計，穿插些 ACG 心得和敗家紀錄，更詳細的資訊請見[關於這](http://blog.othree.net/about/here/)，如要聯絡我請寄信到 othree@gmail.com

### 近期文章

* [Unicode CLDR 與貨幣數值格式](/log/2025/12/16/unicode-cldr/ "2025-12-16")
* [2024 北海道 Part 3](/log/2025/12/06/2024-hokkaido-3/ "2025-12-06")
* [2024 北海道 Part 2](/log/2025/12/05/2024-hokkaido-2/ "2025-12-05")
* [2024 北海道 Part 1](/log/2025/12/04/2024-hokkaido-1/ "2025-12-04")
* [HayatoS](/log/2025/11/30/hayatos/ "2025-11-30")
* [鳥部製作所 廚房剪刀](/log/2025/01/05/toribe-kitchen-scissor/ "2025-01-05")
* [世界上最透明的故事](/log/2024/12/23/transparent-story/ "2024-12-23")
* [InnoDB 修復紀錄](/log/2024/12/22/innodb-recovery/ "2024-12-22")
* [onAutofill](/log/2024/10/19/onautofill/ "2024-10-19")

### 分類彙整

* [關於這裡](/log/about/) (43)
* [動畫、漫畫、遊戲](/log/acg/) (42)
* [與書相關](/log/books/) (39)
* [敗家](/log/buy/) (52)
* [CSS & HTML](/log/css-html/) (111)
* [日記](/log/diary/) (143)
* [蘋果蘋果](/log/mac/) (17)
* [其他](/log/others/) (23)
* [SCRIPT](/log/script/) (151)
* [Server Side](/log/server/) (16)
* [軟體推薦、TIP](/log/software/) (84)
* [UNIX](/log/unix/) (21)
* [VIM](/log/vim/) (46)
* [Web](/log/web/) (202)

### 訂閱本網誌

* [FeedBurner](https://feeds.feedburner.com/othree)

### 貼紙

[![時間がない](/images/busy_banner.png)](https://sites.google.com/view/happy-busy/)
[![JavaScript Reference, JavaScript Guide, JavaScript API, JS API, JS Guide, JS Reference, Learn JS, JS Documentation](/images/240x480-1.2v2.png)](https://developer.mozilla.org/en/JavaScript "JavaScript Reference, JavaScript Guide, JavaScript API, JS API, JS Guide, JS Reference, Learn JS, JS Documentation")

## 使用技術、規範、服務

[CC BY 4.0](http://creativecommons.org/licenses/by/4.0/deed.zh_TW)
[othree.net](https://othree.net)