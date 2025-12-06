---
title: [AGV] 初探 OpenTCS 開源車隊管理系統
url: https://woodloch.blog/2025/12/06/agv-%e5%88%9d%e6%8e%a2-opentcs-%e9%96%8b%e6%ba%90%e8%bb%8a%e9%9a%8a%e7%ae%a1%e7%90%86%e7%b3%bb%e7%b5%b1/
source: 木澤的研發腦
date: 2025-12-05
fetch_date: 2025-12-06T03:13:23.909975
---

# [AGV] 初探 OpenTCS 開源車隊管理系統

[直接觀看文章](#content)

[搜尋](#search-container)

搜尋：

[木澤的研發腦](https://woodloch.blog/)

* [linkedin](https://tw.linkedin.com/in/peter-yu-47379aab)
* [github](https://github.com/z29591259)
* [twitter](https://twitter.com/PeterYu77979783)
* [stackoverflow](https://stackoverflow.com/users/4225117/peter-yu)

選單

* 文章分類
  + [觀點想法](https://woodloch.blog/category/%E8%A7%80%E9%BB%9E%E6%83%B3%E6%B3%95/)
  + [學習分享](https://woodloch.blog/category/%E5%AD%B8%E7%BF%92%E5%88%86%E4%BA%AB/)
  + [談天說地](https://woodloch.blog/category/%E8%AB%87%E5%A4%A9%E8%AA%AA%E5%9C%B0/)
* 實用系列
  + [[WPF]常用小撇步](https://woodloch.blog/2020/02/24/wpf-dev-note/)
  + [常用工具](https://woodloch.blog/online-tools/)
* 系列文章
  + [你其實不用javascript](https://woodloch.blog/you-may-not-need-javascript/)
* 作品
  + [[Web]一鍵公車](https://woodloch.blog/2020/05/30/oneclickbus/)
  + [小遊戲 – 連鎖反應](https://woodloch.blog/2020/09/06/web-game-chain/)
  + [[Web]記憶遊戲](https://woodloch.blog/2020/02/07/web-memory-game-for-son/)
  + [[Web]迷宮產生器](https://woodloch.blog/2021/12/05/mazecreator/)
* [關於我](https://woodloch.blog/aboutme/)
* [所有文章](https://woodloch.blog/archives/)

開啟搜尋

# [[AGV] 初探 OpenTCS 開源車隊管理系統](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/)

![](https://woodloch.blog/wp-content/uploads/2025/12/cznmcy1wcml2yxrll3jhd3bpegvsx2ltywdlcy93zwjzaxrlx2nvbnrlbnqvchg4nzm0mdutaw1hz2uta3d2dxcxoweuanbn-1.webp?w=1024)

車隊管理系統 (Fleet Management System) 是調度多台AGV(AMR)時所需的必要系統，其功能主要包括整合上位系統、任務管理、交通管理、充電策略等等，目的是讓車輛的運行順暢和符合上位系統(MES or WCS)的期待，這篇文章將介紹筆者接觸到的開源解決方案 OpenTCS 是一個什麼樣的車隊系統，又適合哪些用戶來使用，協助工程師評估導入的可行性，不會包含詳細的功能說明和操作步驟。

目錄

1. [OpenTCS 簡介](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#opentcs-簡介)
2. [重點功能介紹](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#重點功能介紹)
   1. [地圖](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#地圖)
   2. [任務管理](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#任務管理)
   3. [路徑搜尋與成本](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#路徑搜尋與成本)
   4. [交通管理](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#交通管理)
   5. [待命區](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#待命區)
   6. [充電策略](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#充電策略)
   7. [模擬車](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#模擬車)
   8. [整合性](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#整合性)
3. [使用心得](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#使用心得-1)
   1. [各功能評估](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#各功能評估)
   2. [期望與落差](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#期望與落差)
   3. [適用客群](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#適用客群)
4. [參考](https://woodloch.blog/2025/12/06/agv-%E5%88%9D%E6%8E%A2-opentcs-%E9%96%8B%E6%BA%90%E8%BB%8A%E9%9A%8A%E7%AE%A1%E7%90%86%E7%B3%BB%E7%B5%B1/#參考)

## OpenTCS 簡介

[OpenTCS](https://github.com/openTCS/opentcs) 是由德國公司 [Fraunhofer IML](https://www.iml.fraunhofer.de/en.html) 維護的開源專案，採用寬鬆友善的 [MIT 授權](https://zh.wikipedia.org/zh-tw/MIT%E8%A8%B1%E5%8F%AF%E8%AD%89)，該車隊系統的Scope包含了一般對車隊系統的認知的功能，因爲供應商中立的特性，可以自行整合任意協定的車輛，這也意味着官方沒有爲了特定廠商開發協定的Adapter，需要自己動手開發對應的Adapter，唯一支援的協定，也是開源的AGV控制協定 [VDA5050](https://woodloch.blog/2024/11/08/agv-%E5%88%9D%E6%8E%A2vda5050-v2%E9%80%9A%E8%A8%8A%E5%8D%94%E5%AE%9A/)，github上有專門的 [Repo](https://github.com/openTCS/opentcs-commadapter-vda5050) 維護 VDA5050 Adapter，對於電梯或自動門等設備的整合，也可以開發週邊設備的Adapter自行整合，可以做到確保設備回到成功才讓車子拿到路徑資源允許通過。

其使用的程式語言爲 java，支援跨平臺使用，官方提供了 3 個桌面程式和1個控制台程式，分別是

* Model Editor : 地圖編輯器，用來編輯地圖layout以及車輛資訊
* Operations Desk : 觀看車輛、任務、路徑資源的即時狀態，可以進行派車、設定車輛可用性等
* Kernel Control Center : 車輛和週邊設備的控制管理，可以設定車輛的 Adapter 類型、停用啓用、週邊設備控制，可以看一些 Kernel Watchdog 的訊息
* Kernel : 車隊的主程式，沒有任何 UI，會將運行狀況以文字顯示在控制台界面上，前三者操作時，都是對 Kernel 進行連線後才能動作

整體而言，應用程式的 UI 非常工程師，算是堪用狀態，一些簡單的操作也需要做很多動作，屬於簡單直接沒有 UX 的 UI，更新方面，目前 OpenTCS 算是活躍的專案，約1-2個月就會發佈更新，持續修復問題和提供新功能，爲了方便描述，以下會將 OpenTCS 簡稱爲 TCS。

## 重點功能介紹

### 地圖

TCS的地圖屬於拓撲式，由點和線組成，比較特別的是兩點之間來回會算成2條線，而不是1條線，地圖上可以定義的元素如下

* Point：定義坐標、角度、類型
* Path：定義路線，包含開始與結束的Point、最高速度、是否鎖定
* Location Type：站點類型，供站點參照，定義可執行的”操作“，像是對接、充電等，也可以定義可執行的設備”操作“，像是開門”關門等
* Location：站點，關聯到特定的Point，作爲派車的目的地，也可以是設備的觸發點，會跟Path關聯，像是走到這條路就觸發開門，開門完成後才會把命令發給車子
* Block：資源的集合，所謂的資源指的是前面的Point、Path、Location，不同類型有不同作用，像是僅允許一臺車或是僅允許相同入口，不符合條件時，車輛會在外面等待，直到可以爲止
* Vehicle：定義車輛屬性，有名稱、最高速度、電量門檻、尺寸、允許接受的任務類型（一對多）、整合度，其中整合度可以決定車子是否占用資源、允許接收任務

爲了可以整合各種車輛和客製化邏輯，以上各元素都支援自定義屬性，這算是TCS的特色，TCS自己的屬性會用 tcs: 爲前綴，整合 VDA5050 時，adapter 也會讀取 vda5050: 開頭的屬性，保留最大的彈性

### 任務管理

TCS 的搬運任務稱爲 TransportOrder，是上位派車時的最小單位，可包含多個目的地，每個目的地就像一個子任務，稱爲DriveOrder，TransportOrder 可以指定

* 類型：對應 vehicle 的任務類型，可以做到不同類型的車接收各自類型的任務
* 期限時間：用來當成排序得參考
* 指定車輛：是否指定特定車輛執行
* 自動取消：主要用於回待命區或充電的 Order，可自動取消並接受新的 Order 節省時間
* 依賴：可設定A和B任務都完成後才執行此任務，TCS 會依照依賴順序執行 Order
* 客製化屬性：這邊定義的屬性都可以在 Adapter 內被讀取到，用來實現客製化的功能，以 VDA5050 Adapter 爲例，可以定義在目的地要執行的 action

任務的排序和每個任務挑選車子的條件可在config中進行態調各條件的優先順序，可以做到回頭車、Order期限快到時優先執行等較複雜的任務管理

### 路徑搜尋與成本

TCS內建 [Dijkstra](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) 算法找尋成本最低的路徑，成本則可以在 config 中多選，像是距離、Point數量、時間、群組、尺寸，其中群組是可以在 Path 和 Vehicle 定義屬性，讓相同群組的車使用對應的成本值，可以做到特定路讓特定車優先，尺寸則是在 Point 限定允許通過的大小，若 Vehicle 定義的大小超過則成本無限大，不允許通過，路徑一旦算出來後，就不會再改變了，基本上Path的鎖定沒有變更的話，算幾次都是一樣的結果

### 交通管理

有資源管理的概念，每個資源同時只能被一台車持有，正常狀況下，每台車會持有所在的 Point，有任務路徑時，會持有後續 N 段 Path 和 Point，如果路徑有交錯，先拿先贏，其他車只能等待釋出後再取得。

有些時候兩台車算出來的路徑會有衝突，例如都在同一條雙向道上，兩台車都各拿1個點，就會造成黑羊白羊過橋問題卡住，這時候可以設定 Block 來限定特定區域只能一台車來避免問題，但 Block 是把雙面刃，過度設置也會造成交通癱瘓

另外針對設備的部分，可以在走到特定路線時，對特定的設備 Adapter 發起 Open 的命令，等到設備確實回報成功，才會將行走的命令發給車子，並且繼續往後占有路權

### 待命區

Point 的類型可以選擇待命區，然後 config 檔設定閒置時自動回待命區，還可以設置待命區的優先順序，或是特定車輛停在特定待命區，更進階的還可以設定當優先權高的待命區空出時，重新把低優先權的車派過去停。

### 充電策略

每台車可以設定幾個門檻

* 低 critical energy level 於門檻時，不接任務且前往充電
* 低 good energy level 於門檻時，閒置時會去充電，但一有任務就會中斷去執行任務
* 高 full (sufficiently) recharged energy level 於門檻時，停止充電，回到待命區

也可以設定每輛車優先或特定的充電站，但僅此而已

### 模擬車

TCS 有提供模擬車輛，可以接收任務模擬行走，但是真實度有限，行走部分並不會參考速度，而是一個點一個點跳躍，可以用來測試地圖的可運行性，另外有支援耗電跟充電功能，可以測試充電策略

### 整合性

針對外部系統整合部分，有提供Restful Web API，跟 [Swagger](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/openTCS/opentcs/v7.0.1/opentcs-documentation/src/docs/service-web-api-v1/openapi.yaml) 可以了解格式，但是沒有 event trigger 的方法，只能不斷輪詢，頂多有一個 long polling 的API可以接收所有類型的 event，使用過後還是直接輪詢特定資訊的 API 比較方便

對於車子的部分，需要自行開發Adapter，這部分可以參考模擬車的專案進行修改，將 TCS 這邊定義的 command 發給車子，並把車子回傳的狀態，對應到TCS的車輛狀態，像是電量、是否有貨、閒置還是執行中等等，就可以控制車子

對於設備的部分，也是有開放Adapter的開發彈性，看你是要整合電梯還是自動門等，有明確的協定就可以完成

## 使用心得

### 各功能評估

以下對針對各功能評估對於一般使用情境是否足夠，尚缺哪些必要機制

✅：滿足所需
🆖：堪用但不足
❌：不滿足需求

|  |  |  |
| --- | --- | --- |
| 功能名稱 | 評價 | 評論 |
| 工具軟體 | 🆖 | Model Editor 操作有大量待優化空間，尤其是地圖設定自定義屬性的操作步驟過多，且沒有檢查機制，很容易漏東漏西  Operations Desk 可派簡易的循環任務，但不支援派含有自定義屬性的任務，只能自己打 API，另外可以看每台車的路權，不過想要看交互關係（A車卡B車、B車卡C車）就只能自己分析了  Kernel Control Center 操作部分較少，算是堪用，但要看問題，內建的 watchdog 資訊還是太少  以上工具還有一個缺點，就是一定要執行程式，沒有提供網頁界面快速瀏覽，也沒有提供權限功能，一定程度...