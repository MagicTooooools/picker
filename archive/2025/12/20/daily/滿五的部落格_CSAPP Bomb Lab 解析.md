---
title: CSAPP Bomb Lab 解析
url: https://blog.aeilot.top/2025/12/20/csapp-lab2/
source: 滿五的部落格
date: 2025-12-20
fetch_date: 2025-12-21T03:27:51.985383
---

# CSAPP Bomb Lab 解析

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

1. [1. 題目要求](#%E9%A1%8C%E7%9B%AE%E8%A6%81%E6%B1%82)
2. [2. 環境](#%E7%92%B0%E5%A2%83)
3. [3. 前置知識](#%E5%89%8D%E7%BD%AE%E7%9F%A5%E8%AD%98)
   1. [3.1. 1. 啟動與退出 (Startup & Exit)](#1-%E5%95%9F%E5%8B%95%E8%88%87%E9%80%80%E5%87%BA-Startup-amp-Exit)
   2. [3.2. 2. 斷點管理 (Breakpoints)](#2-%E6%96%B7%E9%BB%9E%E7%AE%A1%E7%90%86-Breakpoints)
   3. [3.3. 3. 執行控制 (Execution Control)](#3-%E5%9F%B7%E8%A1%8C%E6%8E%A7%E5%88%B6-Execution-Control)
   4. [3.4. 4. 查看數據 (Inspection)](#4-%E6%9F%A5%E7%9C%8B%E6%95%B8%E6%93%9A-Inspection)
   5. [3.5. 5. 堆棧與上下文 (Stack & Context)](#5-%E5%A0%86%E6%A3%A7%E8%88%87%E4%B8%8A%E4%B8%8B%E6%96%87-Stack-amp-Context)
   6. [3.6. 6. 提升體驗：TUI 模式 (Text User Interface)](#6-%E6%8F%90%E5%8D%87%E9%AB%94%E9%A9%97%EF%BC%9ATUI-%E6%A8%A1%E5%BC%8F-Text-User-Interface)
4. [4. 反匯編](#%E5%8F%8D%E5%8C%AF%E7%B7%A8)
5. [5. strings](#strings)
6. [6. Phase 1](#Phase-1)
7. [7. Phase 2](#Phase-2)
8. [8. Phase 3](#Phase-3)
9. [9. Phase 4](#Phase-4)
   1. [9.1. 偏置](#%E5%81%8F%E7%BD%AE)
10. [10. Phase 5](#Phase-5)
11. [11. Phase 6](#Phase-6)
12. [12. 隱藏關](#%E9%9A%B1%E8%97%8F%E9%97%9C)
13. [13. 總結](#%E7%B8%BD%E7%B5%90)

![滿五](/images/gravatar.jpeg)

滿五

Stay Hungry. Stay Foolish.

[62
文章](/archives/)

[13
分類](/categories/)

[51
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

# CSAPP Bomb Lab 解析

發表於
2025-12-20

分類於

[CSAPP](/categories/CSAPP/)

閱讀次數：

Waline：

文章字數：
43k

做完了 CSAPP Bomb Lab，寫一篇解析。

## 題目要求

運行一個二進制文件 bomb，它包括六個”階段(phase)”，每個階段要求學生通過 stdin 輸入一個特定的字串。如果輸入了預期的字串，那麼該階段被”拆除”，進入下一個階段，直到所有炸彈被成功”拆除”。否則，炸彈就會”爆炸”，列印出”BOOM!!!”

## 環境

這個系統是在 `x86_64` Linux 上運行的，而筆者的環境是 `ARM` 架構的 macOS (Apple Silicon)。

弄了半天 `docker`，虛擬化一個 `x86_64` Ubuntu 出來，結果裡面的 `gdb` 不能用，不想折騰。

發現 `educoder` 上面有環境，可以直接用，而且免費，於是就在 `educoder` 上面完成了本實驗。

地址：<https://www.educoder.net/paths/6g398fky>

## 前置知識

本實驗要求掌握 `gdb` 的一些指令。

### 1. 啟動與退出 (Startup & Exit)

| 指令 | 縮寫 | 描述 |
| --- | --- | --- |
| `gdb executable` | - | 啟動 GDB 並載入可執行文件。 |
| `run [args]` | **`r`** | 開始運行程序。如果有命令行參數，跟在後面（如 `r input.txt`）。 |
| `quit` | **`q`** | 退出 GDB。 |
| `start` | - | 運行程序並在 `main` 函數的第一行自動暫停（省去手動打斷點的麻煩）。 |
| `set args ...` | - | 設置運行時的參數（在 `r` 之前使用）。 |

### 2. 斷點管理 (Breakpoints)

| 指令 | 縮寫 | 描述 | 範例 |
| --- | --- | --- | --- |
| `break <loc>` | **`b`** | 設置斷點。支持函數名、行號、檔案名:行號。 | `b main` `b 15` `b file.c:20` |
| `info breakpoints` | **`i b`** | 查看當前所有斷點及其編號 (Num)。 | - |
| `delete <Num>` | **`d`** | 刪除指定編號的斷點。不加編號則刪除所有。 | `d 1` |
| `disable/enable <Num>` | - | 暫時禁用或啟用某個斷點（保留配置但不生效）。 | `disable 2` |
| `break ... if <cond>` | - | **條件斷點**：僅當條件為真時才暫停（非常有用）。 | `b 10 if i==5` |

### 3. 執行控制 (Execution Control)

| 指令 | 縮寫 | 描述 | 區別點 |
| --- | --- | --- | --- |
| `next` | **`n`** | **單步跳過**。執行下一行程式碼。 | 如果遇到函數調用，**不進入**函數內部，直接執行完該函數。 |
| `step` | **`s`** | **單步進入**。執行下一行程式碼。 | 如果遇到函數調用，**進入**函數內部逐行除錯。 |
| `continue` | **`c`** | 繼續運行，直到遇到下一個斷點或程序結束。 | - |
| `finish` | - | 執行直到當前函數返回。 | 當你不小心 `s` 進了一個不想看的庫函數時，用這個跳出來。 |
| `until <line>` | **`u`** | 運行直到指定行號。 | 常用於快速跳出循環。 |

### 4. 查看數據 (Inspection)

| 指令 | 縮寫 | 描述 |
| --- | --- | --- |
| `print <var>` | **`p`** | 列印變數的值。支持表達式（如 `p index + 1`）。 |
| `display <var>` | - | **持續顯示**。每次程序暫停時，自動列印該變數的值（適合跟蹤循環中的變數）。 |
| `info locals` | - | 列印當前棧幀中所有局部變數的值。 |
| `whatis <var>` | - | 查看變數的數據類型。 |
| `ptype <struct>` | - | 查看結構體或類的具體定義（成員列表）。 |
| `x /nfu <addr>` | **`x`** | **查看記憶體**。`n`是數量，`f`是格式(x=hex, d=dec, s=str)，`u`是單位(b=byte, w=word)。 例如：`x/10xw &array` (以16進制顯示數組前10個word)。 |

### 5. 堆棧與上下文 (Stack & Context)

| 指令 | 縮寫 | 描述 |
| --- | --- | --- |
| `backtrace` | **`bt`** | **查看調用棧**。顯示程序崩潰時的函數調用路徑（從 main 到當前函數）。 |
| `frame <Num>` | **`f`** | 切換到指定的堆棧幀（配合 `bt` 看到的編號）。切換後可以用 `p` 查看該層函數的局部變數。 |
| `list` | **`l`** | 顯示當前行附近的原始碼。 |

### 6. 提升體驗：TUI 模式 (Text User Interface)

* **`layout src`**：螢幕分為兩半，上面顯示原始碼和當前執行行，下面是命令窗口。（強烈推薦）
* **`layout asm`**：顯示匯編代碼。
* **`layout split`**：同時顯示原始碼和匯編。

## 反匯編

我們可以使用 `objdump` 直接進行反匯編，查看匯編原始碼。

|  |  |
| --- | --- |
| ``` 1 ``` | ``` objdump -d bomb > bomb.asm ``` |

我們可以觀察到，幾個 `phase` 其實是幾個函數，`phase_x()`。

## strings

在終端輸入：

|  |  |
| --- | --- |
| ``` 1 ``` | ``` strings bomb ``` |

這會把 `bomb` 文件裡所有連續的可列印字元（ASCII）都列印出來。

## Phase 1

我們先看看 `phase_1` 長什麼樣子，`disas phase_1`

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 ``` | ``` Dump of assembler code for function phase_1:    0x0000000000400ee0 <+0>:     sub    $0x8,%rsp    0x0000000000400ee4 <+4>:     mov    $0x402400,%esi    0x0000000000400ee9 <+9>:     callq  0x401338 <strings_not_equal>    0x0000000000400eee <+14>:    test   %eax,%eax    0x0000000000400ef0 <+16>:    je     0x400ef7 <phase_1+23>    0x0000000000400ef2 <+18>:    callq  0x40143a <explode_bomb>    0x0000000000400ef7 <+23>:    add    $0x8,%rsp    0x0000000000400efb <+27>:    retq    End of assembler dump. ``` |

`sub $0x8,%rsp` 是設置棧幀，在這裡不用管。

`mov $0x402400,%esi` 和 `callq 0x401338 <strings_not_equal>` 似乎進行了字串的 `strcmp`。

接下來 `je 0x400ef7 <phase_1+23>` 就很明顯了，如果相等跳出炸彈。

設置斷點，`b phase_1`

之後運行程序，`r`，隨便輸入一些內容，就可以觸發斷點

以字串形式查看 `0x402400` 所指向的記憶體：`x/s 0x402400`

|  |  |
| --- | --- |
| ``` 1 ``` | ``` 0x402400:       "Border relations with Canada have never been better." ``` |

我們找到了答案。

## Phase 2

還是先反匯編：

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 ``` | ``` Dump of assembler code for function phase_2:    0x0000000000400efc <+0>:     push   %rbp    0x0000000000400efd <+1>:     push   %rbx    0x0000000000400efe <+2>:     sub    $0x28,%rsp    0x0000000000400f02 <+6>:     mov    %rsp,%rsi    0x0000000000400f05 <+9>:     callq  0x40145c <read_six_numbers>    0x0000000000400f0a <+14>:    cmpl   $0x1,(%rsp)    0x0000000000400f0e <+18>:    je     0x400f30 <phase_2+52>    0x0000000000400f10 <+20>:    callq  0x40143a <explode_bomb>    0x0000000000400f15 <+25>:    jmp    0x400f30 <phase_2+52>    0x0000000000400f17 <+27>:    mov    -0x4(%rbx),%eax    0x0000000000400f1a <+30>:    add    %eax,%eax    0x0000000000400f1c <+32>:    cmp    %eax,(%rbx)    0x0000000000400f1e <+34>:    je     0x400f25 <phase_2+41>    0x0000000000400f20 <+36>:    callq  0x40143a <explode_bomb>    0x0000000000400f25 <+41>:    add    $0x4,%rbx    0x0000000000400f29 <+45>:    cmp    %rbp,%rbx    0x0000000000400f2c <+48>:    jne    0x400f17 <phase_2+27>    0x0000000000400f2e <+50>:    jmp    0x400f3c <phase_2+64>    0x0000000000400f30 <+52>:    lea    0x4(%rsp),%rbx    0x0000000000400f35 <+57>:    lea    0x18(%rsp),%rbp    0x0000000000400f3a <+62>:    jmp    0x400f17 <phase_2+27>    0x0000000000400f3c <+64>:    add    $0x28,%rsp    0x0000000000400f40 <+68>:    pop    %rbx    0x0000000000400f41 <+69>:    pop    %rbp    0x0000000000400f42 <+70>:    retq    End of assembler dump. ``` |

`0x0000000000400f05 <+9>: callq 0x40145c <read_six_numbers>` 這裡看到 `read_six_numbers`

我們可以反匯編 `read_six_numbers`

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 ``` | ``` Dump of assembler code for function read_six_numbers:    0x000000000040145c <+0>:     sub    $0x18,%rsp    0x0000000000401460 <+4>:     mov    %rsi,%rd...