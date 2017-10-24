---
title: 技術書典3で買ったもの
issue: 2
published: true
last-modified: 2017-10-23
---

2017年10月22日(日)に秋葉原で開催された[技術書典3][1]というイベントに一般参加しました。買ったものの感想をメモっておきたいと思います。

<!-- more -->

技術書典は「技術書」専門の同人イベントであり、ニコニコ超会議と併設開催された「超技術書典」も含めると今回で4回目の開催となります。

以下、買ったものの一部の感想を書きます。サークルリスト順です。

* TOC
{:toc}


## 『Goならわかるシステムプログラミング』『定理証明手習い』 -- ラムダノート株式会社

サークルリスト順なので最初に企業枠となります。

『Goならわかるシステムプログラミング』は [Web 連載][2]を書籍にしたものです。ただし目次を見ると分かりますが、全体的に再構成と加筆がされているみたいでした。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">『Goならわかるシステムプログラミング』は、おなじみアスキーjpの連載の書籍化だけど、再構成と怒涛のレビュー、さらに新章の追加により、全体にコンテンツ力が格段にあがっています。OSの教科書の副読本としても最高。 <a href="https://t.co/8MztMyKH8O">https://t.co/8MztMyKH8O</a></p>&mdash; keiichiro shikano λ♪ (@golden_lucky) <a href="https://twitter.com/golden_lucky/status/920854720591175680?ref_src=twsrc%5Etfw">2017年10月19日</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

『定理証明手習い』は ["The Little Prover"][3] の和訳 (中野先生監訳)。まだちゃんとは読んでいませんが、主張がプログラムとして与えられるので、プログラムを式変形して `'t` (真を表す値) にしてみよう、という形で主張を証明していくようです。LISP ベースです。実際に手を動かしながら読むのが良さそうなので、これは紙媒体じゃなくて電子書籍として買った方が良かったかも……と今パラパラめくりながら思いました。


## 『Progmatic Opal』 -- みさきとミギー

[Opal](https://github.com/opal/opal) という Ruby から JavaScript へのトランスパイラを使って遊ぶ本。実際に実用アプリを作ることを通して使い方を概観できる。個人的にこういうトランスパイラは、何故それがあると便利なのかという視点が大事だと思っていて、たとえば OCaml から JavaScript へのトランスパイラである [BuckleScript][5] には「OCaml の型システムを活用できる」などのメリットがあります。この点に関して Opal はどうなのかというと、Opal のドキュメントには

> **Why does Opal exist?**
> 
> To try and keep ruby relevant in a world where client-side apps are making javascript the primary development platform.

と書いてあって、「Rubyist が嬉しい」以上の大きな理由は無いっぽいなあという感じです。もちろん実用上は既存の Ruby コードが再利用できるという話があるのだと思いますが、JavaScript 側の仕様の関係上 Opal にも制限があるようで ([参考1][6], [2][7])、このあたりは実際に使ってみないと Ruby で書くメリットがよく分からないなあと感じました。


## 『解説 Embedded Interpreters』 -- team scomber

@eldesh さんによる StandardML 解説シリーズの4冊目。[DSL][8] を実装する上で、multi-stage programming 的なことができるようにして遊ぼうという本。以下、序文からの引用です。

> 一見面白みのないよくある DSL 用の *embedded/projection* ペアと呼ばれるコンビネータを組み上げて少しずつ拡張していくと、意外な機能拡張ができたり、高速化ができたり、SML では実行不可能な関数が評価できてしまったりします。

ところでこの本を読んでいて、OCaml で `exception E of 'a` と書いて例外を多相にしようとしてもエラーになることに気づきました。素直な polymorphic exception を許すと、例外をデコンストラクトするときに型が何でもよくなっちゃうのが[良くないみたいです][10]。怖いなあ。

<!--
**良くない例** (OCaml):

以下は、例示としてはあまり上手くないが、polymorphic exception を許すとヤバそうな例です。(型推論のルールにも依る。)

``` ocaml
exception E of 'a  (* OCaml 4.05.0 ではこの行で Unbound type parameter のエラーになる *)
let mymagic x =    (* mymagic : 'a -> 'b になってしまう *)
  try
    raise (E x)
  with E x -> x
```

(この例は OCaml の issue トラッカーからもってきました。[こちら][11]と、その原典らしき[こちら][12]です。)

自分用メモ: `exception E of 'a` は OCaml 1.07 の時点ですら既にエラーなのですが、`exception E of _` がエラーじゃなかったのがまずい、というのが上の例の本当のポイントです。OCaml 3.09.3 からは後者もエラーになります。[このコミット][26]によって変更されました。

自分用メモ: "Developing applications with Objective Caml" には `exception E of 'a` に対して `try raise (E false) with E x -> x + 1` がヤバいと書いてあるのだが、そもそも `(+) : int -> int -> int` なので `x + 1` は less general だし、僕が何か誤解しているらしい。
-->

対して StandardML では限定的な polymorphic exception が素直に書けます ("The Definition of Standard ML (Revised)" の 4.6節 "Scope of Explicit Type Variables" などで述べられています)。「限定的な」と書いたのは制限があるからで、たとえばトップレベルにおいて `exception E of 'a` と定義するのはエラーになります。"ML for the working programmer" の第8章には以下のように書いてありました。

> When a polymorphic exception is declared, ML ensures that it is used with only one type, just like a restricted value declaration. The type of a top level exception must be monomorphic and the type variables of a local exception are frozen.

<!--
自分用メモ: あまり歴史について調べきれていませんが、["The Standard ML Core Languages"](http://sml-family.org/history/SML-proposal-7-84.pdf) (July 1984) の第10章 "Syntactic restrictions" (7) にも

> Every global `exception` binding -- that is, not localised either by `let` or by `local` -- must be explicitly constrained by a monotype.

と書いてあるので、古くから知られていたことのようです。
-->

実際に使ってみる例を挙げてみます。

**使用例1** (SML):

``` sml
(* SML/NJ 110.81 で動作確認 *)
fun newdyn () =
  let exception E of 'a in
    (E, fn (E a) => a)
  end;
end;
```

(この関数がどう嬉しいかは今回の同人誌に書いてありました。)

**使用例2** (SML):

Finitely-branching な木の上で DFS する関数です。

``` sml
(* SML/NJ 110.81 で動作確認 *)
datatype 'a tree = Node of 'a * 'a tree list;
(* dfs : ('a -> bool) -> 'a tree -> 'a option *)
fun dfs p (t : 'a tree) =
  let
    exception Ok of 'a;
    fun auxdfs (Node(n, children)) =
      if p n then raise (Ok n)
      else foldl (fn (t, _) => auxdfs t) NONE children;
  in
    auxdfs t handle Ok n => SOME n
end;
```

(このプログラムはケンブリッジ大学の[講義資料][13]にあったものを少し改変したものです。)

**補足** (OCaml):

なお、上で挙げた SML の例みたく型変数のスコープがローカルなら、OCaml でも以下のように書くことができます。

``` ocaml
(* OCaml 4.05.0 で動作確認 *)
(* newdyn : unit -> ('a -> exn) * (exn -> 'a) *)
let newdyn (type t) () =
  let exception E of t in
  let embed x = E x in
  let proj (E x) = x in  (* この行でパターンマッチの warning が出るので本当は対処しないといけない *)
  (embed, proj)
```

#### 関連
{:.no_toc}

* [UniversalType][14] -- MLton
* [Local globally-quantified exceptions][15] -- okmij.org
    * [Re: [Caml-list] best and fastest way to read lines from a file?][16] -- caml-list
    * [Locally-polymorphic exceptions \[was: folding over a file\]][17] -- caml-list
* "Is it possible to raise exceptions with precise stream location information in StringCvt.scanInt functions?" の [回答][18] -- Stack Overflow
* [A universal type?][19] -- Jane Street Tech Blog


## 『進捗大陸02』 -- 進捗大陸

表紙がかっこいい本です。この本には3つ独立した記事が載っていました。それぞれについて感想を書きます。

### 自作言語 Rill と WebAssembly -- @yutopp

Rill (文鳥言語) のバックエンドに WebAssembly を追加したという話。Rill のライブラリが libc に依存しているのでそこを何とかする必要があって、何とかしていたのがつらそうな感じでした。Emscripten にはブラウザ環境用の [musl][20] (軽量libc互換ライブラリ) が[含まれている][28]というのが知見。

### 再帰を持つ関数型言語の符号解析 -- @youxkei

『進捗大陸01』に掲載された[抽象解釈][21]の記事の続き。前回に引き続き、関数型言語っぽいトイ言語で書かれた式の符号（プラス・マイナス）を解析する話ですが、前回と違って再帰関数が言語機能に含まれています。再帰部分を処理するために最小不動点をとっているので、健全性の証明が少し大変そう。

### 証明付き分散システムを作ろう! -- @amutake

分散システムの実装・検証をするために作られた Coq 上のフレームワークである Verdi の紹介と、実際に Raft というプロトコルが Verdi によって検証された際にどのような形だったのかの解説が載っています。Coq 自体の説明から丁寧に入っていて、全体的に読みやすかった && 面白かったです。


## 『Unity Graphics Programming』 -- Indie Visual Lab

Unity を使った 3D CG の技術解説本。全部で10章、計175ページと、力の入った厚い本でした。まだ全然読めていないです。目次はこんな感じ。

1. Unity ではじめるプロシージャルモデリング
2. ComputeShader 入門
3. 群のシミュレーションの GPU 実装
4. 格子法による流体シミュレーション
5. SPH 法による流体シミュレーション
6. ジオメトリシェーダ―で草を生やす
7. 雰囲気で始めるマーチングキューブス法入門
8. MCMC で行う3次元空間サンプリング
9. MultiPlane PerspectiveProjection
10. ProjectionSpray の紹介

TODO: 読む。


## 『Super Mario Maker プログラミング入門』 -- @norakyuuri

スーパーマリオメーカーで論理回路を組み立てる指南書。表紙が特殊加工でザラザラになってて手触りが良かったです。内容は、AND や OR、半加算器や全加算器だけの解説にせず、最後にマルバツゲーム AI (?) の解説を入れてくるところが新奇だなあと思いました。マルバツゲームだとそもそもの状態数が少ない上、手番を固定するなどの手抜きをすることで現実的な状態数まで減らせるので、あとはそれをマリオメーカーで実装するという形になっていました。

マルバツゲームの動作はこんな感じです: <http://www.nicovideo.jp/watch/sm31156135>


## 『可視化法学』 -- へその緒

第1号と第2号の2冊が頒布されていました。どちらも内容は、法律間の参照関係をビジュアライズするというもの (2冊で載っている分野が違う)。e-gov から条文を持ってきて処理することで参照関係をグラフにし、Gephi という可視化ツールに投げて絵にしてもらう、という形。冊子には分野ごとに絵が載っていて、その解説が書かれていました。参照関係が分かれば法律が理解しやすくなるのかどうかはよく分かりませんが、それはそれとして生成された図を見るのは楽しいです。こういうのは書籍ならではかもしれません。

法学的な内容に IT で立ち向かおうという話はロマンがあって好きです。たとえばドイツでは[法律が GitHub にある][22]という話とか、(昔の?)法律エキスパートシステムの話とか。調べていたら、知財周りの統計情報を処理して可視化している企業が[あったり][23]、契約内容を精査して粗を見つけるシステムを売っている企業が[あったり][24]して、ちゃんと調べればもっとありそうで楽しそうだなと思いました。


## 『Paper Robotics』 -- MPM

ほぼ紙だけで二足歩行ロボを作ろう！という内容の冊子。技術書典では初回から連続してロボット勢のサークルがいくつか出展なさっていて、楽しいです。本音を言うともっと増えてほしいのですが、あくまで[技術書][25]を売るイベントなので難しいのかもしれませんね。本題の紙ロボットは下の動画のように動きます。凄い。紙で作るロボットは最近だと[かみロボット研究室][27]あたりが面白いなあと思っていたのですが、この二足歩行ロボには技術力の塊で殴られた感触があって楽しかったです。

<http://www.geocities.jp/kikousya290821/newmpm.htm>

<iframe width="560" height="315" src="https://www.youtube.com/embed/sEs8MYhxFNk?rel=0&amp;start=79" frameborder="0" allowfullscreen></iframe>


## 『ROS ではじめるホビーロボット #06』 -- こそこそ団

Intel Edison + ROS + 3D プリンタで作ってきたかわいい自作ロボットの連載です。2017年をもって Intel Edison が終売になるニュースが最近流れていたのでどうなるのかなあと思っていたのですが、ちょうどその内容も入っていました。技術書典の会場に作成中のロボットがちょこんと座っていて、かわいらしかったです。(写真を撮っておけば良かった！)

そうそう、個人的に趣味でロボットをしたいと思いつつも、工学的な知識が足りずにいつも足踏みしているので、製作とか制御まわりの初心者本があれば<a href="https://github.com/nekketsuuu/nekketsuuu.github.io/issues/{{ page.issue }}">教えてください</a>。そろそろ LEGO Mindstorms から抜け出したい。


## まとめ

というわけで、初回の技術書典、技術書典2に引き続き、3回目の参加でした。今回はイベント当日に台風が来るというスケジュールであったため、運営さんお疲れ様でしたという感想です。独自作成の整理券とウェブアプリを使った人数調整、会場混雑を見越した休憩所の設置、QRコードを用いた電子後払い決済システムの導入など、良いアイディアがたくさん実装されていて凄かった。それではまた次回。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">ただいま17時をもちまして、<a href="https://twitter.com/hashtag/%E6%8A%80%E8%A1%93%E6%9B%B8%E5%85%B83?src=hash&amp;ref_src=twsrc%5Etfw">#技術書典3</a> 閉会いたしました！<br>台風直撃の最中、ユニークの来場者数では2750名、再入場者含む、のべでは3100名のご入場がありました。<br>ご参加本当にありがとうございました！ <a href="https://twitter.com/hashtag/%E6%8A%80%E8%A1%93%E6%9B%B8%E5%85%B8?src=hash&amp;ref_src=twsrc%5Etfw">#技術書典</a></p>&mdash; 技術書典公式アカウント (@techbookfest) <a href="https://twitter.com/techbookfest/status/922010073890615296?ref_src=twsrc%5Etfw">2017年10月22日</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


   [1]:  https://techbookfest.org/event/tbf03
   [2]:  http://ascii.jp/elem/000/001/235/1235262/
   [3]:  http://the-little-prover.org
   [4]:  https://github.com/opal/opal
   [5]:  https://github.com/BuckleScript/bucklescript
   [6]:  https://stackoverflow.com/q/23959511/5989200
   [7]:  http://opalrb.com/docs/guides/v0.10.5/unsupported_features.html
   [8]:  https://ja.wikipedia.org/wiki/%E3%83%89%E3%83%A1%E3%82%A4%E3%83%B3%E5%9B%BA%E6%9C%89%E8%A8%80%E8%AA%9E
   [9]:  http://caml.inria.fr/pub/docs/manual-ocaml-4.05/typedecl.html#sec156
   [10]: https://caml.inria.fr/pub/docs/oreilly-book/html/book-ora018.html
   [11]: https://caml.inria.fr/mantis/view.php?id=4002
   [12]: http://d.hatena.ne.jp/flappphys/20060410#p5
   [13]: https://www.cl.cam.ac.uk/teaching/1213/ConceptsPL/l7.pdf
   [14]: http://mlton.org/UniversalType
   [15]: http://okmij.org/ftp/ML/index.html#poly-exn
   [16]: https://sympa.inria.fr/sympa/arc/caml-list/2007-10/msg00031.html
   [17]: https://sympa.inria.fr/sympa/arc/caml-list/2007-10/msg00038.html
   [18]: https://stackoverflow.com/a/20504731/5989200
   [19]: https://blog.janestreet.com/a-universal-type/
   [20]: http://www.musl-libc.org/
   [21]: https://ja.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E8%A7%A3%E9%87%88
   [22]: https://github.com/bundestag/gesetze
   [23]: http://jp.ipnow.cn/index.html
   [24]: https://wired.jp/2017/03/23/lawyer-robot/
   [25]: https://techbookfest.org/event/tbf03#faq
   [26]: https://github.com/ocaml/ocaml/commit/4dc51329d4705797d0dc545317f753453d8aa22b#diff-283538db7c320a41442d987f0c880fc5
   [27]: http://kamiroboken.com/
   [28]: https://github.com/kripken/emscripten/tree/master/system/lib/libc
