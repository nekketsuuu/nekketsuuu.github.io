---
title: SATySFi 0.0.3 で行列を組版してみた
issue: 9
published: true
last-modified: 2018-10-12
---

SATySFi 0.0.3 を使って行列を組版してみました。以下のリポジトリで公開しています。使い方も含めてリポジトリで説明しています。

<https://github.com/nekketsuuu/satysfi-matrix>

この記事では SATySFi のライブラリ作者向けに、このライブラリを作る際に躓いたポイントをメモしておこうと思います。

**この記事は SATySFi 0.0.3 時点での情報です。バージョンアップに伴い内容が古くなっている可能性が高いです。注意してください。**

## \show-length が欲しい！

length を単位長さで割ると float が出てくるので `\show-float` できるようになります。これで length を出力できるようになります。

```satysfi
let-incline \show-length len = embed-string (show-float (len /' 1pt) ^ `pt`)
```

メモ：ぴったり整数のとき、`42.pt` のように小数点以下の 0 が省略されて出力されてしまうバグがあります。

## set-manual-rising で高さが変わらない？

`set-manual-rising : context -> length -> context` はインラインの**テキスト**の高さを変えるために使います。このため、math や inline-graphics に対して manual_rising を変えても効果がありません。代わりに `raise-inline : length -> inline-boxes -> inline-boxes` を使います。`set-manual-rising` はテキストの高さを局地的に変えますが、`raise-inline` はインラインボックスごとぐいっと上げることができます。

実際の使い方は公式ライブラリ `math.satyh` の `\cases` が分かりやすいです。

## inline-boxes の高さと深さが知りたい

`get-natural-metrics : inline-boxes -> length * length * length` を使うとできます。これは doc-primitives.saty にはまだ載っていませんが、The SATySFibook に載っています。

```satysfi
let (width, height, depth) = get-natural-metrics ib in ...
```

## tabular プリミティブ

行列の中身は表として扱うことができるので、表を扱うプリミティブを触ることになります。SATySFi には tabular プリミティブが用意されており、これは doc-primitives.saty には載っていませんが The SATySFibook に詳細な解説があります。まずは解説を読みましょう。

tabular プリミティブでは、表の各要素がどういうセルであるかの情報と罫線の情報を渡します。罫線無しの場合は第 2 引数は `(fun _ _ -> [])` とすれば良いので気になるのは第 1 引数であるセル情報です。これは場合によって `EmptyCell` / `NormalCell` / `MultiCell` を使うことになります。

セルの中身ですが、まずパディング `pads` は length 4 つの組 (タプル) で、左右上下の空白幅を示します。次に中身となるインラインボックスですが、`inline-fil` を上手く入れることで左寄せや中央寄せなどが表現できます。この点に関しては公式ライブラリ `table.saty` が分かりやすいです。

## 表の最も外側のパディングが邪魔

先頭や末尾のセルのパディングだけ変えてやると外側のパディングだけ狭くできます。

## そもそも行列ってどうやって組版するのが適切なの？

僕は『数式組版』(木枝祐介、2018) を参考にしています。現状の satysfi-matrix v0.0.1 の組版もまだちょっと微妙だなあと思う点があり、future work です。
