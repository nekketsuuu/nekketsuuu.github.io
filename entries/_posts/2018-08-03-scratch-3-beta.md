---
title: Scratch 3.0 beta で気づいたこと (随時更新)
issue: 6
published: true
last-modified: 2018-08-16
---

2018 年 8 月 1 日、Scratch 3.0 が <https://beta.scratch.mit.edu> で公開されました。これまで 3.0 は <https://preview.scratch.mit.edu> 等で公開されていましたが、今回はこれをより広く公開したバージョンということのようです。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="en" dir="ltr">Scratch 3.0 has even more ways to create and get started. Try the Beta version now! <a href="https://t.co/CUWdJYRvNJ">https://t.co/CUWdJYRvNJ</a> <a href="https://t.co/8p0Sw2jcvv">pic.twitter.com/8p0Sw2jcvv</a></p>&mdash; Scratch Team (@scratch) <a href="https://twitter.com/scratch/status/1024701450813890560?ref_src=twsrc%5Etfw">2018年8月1日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Scratch Team が ["Try the Scratch 3.0 Beta today!"](https://medium.com/scratchteam-blog/try-the-scratch-3-0-beta-today-b50a05d63348) という記事を出しており、同時に教育者向けと教材開発者向けに更新通知を行うメーリングリストの告知を行っています。

Scratch 3.0 と 2.0 の差異は Scratch Wiki 日本語版の[この記事](https://ja.scratch-wiki.info/wiki/Scratch_3.0)によくまとまっています。主要な差異はこの記事で網羅されているので、この記事では僕が Scratch 3.0 を触っていて気になった些末な挙動やバグについてまとめたいと思います。正式リリース時にどうなっているか確認するためのメモでもあります。

追記: 2.0 から 3.0 へするときに現れる非互換性については、フォーラム [PROJECT INCOMPATIBILITY LIST](https://scratch.mit.edu/discuss/topic/308114/) でまとめられています。

追記: <https://scratch.mit.edu/> での作品 ID を <https://beta.scratch.mit.edu/#68760268> のように URL 末尾につけると、Scratch 2.0 の作品を 3.0 として読み込むことができることを知りました。

**以下の情報は随時更新する予定であり、また 3.0 beta は頻繁に更新されうるため、古い情報が簡単に入りえることをご承知おきください。**

## Scratch GUI

<https://github.com/LLK/scratch-gui> で開発されています。

* Undo / redo が実装されている！
* スプライトの x, y 座標が書かれる位置が変わったのに伴い、マウスカーソルの x, y 座標は表示されなくなった。
    * ステージの縦横がそれぞれ何ピクセルなのか分かりにくくなったかもしれない。
* スプライト一覧側のスプライトをクリックしても、ステージ上でどこにいるかハイライトされなくなった。ステージの外に行っちゃったときや、隠しているとき、同じコスチュームのスプライトが複数いるときにどこにいるのか確認しづらくなったので、注意。
* 画面を掴むことでスクリプトエリアを移動できるようになった。便利。

## Scratch Blocks

<https://github.com/LLK/scratch-blocks> で開発されています。

* 「イベント」と「制御」の色が Scratch 2.0 と逆転している。これは ScratchJr に色を合わせたから。([#1489](https://github.com/LLK/scratch-blocks/issues/1489))
* 日本語で長いコメントを書いた後コメントを閉じると、開けなくなる。([#1680](https://github.com/LLK/scratch-blocks/issues/1680))
* ビルドに Python 2.x が必要。ただし簡単に Python 3.x へ移行できそう。([#1679](https://github.com/LLK/scratch-blocks/issues/1679))
* 「○度に向ける」の角度指定ツールが分かりやすくなっている。

## Scratch Paint

<https://github.com/LLK/scratch-paint> で開発されています。

* デフォルトがベクターモードになっており、更にベクターモードでブラシと消しゴムが使えるようになっている。
    * <blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">Scratch 3.0 betaのscratch-paint、デフォがベクターモードで、2.0と違ってベクターモードでもブラシと消しゴムが使えるようになってる。塗りつぶした領域を消すと分割されて別パーツ扱いになったりする (なるほど)。どういう思想でベクターを優先するデザインにしたのだろう <a href="https://t.co/QupnIlUAor">https://t.co/QupnIlUAor</a> <a href="https://t.co/Bfwyfn9Hum">pic.twitter.com/Bfwyfn9Hum</a></p>&mdash; ねっけつ (@nekketsuuu) <a href="https://twitter.com/nekketsuuu/status/1024961881197305856?ref_src=twsrc%5Etfw">2018年8月2日</a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
* Scratch 2.0 の頃にあった、「中心」を変えるツールがまだ無い。([#32](https://github.com/LLK/scratch-paint/issues/32), [#456](https://github.com/LLK/scratch-paint/issues/456), [#528](https://github.com/LLK/scratch-paint/issues/528))
* 色選択ツールにカラーパレットが無い。([#580](https://github.com/LLK/scratch-paint/issues/580))
* 色選択ツールの HSV を手入力できない。
* 図形の上で右クリックしてもブラウザのコンテキストメニューが出るだけ。Scratch 2.0 でもペイントツール上で右クリックしても意味が無かったが、3.0 では逆に右クリックからコピー等ができないのに違和感を覚える。
* デフォルトがベクターモードなので、背景を塗りつぶそうとしてもできない（四角形を作るか、ビットマップモードにすることになる）。以前のように 1 ステップでは出来ないので注意する。

## EV3 Extension

初期設定の仕方は <https://scratch.mit.edu/ev3> で解説されています。拡張機能部分は <https://github.com/LLK/scratch-vm> で開発されています。

**TODO**: 再現性を確認して issue 報告する。

* Scratch Link (Windows 版) のインストーラが何かおかしい。アンインストール情報は記録するもののアプリ一覧に名前を記録しないので、アプリ一覧から名前を検索して実行することができなかった。インストーラが初めてインストールに成功した後起動オプションを選ぶことで起動できるので、「アンインストール→再インストール→オプションから起動」という流れを辿れば起動できます。
    * Scratch Link の issue トラッカーがどこか調べる。 → [分からなかったのでフォーラムで聞いてみました。](https://scratch.mit.edu/discuss/topic/311652/)普通に Scratch 3.0 beta のフォーラムで聞けば良いようです。
    * おそらく同じ issue: https://scratch.mit.edu/discuss/topic/311433/
* EV3 の接続を切ったり付けたりしたときの挙動が不安定。また、その場にいないはずの EV3 がアンテナ 4 本立って表示されたりする。
* EV3 のファームウェアを現在最新の 1.10 にしないと駄目。1.09 では動かなかった。    
* Scratch Link + EV3 Extension は PC 側の Bluetooth がオフになっていても検知してくれない。
    * また、EV3 側の Bluetooth がオフになっていても検知してくれないが、これは原理的に無理なので確認する必要がある。
* モーターポート A ～ D は内部的には 0 ～ 3 の数値として管理されており、ポート部分に変数等をはめこんで利用する場合は 0 ～ 3 を使う必要がある。同様にセンサーポート 1 ～ 4 も内部的には 0 ～ 3 として扱われている。
* モーターを動かすブロックが実質「1 つのモーターを ○○ 秒動かす」しかないため、EV3 Software における「タンク」に値する動作をさせるためには 2 つのブロックを並列に動作させる必要がある。これはメッセージを使えば実装できるが、マイブロックを作るときに不便。([#1447](https://github.com/LLK/scratch-vm/issues/1447))
* brightness でカラーセンサーの輝度を読めるように見えるが、赤色ではなく青色で測っている。どうも COL-REFLECT じゃなくて COL-AMBIENT を使っている。([#1446](https://github.com/LLK/scratch-vm/issues/1446))
* distance が cm モードではなく インチ モードになっていた。あるいは百分率？ ([#1444](https://github.com/LLK/scratch-vm/issues/1444))
* タッチセンサー以外のセンサーが複数個接続されることが想定されておらず、たとえば超音波センサーを複数個つけると distance が常に 0 になった。
* モーターの回転数が 0 ～ 360 でしか取れない。これは EV3 Software の挙動と異なる。この挙動のために、「モーターを ○○ 回転回す」というブロックを作りにくい。([#1287](https://github.com/LLK/scratch-vm/issues/1287))
    * [#1287](https://github.com/LLK/scratch-vm/issues/1287) で質問してみたところ、これは子どもが理解しやすいようにするためのデザインであり、仕様であるとのこと。
* Bluetooth 接続の都合上、Scratch から EV3 を触りながら、同時に EV3 Software + Bluetooth で触ることができないっぽい (要検証)。
    * EV3 Extention ブロックたちの右上にある接続ボタンから切断することができます。
* Scratch Link の仕様上？なのかもしれないが、Scratch Link につなぎながら EV3 上の Port View をすると、Scratch 側と測定モードが違ったときに不安定になる。
* 赤外線センサーが使えない。distance としても認識されない。([#1445](https://github.com/LLK/scratch-vm/issues/1445))
