---
title: Gitで管理しているLaTeXのdiffをpdfで見る(TeXLive2015版)
imgpath: /assets/entries/2017-01-27-latexdiff-vc/
published: true
---

## 結論: latexdiff-vcを使う。

<!--more-->

```
$ latexdiff-vc -e utf8 --git --flatten --force -d diff -r HEAD test.tex test.bbl
```

```
[alias]
	ldiff = !sh -c 'latexdiff-vc -e utf8 --git --flatten --force -d diff -r "${1:-HEAD}" test.tex test.bbl && cd diff && latexmk' -
```

## 仮定

TeXLive2015(以上)を使います。古いと`latexdiff-vc`が入っていないので、自分でインストールする必要があります。

僕の環境では、

* LATEXDIFF 1.1.0  (Algorithm::Diff 1.15 so, Perl v5.18.2)
* LATEXDIFF-VC 1.1.0
* git version 1.9.1

でした。`latexdiff`のバージョンが古いと`--flatten`が上手く動作せず、`git`のバージョンが相当古いとエイリアスが動かないみたいです。

## できること

![diffの画像]({{ site.baseurl }}{{ page.imgpath }}diff.png)

Gitなどのバージョン管理システムで管理されたLaTeXファイルのdiffを良い感じにPDFで出力できます。

現在この方法をググるとややこしいのが色々出てきますが、`latexdiff-vc`というのが便利になってたので以下ではそれについて書きます。

## 前準備

* Bib(La)TeXを使っている方は、`.bbl`ファイルもコミットしてください。
  * <div class="small80">.bblファイルは文献情報の入った.bibファイルやスタイルを示す.bstファイルを元に自動生成される(La)TeXファイルです。latexdiffが上手くdiffをとるにはこれを生成した状態でなければならず、さらにコミット間のdiffをとるにはこれがコミットされていないといけません。man latexdiff-vc にも.bblファイルはコミットした方が良いと書いてあります。</div>
* 日本語がメインのファイルを管理している場合、「latexdiff 日本語」などでググって適切な前処理をしてください。単語境界の設定と、`-t CFONT`による見栄えの調整が必要かもしれません。また、`tlmgr update`などで壊れるかもしれないので注意してください。
* 各ファイルはUTF-8でエンコーディングされているとします。違う場合はオプション引数を変えて下さい。ファイルのエンコーディングは`nkf --guess <file>`などで分かります。

## 結論(再掲)

```
$ latexdiff-vc -e utf8 --git --flatten --force -d diff -r HEAD test.tex test.bbl
```

これで「今あるファイル」と「直前にコミットしたファイル(HEAD)」のdiffが`./diff/test.tex`に出力されます。あとはこれをコンパイルすればPDFになります。

ただし大元のファイル名は`test.tex`だとしています。
`latexmk`を使っている場合、`.bbl`ファイルの名前は勝手に決まるはずです。

`test.tex`の中で`include`などをしていても引数は`test.tex`のみで大丈夫です。勝手に判断してくれます。

このコマンドは`diff`ディレクトリの中に結果を出します。
元々`diff`というディレクトリがある場合は注意してください。

自分用にスタイルファイルやクラスファイルを作っている場合、それらを読み込ませてあげる必要があります。
たとえば`./mystyle.sty`は`./diff/test.tex`からでは読めないので、`cd diff && ln -s ../mystyle.sty`などとシンボリックリンクを貼ってあげるなどすると良いと思います。

また、`.gitconfig`に次のように記述すると`git ldiff`でdiffのPDFが作れるようになります。`git diff HEAD~1`みたいにも使えます。

```
[alias]
        ldiff = !sh -c 'latexdiff-vc -e utf8 --git --flatten --force -d diff -r "${1:-HEAD}" test.tex test.bbl && cd diff && latexmk' -
```

ただしこのコマンドはレポジトリ依存なので、`~/.gitconfig`や`.git/config`に書くのではなく、レポジトリの中に`.gitconfig`ファイルを作り、`git config --local include.path ../.gitconfig`することでローカルの`.gitconfig`も認識してもらうようにするのが良いでしょう。

また、このコマンドは`latexmk`でコンパイルすることを前提としています。
違う方は書き換えて使ってください。
適切に`~/.latexmkrc`を設定していない場合、これだとPDFが出力されないかもしれません。
その場合は適宜`--pdf`などのオプションを足すか、`~/.latexmkrc`を書いて下さい。

## 解説

`latexdiff-vc`はバージョン管理システムの元で`latexdiff`を使うためのラッパです。
Git専用ではなく、SVNやMercurialでも使えます。
詳しくはmanページを見て下さい。

`latexdiff-vc`は

```
latexdiff-vc [ latexdiff-options ] [ latexdiff-vc-options ] -r [rev1] [-r rev2]  file1.tex [ file2.tex ...]
```

という形で使えます。元々のコマンドは

```
$ latexdiff-vc -e utf8 --git --flatten --force -d diff -r HEAD test.tex test.bbl
```

でした。以下オプションについて説明します。

* `-e utf8`: エンコーディングです。実はデフォルトがUTF-8なので無くてもいいです。これが唯一`latexdiff`に渡されるオプションです。
* `--git`: Gitで管理されていることを示します。これがない場合勝手に推論してくれるので実は無くてもいいです。
* `--flatten`: `\include`や`\input`しているものを全部展開して1ファイルとして扱うためのオプションです。これは`latexdiff-vc`のオプションです。順番を間違えると`latexdiff`のオプションだと解釈されるときがあるので注意。
* `--force`: diffのために作る`.tex`ファイルなどの上書きを強制します。
* `-d diff`: diffという名前のディレクトリの中に結果を出力します。これが無いとその場に結果を出力してしまい、`latexmk`しにくいです。
* `-r HEAD`: どのコミットと比較するか指定します。ここでは例として`HEAD`にしていますが、`HEAD~1`などにもできます。`.gitconfig`版ではここをオプション引数にしており、`git ldiff HEAD~2`などとすることで自由に選ぶことができます。

また、Gitのエイリアスに書いてあるびっくりマークは引数を処理するためのものです。[詳しくはこちら](https://git.wiki.kernel.org/index.php/Aliases#Advanced_aliases_with_arguments)。最後のハイフン(ダッシュ)は`sh`の引数で、引数を通常通り`$1`から始めさせるためにつけます。詳しくは`sh`のmanページの`-c`オプションの所に書いてあります。

## 応用

* どのコミットか示すために[Gitのタグ](https://git-scm.com/book/ja/Git-%E3%81%AE%E5%9F%BA%E6%9C%AC-%E3%82%BF%E3%82%B0)も使えるので、たとえば先生に提出する度にタグを付けておくと、提出の間に何回コミットしても大丈夫なので便利。
* rev2のオプションも指定することで、2つのコミット間のdiffもとれます。

## 参考

* 以下のコマンドのmanページ: latexdiff, latexdiff-vc, bibtex, sh(dash)
* [Git Aliases - git-scm.com](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)
* [Advanced aliases with arguments - Aliases - Git Wiki Homepage](https://git.wiki.kernel.org/index.php/Aliases#Advanced_aliases_with_arguments)
