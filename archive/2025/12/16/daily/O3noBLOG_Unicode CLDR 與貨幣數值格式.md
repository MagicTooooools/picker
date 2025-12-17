---
title: Unicode CLDR 與貨幣數值格式
url: https://blog.othree.net/log/2025/12/16/unicode-cldr/
source: O3noBLOG
date: 2025-12-16
fetch_date: 2025-12-17T03:23:22.860736
---

# Unicode CLDR 與貨幣數值格式

[首頁](/)
[彙整](/log/)
[部落滾](/blogroll/ "BLOGROLL")
[About](/about/me/)
[![GitHub](/images/github.svg)](https://github.com/othree "GitHub")

# [O3noBLOG](/ "O3noBLOG")

## 單篇彙整

[上一層](../)

---

12月
16日

### [Unicode CLDR 與貨幣數值格式](/log/2025/12/16/unicode-cldr/)

[![Venezia](https://live.staticflickr.com/5677/30489284895_1b74c69f22_b.jpg)](https://www.flickr.com/photos/othree/30489284895/ "Venezia by othree, on Flickr")

其實一直以來都有點好奇，像 [`Intl`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl) 這種可以根據語系格式化輸出[數字、貨幣數值](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat)或是[日期](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat)等資料的工具函數，它的資料集到底是哪裡來的？會很好奇是因為這其實是一個多對多的組合，不同的語言地區配上不同的貨幣，算起來每種資料格式都要維護一組數量不小的組合，例如我想要用法文或英文顯示 10000 新台幣要用怎麼顯示呢：

```
// zh-Hant
$10,000.00
// fr
10 000,00 TWD
// en-US
NT$10,000.00
```

可以看到有很多點不一樣，首先數字格式的部分，可以看到小數點符號、千位數的分隔符號都不一樣；然後是貨幣符號，可能放前面可能放後面，中間可能會有空白（而且是 NBSP），然後根據不同組合可能會用短的貨幣符號，也可能用長的或是完整的貨幣代碼。作為比較，同樣的三組語系，如果改成顯示歐元則是：

```
// zh-Hant
€10,000.00
// fr
10 000,00 €
// en-US
€10,000.00
```

可以明顯看到，相同的語系顯示不同的貨幣數值，也有不一樣的格式設定，證實了它確實是一個多對多的關係，現在 ISO-639 有規範到的語言有 183 個，ISO-3166 的國家則是 271 個，相乘起來將近五萬種組合的資料是怎樣維護的呢？結果，其實這些資料的定義和管理都是在 Unicode 的 [CLDR 專案](https://cldr.unicode.org/)，CLDR 是 Common Locale Data Repository 的縮寫，收集整理了各種因為地區不同而有差異的資料格式，除了上面提到的貨幣、數字之外，還有像是日期、時間、度量衡、數字、姓名（姓前面還名前面、有沒有中間名等）等的格式，然後還有像是國家、貨幣、時區、單位甚至是 emoji 的翻譯。

CLDR 網站上可以直接看到這些資料，拿繁體中文為例，網頁位置在 [charts 下](https://www.unicode.org/cldr/charts/48/summary/zh_Hant.html)，下面的圖是第 44 版的，最新的版本其實是 48，以下請見截圖介紹，最右邊都有值的那欄就是 zh-Hant 繁體中文的翻譯和格式(pattern)。

國家名稱（這邊用的單字其實是 territory，翻譯通常是領土、疆域）、語言和時區：

[![CLDR 44 zh-Hant Country](https://live.staticflickr.com/65535/54972514552_ca3556bc2d_b.jpg)](https://www.flickr.com/photos/othree/54972514552/ "CLDR 44 zh-Hant Country by othree, on Flickr")
[![CLDR 44 zh-Hant Language](https://live.staticflickr.com/65535/54973659759_80e3a3d7bc_b.jpg)](https://www.flickr.com/photos/othree/54973659759/ "CLDR 44 zh-Hant Language by othree, on Flickr")
[![CLDR 44 zh-Hant Timezone](https://live.staticflickr.com/65535/54973659809_e1ec312a4b_b.jpg)](https://www.flickr.com/photos/othree/54973659809/ "CLDR 44 zh-Hant Timezone by othree, on Flickr")

日期時間格式：

[![CLDR 44 zh-Hant Date Time](https://live.staticflickr.com/65535/54973405756_35c566eb5d_b.jpg)](https://www.flickr.com/photos/othree/54973405756/ "CLDR 44 zh-Hant Date Time by othree, on Flickr")

也有特定國家用的系統，像是中國的天干地支和二十四節氣：

[![CLDR 44 zh-Hant Chinese Time and Season](https://live.staticflickr.com/65535/54973659734_7444790b83_b.jpg)](https://www.flickr.com/photos/othree/54973659734/ "CLDR 44 zh-Hant Chinese Time and Season by othree, on Flickr")

日本的年號：

[![CLDR 44 zh-Hant Japan Era](https://live.staticflickr.com/65535/54973704455_ecba3770bb_b.jpg)](https://www.flickr.com/photos/othree/54973704455/ "CLDR 44 zh-Hant Japan Era by othree, on Flickr")

這篇所提到的數字格式和貨幣顯示方式：

[![CLDR 44 zh-Hant Number](https://live.staticflickr.com/65535/54973659794_bab68c0595_b.jpg)](https://www.flickr.com/photos/othree/54973659794/ "CLDR 44 zh-Hant Number by othree, on Flickr")

貨幣名稱：

[![CLDR 44 zh-Hant Currency](https://live.staticflickr.com/65535/54973405786_c7fb481a55_b.jpg)](https://www.flickr.com/photos/othree/54973405786/ "CLDR 44 zh-Hant Currency by othree, on Flickr")

可以看到以上這些資訊都有繁體中文的文字翻譯，所以其實這都可以當成正式的譯名了。

這些資料的原始格式都是用 XML 維護的，東西放在 GitHub 上，例如我們上面舉例的 zh-Hant 的檔案[就在這](https://github.com/unicode-org/cldr/blob/main/common/main/zh_Hant.xml)，有興趣的可以看看檔案結構長怎樣，去[上一層目錄](https://github.com/unicode-org/cldr/tree/main/common/main)則可以看到所有目前有存在的地區語言組合(語系、locale)，目前有在維護的是 1127 種，超過一千個檔案所以其實在 GitHub 的網站上也不會全部列出來，而這一千多種語系其實遠大於我一開始提到的 ISO-639 規範到的一百多個語言，所以實際有在維護的資料數量比我一開始隨手估計的還要大上六七倍。

回到檔案內容，有兩個字元想要特別介紹的，這種用字元表示特別意義的方法在其他地方蠻少見，真不愧是 Unicode 專案：第一個是`↑↑↑`，可以在很多地方看到，這三個字元出現的話，就是代表請找 fallback 值，一般而言就是把語系的值拆分扣掉最後面那一段，如果在查找的時候收到的語系是`zh-Hant-TW`，那第一次 fallback 就會發生在找不到該檔案，改成找`zh-Hant`的檔案，也就是上面範例那個檔案，找到檔案後，查找該檔案內的指定的定義，結果定義是`↑↑↑`時，就改成去找`zh`的檔案內的定義，如果一直都找不到就會去 [root.xml](http://github.com/unicode-org/cldr/blob/main/common/main/root.xml) 找。

第二個要介紹的則是 [`¤`](https://en.wikipedia.org/wiki/Currency_sign_%28generic%29) 字元，這是我第一次注意到這個字元，本來以為是比較新的定義，不過其實它是很古老的字元，根據 Wikipedia，它在電腦領域最早出現是在 1972，7 bit 編碼時就有了，比我還老，一開始就是作為貨幣符號的占位符號使用，不代表任一特定的貨幣，那這個符號在 CLDR 這邊是怎樣用的呢？在 zh-Hant.xml 檔案內直接搜尋該字元，就可以看到這段定義：

```
<currencyFormat type="accounting">
	<pattern>¤#,##0.00</pattern>
	<pattern alt="alphaNextToNumber">↑↑↑</pattern>
	<pattern alt="noCurrency">↑↑↑</pattern>
</currencyFormat>
```

其中的`<pattern>`內的值就是 CLDR 自己定義的[數字、貨幣的顯示格式的 pattern（格式）](https://cldr.unicode.org/translation/number-currency-formats/number-and-currency-patterns)，詳細可以參考官網的介紹，每個字元都有其定義，而不是真的用那個字元顯示，例如 0 代表一定要出現的位數，但是數字系統可以換，所以最後輸出不一定是阿拉伯數字；`,`代表的是千位數分隔符號的位置，實際上要用哪個字元作為分隔符號，則是看該語系的定義；同理，`¤`字元的用途就是在這個 pattern 當中，貨幣符號的放置位置，而要顯示怎樣的貨幣符號，則是還要看語系和設定，例如新台幣在 zh-Hant 中的定義就有以下幾種：

```
<currency type="TWD">
	<displayName>新台幣</displayName>
	<displayName count="other">↑↑↑</displayName>
	<symbol>$</symbol>
	<symbol alt="formal">NT$</symbol>
	<symbol alt="narrow">↑↑↑</symbol>
</currency>
```

然後在其他語系可能還有直接使用 currency code 的，所以至少就有四種，而根據 [LDML](https://www.unicode.org/reports/tr35/)（這份 zh-Hant.xml 文件的格式），其實可以根據這個`¤`字元重複的次數，來決定要如何顯示該貨幣符號，詳見下表：

[![LDML currency symbol](https://live.staticflickr.com/65535/54985738533_8d6dbd1f13_b.jpg)](https://www.flickr.com/photos/othree/54985738533/ "LDML currency symbol by othree, on Flickr")

不過其實在 zh-Hant.xml 當中，搜尋到的所有的貨幣數值的 pattern，都只有一個`¤`，所以理論上應該都是一樣的用 standard currency symbol，但是實務上似乎比較少真的直接用到這特性，而是透過設定更改的比較多，在 JavaScript 的`Intl.NumberFormat`的 style option 中有一個 [`currencyDisplay`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat#minimumfractiondigits) 的設定可以用來改變貨幣符號的顯示方式，有`code`、`symbol`、`narrowSymbol`、`name`這四種，並沒有用到不同數量的`¤`的 pattern。深究其原理，其實`Intl.NumberFormat`底層就是 Unicode 官方的 [ICU](https://icu.unicode.org/)，而 ICU 則是透過 [Unit Width](https://unicode-org.github.io/icu/userguide/format_parse/numbers/skeletons.html#unit-width) 這個選項來調整，實作似乎是在替換時根據不同的 unit width 選用不同的值，而不是修改 pattern。而`Intl.NumberFormat`也就是透過設定 Unit Width 來達成的，結果就是，如果沒有直接使用 ICU 手動設定 pattern，似乎是不會真的用到多個`¤`的模式，不過這其實也太細節了，我也不是非常熟悉，所以只能說個大概。

最後想介紹的，就是 CLDR 其實還提供了不少的[補充資料(supplemental)](https://www.unicode.org/cldr/charts/48/supplemental/)，舉幾個比較有趣的：

* [領土 to 貨幣 的對照表](https://www.unicode.org/cldr/charts/48/supplemental/detailed_territory_currency_information.html#territory_currency)；
* 同上一頁的第二章表則是[各個貨幣的小數點後位數](https://www.unicode.org/cldr/charts/48/supplemental/detailed_territory_currency_information.html#format_info) （這我記得 ISO 也有一份）；
* [各個領土的資訊](https://www.unicode.org/cldr/charts/48/supplemental/territory_information.html)，例如使用的日曆系統、每週是星期幾開始、人口、GDP 等；
* [各個語言的範例字母](https://www.unicode.org/cldr/charts/48/supplemental/scripts_languages_and_territories.html)，像中文就是「字」、日文是「か」、韓文是「가」。

[軟體推薦、TIP](/log/software/)

---

---

## 其它資訊

---

### 關於本文章

**Unicode CLDR 與貨幣數值格式**發表於 2025-12-16，文章類別為
[軟體推薦、TIP](/log/software/)。

Share

[上一篇：2024 北海道 Part 32025-12-06](/log/2025/12/06/2024-hokkaido-3/)

### 關於本網誌

本網誌是 [othree](https://twitter.com/othree) 的個人部落格，主要內容為網路標準、網頁設計，穿插些 ACG 心得和敗家紀錄，更詳細的資訊請見[關於這](http://blog.othree.net/about/here/)，如要聯絡我請寄信到 othree@gmail.com

### 近期文章

* [2024 北海道 Part 3](/log/2025/12/06/2024-hokkaido-3/ "2025-12-06")
* [2024 北海道 Part 2](/log/2025/12/05/2024-hokkaido-2/ "2025-12-05")
* [2024 北海道 Part 1](/log/2025/12/04/2024-hokkaido-1/ "2025-12-04")
* [HayatoS](/log/2025/11/30/hayatos/ "2025-11-30")
* [鳥部製作所 廚房剪刀](/log/2025/01/05/toribe-kitchen-scissor/ "2025-01-05")
* [世界上最透明的故事](/log/2024/12/23/transparent-story/ "2024-12-23")
* [InnoDB 修復紀錄](/log/2024/12/22/innodb-recovery/ "2024-12-22")
* [onAutofill](/log/2024/10/19/onautofill/ "2024-10-19")
* [Oklab Color Space](/log/2024/07/21/oklab-color-space/ "2024-07-21")

### 分類彙整

* [關於這裡](/log/about/) (43)
* [動畫、漫畫、遊戲](/log/acg/) (42)
* [與書相關](/log/books/) (39)
* [敗家](...