---
title: Best Pwnable Challenges 2025
url: https://ptr-yudai.hatenablog.com/entry/2025/12/31/135605
source: CTFするぞ
date: 2025-12-31
fetch_date: 2026-01-01T03:36:56.520272
---

# Best Pwnable Challenges 2025

[![CTFするぞ](https://cdn.image.st-hatena.com/image/square/994ee0e012cf90178bff014bfd99123c02a89b36/backend=imagemagick;height=128;version=1;width=128/https%3A%2F%2Fcdn.user.blog.st-hatena.com%2Fblog_custom_icon%2F153103571%2F1535456424487441)](https://ptr-yudai.hatenablog.com/)

[CTFするぞ](https://ptr-yudai.hatenablog.com/)

[読者になる](https://blog.hatena.ne.jp/ptr-yudai/ptr-yudai.hatenablog.com/subscribe?utm_medium=button&utm_campaign=subscribe_blog&utm_source=blogs_topright_button)

# [CTFするぞ](https://ptr-yudai.hatenablog.com/)

## CTF以外のことも書くよ

[2025-12-31](https://ptr-yudai.hatenablog.com/archive/2025/12/31)

# [Best Pwnable Challenges 2025](https://ptr-yudai.hatenablog.com/entry/2025/12/31/135605)

[CTF](https://ptr-yudai.hatenablog.com/archive/category/CTF)

# はじめに

今年参加したCTFの中から主観で面白かった問題を取り上げます。毎週参加してるわけではないので他にも面白い問題があったと思いますが、CTFtimeでtop 10に入るほどには参加していたらしいので今年は記事にしてみました。もっと面白い問題を知っているぞという方はぜひ教えてください。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/p/ptr-yudai/20251231/20251231135045.jpg)

* [はじめに](#はじめに)
* [受賞作品一覧](#受賞作品一覧)
  + [Stack Impromptu - 創造力賞](#Stack-Impromptu---創造力賞)
    - [作問者](#作問者)
    - [解説と概要](#解説と概要)
    - [コメント](#コメント)
  + [decore - 脆弱性賞](#decore---脆弱性賞)
    - [作問者](#作問者-1)
    - [解説と概要](#解説と概要-1)
    - [コメント](#コメント-1)
  + [new\_era - 教育賞](#new_era---教育賞)
    - [作問者](#作問者-2)
    - [解説と概要](#解説と概要-2)
    - [コメント](#コメント-2)
  + [old school - 風水賞](#old-school---風水賞)
    - [作問者](#作問者-3)
    - [解説と概要](#解説と概要-3)
    - [コメント](#コメント-3)
  + [その他の良問](#その他の良問)

# 受賞作品一覧

## Stack Impromptu - 創造力賞

創造力賞（Creativity Award）：解法がもっとも独創的だった・美しかった問題に与えられる賞

### 作問者

Dronexさん

### 解説と概要

はじめに紹介するのはBlackHat MEAの決勝で出題した[Stack Impromptu](https://bitbucket.org/ptr-yudai/writeups-2025/src/master/BlackHatMEA_Finals/Stack_Impromptu.zip)という問題です。「出題した」というように私が作問者になっているので紹介するか迷いましたが、90%くらいはDronexさんが作った問題なので対象にしました。

スレッド型サーバで次のような自明な[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)があるという問題です。

```
void fatal(const char *msg) {
  perror(msg);
  pthread_exit(NULL);
}

int server_read(int& fd) {
  size_t size;
  char buf[0x40];

  memset(buf, 0, sizeof(buf));
  if (read(fd, &size, sizeof(size)) != sizeof(size)
      || size > 0x100
      || read(fd, buf, size) < size)
    goto err;

  write(fd, buf, size);
  return 0;

err:
  close(fd);
  fatal("Could not receive data (read)");
  return 1;
}

void* server_main(void* arg) {
  int fd = (int)((intptr_t)arg);
  while (server_read(fd) == 0);
  return NULL;
}
```

セキュリティ機構がすべてかかっているため、stack canaryやlibcのアドレスをリークする必要があります。リークするためには `write` を適切なサイズで呼ぶ必要がありますが、そのためには `read` が失敗する（-1を返す）必要があります。

この問題では[バッファオーバーフロー](https://d.hatena.ne.jp/keyword/%EF%BF%BD%D0%A5%C3%A5%D5%A5%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%D0%A1%EF%BF%BD%EF%BF%BD%D5%A5%EF%BF%BD%EF%BF%BD%EF%BF%BD)によってfdが書き換えられるため、 `pthread_exit` が死なないように工夫すると良い感じに任意のfdをcloseできるprimitiveが手に入ります。ここでさらに、 `read` で待機中のソケットにRSTパケットを送ると、こちらから接続を切ることなく相手の `read` を失敗させることができます。これらを組み合わせて、サーバ側のfdを差し替えることで別のソケットが未初期化バッファのリークを受け取ることができるという、ソケットの知識をフル活用したパズルになっています。

[解法スクリプト](https://gist.githubusercontent.com/ptr-yudai/ebf09b77256853fdfc3b2da5335b5ff2/raw/e98a2b5301472cdc9432d1fd330301e0034d7ce3/stack_impromptu.py)

### コメント

間違えて解けない状態の問題をDronexに渡したら、1日かけて解いてきたので驚きました。
fdが差し替わることで、新しく開いたソケットに突然プログラム上ありえない謎のデータが降ってくるリーク方法は過去に見たことがなく美しかったです。

## decore - [脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)賞

[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)賞（[Vulnerability](https://d.hatena.ne.jp/keyword/Vulnerability) Award）：[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)がもっとも巧妙かつ自然に隠されていた問題に与えられる賞

### 作問者

不明

### 解説と概要

decoreはKalmarCTF 2025で出題された問題です。脆弱なプログラムが `core_pattern` に登録されているので、クラッシュすると「解析されると[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)を発火するコアダンプ」を生成するようなプログラムを作る必要があります。[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)はシンボル情報のパース時にELFのsymtab/strtabが不正だと範囲外参照を起こしてしまうというバグです。範囲外参照でフラグを表示するためにはフラグがメモリにマップされている必要があるので、そこをなんとかするという問題です。
この問題に関してはwriteupを公開しているので詳しくはそちらをご覧ください。

[ptr-yudai.hatenablog.com](https://ptr-yudai.hatenablog.com/entry/2025/03/10/123050#Pwn-427pt-decore)

### コメント

`core_pattern` に脆弱なプログラムが登録されていて権限昇格に使うという問題設定がそもそも斬新でした。プログラム側の[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)と、[Linux](https://d.hatena.ne.jp/keyword/Linux)側の回避しようのない問題を組み合わせて初めて解けるのも面白かったです。
[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)も実際にありそうな感じで良かったですが、欲を言えば[ソースコード](https://d.hatena.ne.jp/keyword/%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD)も配布してほしかったです。

## new\_era - 教育賞

教育賞（Educational Award）：もっとも教育的な問題に与えられる賞

### 作問者

r1ruさん

### 解説と概要

OSINTで有名なTsukuCTFですが、今年は[r1ruさん](https://r1ru.github.io/)がpwnを出題されていました。この問題は、[Linux](https://d.hatena.ne.jp/keyword/Linux) kernelのヒープでoff-by-nullが起きるというシンプルな[脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)です。
こちらは作問者writeupが公開されているので、詳しくはそちらをご覧ください。

[r1ru.github.io](https://r1ru.github.io/posts/7/)

### コメント

ここ数年、[Linux](https://d.hatena.ne.jp/keyword/Linux) kernel exploitは半分以上がdata-oriented attackになっており、 `pipe_buffer` やページテーブルを使う問題を多く見るようになったため、その点でも教育的だと思いました。
同CTFのxcacheという問題も、[Linux](https://d.hatena.ne.jp/keyword/Linux) kernelの専用キャッシュでUAFが起こるという近年よく見るパターンを簡略化した問題設定のため、cross-cache attackの教材としておすすめです。

## old school - 風水賞

風水賞（Feng Shui Award）：もっとも面倒な[\*1](#f-1ba4fcd1 "ヒープ問においては褒め言葉？")[glibc](https://d.hatena.ne.jp/keyword/glibc)ヒープ問題に与えられる賞

### 作問者

c0mm4nd\_さん

### 解説と概要

最後に紹介するのはsnakeCTF 2025 Qualsのold schoolです。
この問題は、tcacheが無効化された環境で、freeとreallocでdouble freeが発生するという状況をなんとかするヒープ問です。fastbinのdouble freeといえば、2つのチャンクを交互にfreeして検知を回避するのが一般的ですが、この問題は2連続でfreeされることが確定しているため難しい問題です。topから取得できないがfastbinがあるときに `malloc_consoliadte` が走るという挙動を利用すると解けます。
公式writeupが公開されているので、詳しくはそちらをご覧ください。

[snakectf.org](https://snakectf.org/writeups/2025-quals/pwn/old-school)

### コメント

fastbinをunsortedbinに追い出したり、fastbinでFILE構造体を書き換えにいったり、いろいろ考えることがあって大変でした。ヒープで~~苦しみ~~楽しみたい方にはおすすめです。
いままでありがとう、fastbin。

## その他の良問

惜しくも受賞を逃した問題たちです。

* 創造力賞
  + Stack Rhapsody - BlackHat MEA CTF 2025 Finals（[ソースコード](https://d.hatena.ne.jp/keyword/%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD%EF%BF%BD)がとてもシンプルで一見すると不可能ですが解ける面白い問題です。）
* [脆弱性](https://d.hatena.ne.jp/keyword/%EF%BF%BD%C8%BC%EF%BF%BD%EF%BF%BD%EF%BF%BD)賞
  + piano - HKCERT CTF 2025 Quals（例外発生時のスタックの扱いでコーナーケースが発生し、結果としてUse-after-Freeにつながるという、一見パッチから何が起こるかわかりにくい問題でした。）
* 教育賞
  + LPE - CODEGATE CTF 2025 Finals（[Windows](https://d.hatena.ne.jp/keyword/Windows) 11のセキュリティ機構入門として良いと思います。）
  + RandomJS - ASIS CTF 2025 Quals（[JavaScript](https://d.hatena.ne.jp/keyword/JavaScript)のUse-after-Free入門として良いです。）
* 風水賞
  + pryspace - TSG CTF 2025 Quals（厳しい制約で巨大なunsortedbinを作るというア[イデア](https://d.hatena.ne.jp/keyword/%EF%BF%BD%EF%BF%BD%EF%BF%BD%C7%A5%EF%BF%BD)が斬新でした。）

[\*1](#fn-1ba4fcd1):ヒープ問においては褒め言葉？

ptr-yudai
[2025-12-31 13:56](https://ptr-yudai.hatenablog.com/entry/2025/12/31/135605)

[読者になる](https://blog.hatena.ne.jp/ptr-yudai/ptr-yudai.hatenablog.com/subscribe?utm_campaign=subscribe_blog&utm_source=blogs_entry_footer&utm_medium=button)

[広告を非表示にする](http://blog.hatena.ne.jp/guide/pro)

* もっと読む

コメントを書く

[login-bonus (Daily AlpacaHack 2025/12/1…
 »](https://ptr-yudai.hatenablog.com/entry/2025/12/18/151253)

プロフィール

[![id:ptr-yudai](https://cdn.profile-image.st-hatena.com/users/ptr-yudai/profile.png?1535456024)](https://ptr-yudai.hatenablog.com/about)

🍣 sushi 🍣

読者です
読者をやめる

読者になる
読者になる

[このブログについて](https://ptr-yudai.hatenablog.com/about)

検索

[月別アーカイブ](https://ptr-yudai.hatenablog.com/archive)

* ▼
  ▶

  [2025](https://ptr-yudai.hatenablog.com/archive/2025)
  + [2025 / 12](https://ptr-yudai.hatenablog.com/archive/2025/12)
  + [2025 / 9](https://ptr-yudai.hatenablog.com/archive/2025/09)
  + [2025 / 4](https://ptr-yu...