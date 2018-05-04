---
title: 技術書典4で買ったもの
issue: 5
published: true
last-modified: 2018-05-04
---

2018年4月22日(日)に秋葉原で開催された[技術書典4][1]というイベントに一般参加しました。買ったものの感想をメモっておきたいと思います。

<!--more-->

技術書典は「技術書」専門の同人イベントであり、ニコニコ超会議と併設開催された「超技術書典」も含めると今回で5回目の開催となります。

以下、買ったものの一部の感想を書きます。サークルリスト順です。

* TOC
{:toc}

## 『数式組版』 -- ラムダノート株式会社

サークルリスト順なので最初に企業枠となります。

この本は、数式を組版 (typesetting) する際の理論を体系化しようと試みているものです。「数式とは何か」から始まることと、TeX をメインとして **いない** ことが気に入りました。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">『数式組版』についての感想、同感。こういう本を待ち望んでいた。<br>TeXを使い始めて何十年にもなるが、非常に自由度が高いので、どう組版すべきか迷うことも多い。そういう場合の考え方の指針になりそう。</p>&mdash; s.komata (@_kmt46) <a href="https://twitter.com/_kmt46/status/988475722875322368?ref_src=twsrc%5Etfw">2018年4月23日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## 『Coq/SSReflect/MathCompによる定理証明』 -- 森北出版

企業枠です。

定理証明支援系 Coq で数学の証明を形式化することを解説した本です。Coq を使った四色定理の証明にも使われた MathComp ライブラリを活用して、具体的にどのように数学の証明を形式化していくのかが解説されていました。逆に、ソートの正当性証明などのコンピュータ・サイエンス寄りの題材は主ではありません。Coq に触ったことが無い方向けの、丁寧な入門書だと感じました。

## 『日本語入力を作るのに必要だった本』 -- SKK=さらに かしこく かわいい

この本は、macOS 上で :emoji: 形式の入力を絵文字に変換する入力メソッドを作っていくことで、入力メソッドの仕組みを理解するものです。実際のコードベースに、API の解説や、明確な文書化がされていない情報についての解説が為されていました。

個人的には、VS Code などの新しいエディタが開発される中で入力メソッドに関連するバグをいくつか[見る機会があり](https://twitter.com/mattn_jp/status/918116306599026688)、こういったものがどうやって生じるのか知りたい、というモチベーションがありました。たとえばよくあるものとして未確定文字が確定文字として打たれてしまう、二重入力のバグがありますが、この本の最後にこれについても短く述べられていました。僕たちは日本語入力に慣れているのでこういったバグを聞くとすぐ想像が付きますが、アメリカで想像されにくいことは容易に想像できます。逆に我々が想像しにくいような仕組みについて色々知りたいなあとも思いました。こういうのって[テスターさんがやるお仕事](https://html5experts.jp/shumpei-shiraishi/24538/)に繋がっていくのでしょうか。

## 『工場実習日記』 -- 工場夜景

元々購入リストに入れていなかった、かつ、会場が混雑していてその場で購入判断しないようにしていたために会場では買いませんでしたが、話題になっていたため後から電子版を購入しました。

読んでてつらいです。酷いと適応障害になりそうな状況があちらこちらに見え隠れしています。僕は 1 章読み終わるごとに休憩しました。この本は工場に限った話ですが、きっと同じような話は色んな職場に転がっているのだろうと思いました。自分が適応しやすい場所を自分で選択していくこと、大事。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">ろくにプライバシーもない寮に詰め込まれ、ソフトウェアとはおよそ関係のない単純作業をさせられることは、基本的人権の侵害に限りなく近い。しかしこれを製造業の人達に理解しろといっても無駄なのかもしれない。闇を感じる。必読。 <a href="https://t.co/6xXBlGvkdp">https://t.co/6xXBlGvkdp</a></p>&mdash; Kenji Rikitake (@jj1bdx) <a href="https://twitter.com/jj1bdx/status/988677786578575360?ref_src=twsrc%5Etfw">2018年4月24日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">工場実習日記が話題ですが、現在進行系の闇を見たい方は、「工場研修」でツイートを検索するのをおすすめします。現在進行系で工場に送られている人達が観測できます。つらい。</p>&mdash; 社会の闇、よんた (@keita44_f4) <a href="https://twitter.com/keita44_f4/status/988624870605864960?ref_src=twsrc%5Etfw">2018年4月24日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## 『SVGで作ろうローディングアニメーション』 -- CANDY CHUPS Lab.

Illustrator で SVG を作り、出てきた XML に手書きでアニメーションを付け足して SVG アニメーション (SVG SMIL) を作る本でした。

アニメーション手書きはつらいなー、ツール欲しいなあと思ってググったら[山のようにあった](https://superuser.com/q/171677/680903)。また、CSS アニメーションなど他のアニメーション技術との比較も ["A Comparison of Animation Technologies"](https://css-tricks.com/comparison-animation-technologies/) や ["GPU accelerated CSS animation vs SVG native animations"](https://stackoverflow.com/q/25233248/5989200) に載っていました。このあたりの技術選択は慎重にやりたい。実際に業務となるとデザイナーの方々との兼ね合いもあるので一律には決められなさそう。

あとやっぱり思ったのは、モーショングラフィクスすごいなーかっこいいなー、ということ。この本では簡単なローディングアニメーションが完成するのですが、シンプルであってもやっぱり見栄えが良いですね。僕はデザイン系さっぱり分からないので[「モーション周期表」](http://foxcodex.html.xdomain.jp/index.html)や [Everyday One Motion](https://twitter.com/motions_work) などを眺めることしかできません。少しでも感覚が掴めたらまだ良いのですが。

## 『日経電子の本』 -- Nikkei Engineer Team

今回のベスト・インパクト賞。事前に何が頒布されるのか調べていなくて、とりあえず買おうとだけ思っていました。蓋を開けてみると技術記事が載った **新聞** が無料配布されていました。でかい。手持ち袋に入らない。すべての記事が技術関係の新聞を受け取るのは初めてです。いや、『日経電子の本』だから本なのかな……？

内容も興味深かったですがそれよりも印象深かったのが、技術記事と新聞の親和性についてです。新聞形式になるとこんなに全容が把握しやすいとは思っていませんでした。コードと本文が自然に横並びになるからなのか何なのか分かりませんが、紙面が広いとこういう配置ができるんですね。

## 『言語処理系のススメ - 進捗大陸03』 -- 進捗大陸

* **Raspberry Pi の GPU を使うコンパイラ**: GPU 向けコンパイラの話は今まで読んだことが無かったので興味深かったです。現代の CPU に比べハードウェアの制限がキツそうでした。OpenCL をサポートする、RasPi GPU 向けのコンパイラを作ろうという話で、実際には OpenCL C/C++ --> LLVM IR --> SPIR-V までは既存の OpenCL の仕組みを使って出来るので、そこから RasPi GPU 向けバックエンドを作ろう、という話です。それ今まで無かったの？！、という感じですが、確かにちょっとググった感じだと、[開発が進んでいるものは無かった](https://raspberrypi.stackexchange.com/q/41)ようです。GPU の仕様が公開されたのが 2014 年で、そこから模索が始まり、今回の記事で取り上げられている [VC4CL](https://github.com/doe300/VC4CL) / [VC4C](https://github.com/doe300/VC4C) が公開されたのが 2017 年です。VC4C は[修論](https://github.com/doe300/VC4C/blob/master/doc/thesis.pdf)の成果みたいなのですが、それがドイツ語で書かれているので Google 翻訳を使って英語に直しつつちまちま読んでいます……。VC4C はまだ最適化が進んでいないと書かれていましたが、[SPIRV-Tools](https://github.com/KhronosGroup/SPIRV-Tools) の Optimizer とか転用できないのかなあ。まだよく分かっていないので、もう少し調べます。
* **LLVM で作り直す自作言語入門**: <https://github.com/yutopp/jade> の話でした。ここまでの進捗大陸では文鳥言語という自作言語の処理系の実装について語られてきましたが、実装が複雑になってきてつらいので、最適化の役割を分けた中間言語を作って変換パスを綺麗にしようという話。個人的には Rust の [MIR が提案された経緯](http://rust-lang.github.io/rfcs/1211-mir.html) を知れました。
* **BuckleScript で Transpiler を作る**: OCaml から JavaScript へのトランスパイラである BuckleScript を使って、S 式で書かれる altJS の処理系を作る話でした。要するに S 式をもらって JavaScript AST を生成する処理系を OCaml + BuckleScript で書き、BuckleScript を使って JavaScript で書かれた処理系に翻訳しています。最初は動機がよく分からなかったのですが、HTML Canvas 上で動くノベルゲームで使われるシナリオスクリプトの処理系にこのやり方を使ったと最後の章に書かれていたので腑に落ちました。
    * ところで本文中に「(OCaml の) パイプライン演算子 `(|>)` は F♯ という言語から輸入された」と書かれていて、気になったので調べてみました。すると、ウェブ上の情報からは以下のことが分かりました。
      
      1. パイプライン演算子自体は F♯ 以前の 1994 年には既に存在しており、Isabelle で使われていた。このことは F♯ 開発者の Don Syme 氏もご存知。(出典: ["Archeological Semiotics: The Birth of the Pipeline Symbol, 1994"](https://blogs.msdn.microsoft.com/dsyme/2011/05/17/archeological-semiotics-the-birth-of-the-pipeline-symbol-1994/))
      2. OCaml のメーリングリストである caml-list では、1998 年には関数合成する演算子について議論がなされている。この際には `(|>)` という名前は見当たらない。この議論に Don Syme 氏が参加していた。(caml-list, ["Functional composition operator?"](https://inbox.ocaml.org/caml-list/13932.45585.522844.651609@hadar.cs.Buffalo.EDU/), John Whitley, 8 Dec 1998)
      3. F♯ 1.0.0 がリリースされたのは [2005 年頃](https://blogs.msdn.microsoft.com/dsyme/2005/01/05/welcome-to-dons-f-blog/)。F♯ 1.x は 2010 年まで続いたが、2006 年には既に pipeline forward 演算子 `(|>)` について言及したブログ記事が[あった](http://www.secretgeek.net/fsharp_3mins)ので、初期から `(|>)` は存在した模様。
      4. OCaml Pervasives に `(|>)` が追加されるより前、2008 年から、OCaml のライブラリである Batteries の中の Extlib には関数の逆適用演算子 `(|>)` が[あった](https://github.com/ocaml-batteries-team/batteries-included/commit/8d5b064443d9c5e135e956b658913e5244ee7122#diff-735b40ec4cedd41041e12f2556598d60R15)。Batteries Included v0 の[リリース](http://forge.ocamlcore.org/forum/forum.php?forum_id=145)より少し前の話みたい。
      5. 一方 caml-list では、2010 年に再び関数合成の演算子について議論された。このときはじめは演算子の形が `(|>)` でなかったが、発案者とは別の方によって「F♯ の `(|>)`」として `(|>)` が紹介された。(caml-list, ["Infix function composition operator"](https://inbox.ocaml.org/caml-list/1289359172.2282.15.camel@azayaka/), Arlen Christian Mart Cuss, 10 Nov 2010)
      6. OCaml の Pervasives には 2013 年の[このコミット](https://github.com/ocaml/ocaml/commit/ace0205b6499ffdae4588cfdd640c45855217a8f)で追加されている。OCaml 4.01 から使えるようになった。
      
      というわけで、確かに OCaml の `(|>)` は F♯ の影響を受けていました。ただし、`(|>)` 自体は F♯ 以前から存在していました。また、caml-list を漁る限り、Batteries に追加される前から自前で `(|>)` や `(@@)` を定義して使っていた方々はそれなりに[いらっしゃったようです](https://inbox.ocaml.org/caml-list/20070918085310.GB12115@localhost/)。ログが残っていて誰でも考古学者ができるのは最高ですね。
    * OCaml Pervasives では `(|>)` と `(@@)` という非対称な名前付けがされているのは何故なんでしょうね。
* **Erlang VM で動く静的型付き言語 Alpaca の紹介**: Alpaca は元から知っていたので確認のように読みました。ただ僕は Erlang を常用しないので、なかなか自分で使わないんですよね。静的に型付けしたことで何ができなくなったのかが書かれているのが良いなあと思いました。

全体的に解説が丁寧で面白かったです。

## 『iSeq探訪』 -- みさきとミギー

Ruby 処理系の 1 つである Matz's Ruby Implementation (MRI) の内部の Virtual Machine (VM) で使われている命令セット iSeq について書かれた本です。Ruby プログラムが iSeq に変換され、それが VM で実行されます。この本では "Hello world!" の iSeq を眺めることから始め、最終的に MRI のソースコードから iSeq のバイナリ・フォーマットを解説することを試みています。iSeq の命令セットは [insns.def](https://github.com/ruby/ruby/blob/96db72ce38b27799dd8e80ca00696e41234db6ba/insns.def) に書かれているため、iSeq で遊ぶために後は iSeq バイナリが弄れるようになりたい、という流れです。僕も自作 CPU の命令セットに「人生、宇宙、全ての答え」を[入れておけば](https://github.com/ruby/ruby/blob/96db72ce38b27799dd8e80ca00696e41234db6ba/insns.def#L1360)良かったなあ。

## 『Recursion Scheme テクニック』 -- team scomber

ML 系言語において、実装を上手く隠蔽したシグネチャを書くために recursion scheme を使う、という functional pearl の解説です。代数的データ型を使う際、パターンマッチできるようにシグネチャを設計しようとすると、ナイーブには型の詳細を公開することになってしまい、場合によっては適切な抽象化ができません。そこで実際の型とは別にパターンマッチ用の型を用意して、そちらのみ公開しよう、という流れです。より具体的には、前半は論文 "Functional Pearl: Programming with Recursion Schemes" (Daniel C. Wang & Tom Murphy VII, 2002, draft) と同様の内容の解説、後半は前半の活用例という構成でした。自分でも OCaml でちょっと[書いてみた](https://gist.github.com/nekketsuuu/6733a0f3f9074161480cf6a8e8485624)んですが、面白いなと思う一方、過度な抽象化なのではと感じる側面もありました。

## 『ZIP、完全に理解した』『モナーでもわかるビットコイントランザクションスクリプト（プレビュー版）』 -- superflip

ZIP 本は、ZIP のファイル形式の説明から始まり、よく使われている ZIP の暗号化手法 Traditional PKWARE Encryption の紹介とその解読について解説されていました。具体的なコード付きで解説されていたので、これはソースコードをコピペできる電子本の方が良かったかな……、と思いましたが、[著者によってソースコードが公開されている](https://github.com/kusano/understandzip_web)ことに気付いたので問題なさそうです。ところでこの本の[公式ウェブページ](http://sanya.sweetduet.info/understandzip/)には正誤表が記載されており、誠実で凄いなあと思いました。

こういうファイル形式の知識って CTF などで使うのでしょうか。僕は全く知らなかったです。個人的に面白かったのは次のポイントです。

* ZIP や PNG では、CRC の計算は xor を減算とした除算を使って行うこと。
* ZIP は deflate 圧縮以外にもサポートしている圧縮形式があること (BZIP2 や Deflate64 の他にもある)。
* 暗号化 ZIP に対する攻撃の具体的なアルゴリズムの解説。

トランザクションスクリプト本には知らないことしか書かれていませんでした。そもそも送金時の署名と署名検証スクリプトが (適当な命令セットを使って) 自由に書けるということを知りませんでした。確かにこうすると電子署名の方式を仕様に含めなくてよいのでセキュリティ面で冗長性が生まれますね。なるほど。

## 『Unity Graphics Programming vol.2』 -- Indie Visual Lab

以前購入した vol. 1 を未だに積ん読しているので vol. 2 も積ん読です……ごめんなさい……。面白そうなことをしている本なのでちゃんと読みたい……。

## 『Action on Googleで作る音声操作ロボットの作り方』 -- 	俺のLab.

Google Home や Google アシスタントを使って音声認識し、その入力を元に動く四足ロボを作る話。ガワは 3D プリンタで自作。どの電池でどのサーボを動かすのか、という話から、最終的にどうやって Google の音声認識と繋げるのか、までが解説されている本でした。音声認識で動くロボットというと、どうしても大人向けロボット大会のイメージが強く、レベルが高いものだと勝手に思っていましたが、確かに Actions on Google を使うとある程度までは気軽に実装できる！　この発想が欲しかったです。

技術書典には、この本に限らず 3D プリンタで筐体を自作するロボット・サークルがあって、驚くばかりです。自分はコンピュータ科学の学生であり、機構の設計の仕方や、3D モデリングのやり方について知識が足りません。みなさんどういう思考を辿ってロボットの形を決めてらっしゃるんですかね？　3D プリンタ系の本ではよく「ここに 3D データがあるので、まずはこれをプリント。ここに気をつけながら、こう組み立てる」といった形の解説を目にしますが、それだけでなくて、どうしてそのような設計に至ったのかも解説頂けると僕が嬉しいです。

## 『Do It Ourselves: 社会人サークルによる自律移動ロボット開発』 -- Project C.G.S.

つくばチャレンジと MBZIRC というロボットの大会に参加した社会人サークルが出された本です。単にこういう外見のロボットを作って大会の結果はこうだった、というだけでなく、どのようなアルゴリズムを使ったかや、どのような機構を使ったか、センシングの結果どのようなデータが得られたか、などが適度な粒度で書かれており、ロボット初心者の僕にも概要がある程度具体的に想像できました。自分でやりたくなった人のためのポインタもある程度張られており、やさしい本だと思いました。

内容としては、たとえばつくばチャレンジについては、大会のルール概要と、実際に使ったハードウェアの概説、ナビゲーション・システムのアルゴリズムの詳しい説明、実際に得られた点群 (ポイント・クラウド) の可視化、学習のために Tensorflow をどう使ったかの詳細な解説、そして今後の課題が書かれていました。

個人的には、つくばでロボットを走らせていると子どもが面白がって立ち寄ってくるのでハラハラするという文脈で、

> ただ、最近はつくば市でロボットが走っているのは最早「普通のこと」になってまったのか、昔ほどは子供が興味を示さなくなってしまいました。 *(原文ママ)*

と書かれていたのが面白かったです。

## 『micro:bit こども向けプログラミング本』 -- micro:friends

[micro:bit](http://microbit.org/ja/) をご存知でしょうか。プログラマブルな小さい基盤で、ボタンとチップ LED マトリクスなどが付いています。ブロック / JavaScript / Python のプログラミング環境が提供されています。イギリス発であり、イギリスの 11 歳と 12 歳のすべての子どもたちに無料配布されたことで有名です。

『micro:bit こども向けプログラミング本』は、micro:bit を使ってモノを作るための、子ども向け本でした。この本では、Lチカから始まり、サイコロ、音楽プレーヤー (スピーカーを外付け)、イライラゲーム (スピーカーの途中でイライラ棒に繋げ、通電したら音が鳴る)、円周上に LED を並べた [ZIP Halo](https://www.switch-science.com/catalog/3484/) の接続、[心拍センサ](https://www.switch-science.com/catalog/3208/)の接続、という内容でした。

手順の写真やスクリプトのスクリーンショットが豊富であり、発展的な内容にも踏み入っているのが良いなあと思いました。「頑張れば凄いことできるんだぜ！」というアピールは大事だと思います。一方で、子どもに読ませることが想定されているのであれば、漢字が難しすぎると思いました。

あ、そういえば micro:bit と言えば最近[『手づくり工作をうごかそう! micro:bitプログラミング』](http://amzn.asia/7IG4VMQ)が出てましたね。良さそうなのでチェックせねば……。

## まとめ

晴れたこともあり、いつも以上に混雑した技術書典でした。今回は流石にその場で追加購入を考える暇がなかったです。今回はイベント終了後、BOOTH の <https://booth.pm/ja/events/techbookfest-4> にて電子出版を公開して下さるサークルさんが多くて助かりました。

今年は参加者が多く、最後には整理券が手書き発行されていたみたいですね。あれだけの人数を捌ける運営さん凄いなあ。技術季報 vol.3 によるとサークル数も増えているらしいので、盛り上がりを感じます。また、今回は同日に別の技術書イベント 技書話人伝 が開催されてもいましたね。このままマッハ新書みたいな流れに繋がっていくのでしょうか。

それでは、また次回。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">ただいまを持ちまして、 <a href="https://twitter.com/hashtag/%E6%8A%80%E8%A1%93%E6%9B%B8%E5%85%B84?src=hash&amp;ref_src=twsrc%5Etfw">#技術書典4</a> 閉幕しました！今回の総参加者数は6380人でした。<br>皆さんご来場ありがとうございました。</p>&mdash; 技術書典公式アカウント (@techbookfest) <a href="https://twitter.com/techbookfest/status/987965787284516864?ref_src=twsrc%5Etfw">2018年4月22日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

  [1]: https://techbookfest.org/event/tbf04
