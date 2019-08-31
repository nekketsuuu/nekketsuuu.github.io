---
title: CWiiDをUbuntu 14.04にインストールする
issue: 15
published: true
last-modified: 2019-08-31
---

**\# この記事は、2016年1月21日にYahoo!ブログへ投稿した記事を、Yahoo!ブログの終了に伴って2019年8月に移行したものです。**

CWiiDをUbuntu 14.04 LTSでコンパイルして、wmgui, lswm, wminputを使えるようにする。

<!--more-->

[Scratchで使いたかった](/entries/2016/01/20/scratch-wii.html)という話の流れですがこの記事にScratchは出てきません。あと、単に使うだけなら`apt-get install`できます。

## 1. git clone

オリジナルのレポジトリは[abstrakraft/cwiid](https://github.com/abstrakraft/cwiid)なのだが、これの開発は2010年から止まっている。開発者のabstrakraftさんは「もっと良いAPIとしてWiiリモコン用のカーネルドライバができたのでcwiidを開発する必要が無くなった」と[言っている](https://github.com/abstrakraft/cwiid/issues/20#issuecomment-34212643)。

そこで[このissue](https://github.com/abstrakraft/cwiid/issues/20)を参考にしつつ、たとえば[mzimmerman/cwiid](https://github.com/mzimmerman/cwiid)というforkを`git clone`すると良いと思う。オリジナルに寄せられたpull-reqをいくつか取り入れていたりするブランチなのでわざわざ自分でmergeしなくて済む。

以下mzimmerman/cwiidかオリジナルを`git clone`したとして話す。

## 2. 事前に必要なものをインストール

目ぼしいところだとpython2.xやlibbluetooth-dev(bluez-libs)が必要です。僕の環境ではpython2.7で動作確認しました。

```shell-session
$ sudo apt-get install bison flex libgtk2.0-dev autotools-dev automake libbluetooth-dev
```

[フランス語のこのサイト](http://wiki.labomedia.org/index.php/Blender:Wiimote_:_Compilation_de_cwiid_sur_Linux_Mint_12)によると、以下のものが必要らしい(環境はLinux Mint 12)が、本当かなあ。pythonのバージョンを見る限り情報が古そうな気もする。

```shell-session
$ sudo apt-get install bison flex libbluetooth-dev libgtk2.0-dev python-dev libtool automake1.10 autoconf quilt patchutils python-all-dev cdbs lesstif2 poppler-data
$ sudo apt-get install autogen automake gcc bluetooth libbluetooth3-dev libgtk2.0-dev pkg-config python2.6-dev flex bison git-core libbluetooth-dev python-pygame python-tk
```

あと、wminputを動かすためにカーネルにuinputモジュールを入れておく必要がある。これについて詳しいことは、wminputのREADMEを参照。

```shell-session
$ sudo modprobe uinput  # 再起動のたびに実行
```

## 3. makeしてインストール

コンフィグしてmakeする。

オリジナルのレポジトリからgit cloneしたなら、Makefileにバグがあるので修正する必要がある。wmdemo/Makefile.inの11行目を以下のように追記修正。

```shell-session
LDLIBS += -lcwiid -lbluetooth
```

そしてmake

```shell-session
$ cd cwiid
$ aclocal
$ autoconf
$ ./configure --with-cwiid-config-dir=/etc/cwiid/
$ make
$ sudo make install
```

configureが「wmdemo.oに未定義のシンボルstr2baがあんねん」みたいなエラーで止まった場合は上の修正を忘れている。wmdemo/Makefile.inを修正し、`make clean`した後に上の動作をもう一度すると良い。

## 4. 確認

wmgui, lswm, wminputが動くか確認してみる。またはwmdemoを動かしてみても良い。

```shell-session
$ which wmgui
/usr/local/bin/wmgui
$ wmgui
```

僕の環境+オリジナルのcwiidでは、オリジナルのWiiリモコン、Wiiリモコン+ヌンチャク、Wiiリモコン+モーションプラスがちゃんと認識できた。クラシックコントローラは持っていないので分からない。また、Wiiリモコン+モーションプラス+ヌンチャクだと上手く認識できなかった。また、wmgui起動中にヌンチャクとモーションプラスを切り替えてもプラグアンドプレイ的な認識をしてくれなかった。

mzimmerman/cwiidでも同じような認識だった。ただこっちにはさらにguitarというのが増えていた。"Guitar Hero"や"Rock Band"というゲームで使用されている[コレのこと](http://www.amazon.com/Wireless-Guitar-Wii-Games-Color-Nintendo/dp/B004QSY97W)だろうか。持っていないので確認できなかった。

lswmとwminputも普通に動いた。mzimmerman/cwiidならsudoは要らないようだ。

```shell-session
$ lswm
Put Wiimotes in discoverable mode now (press 1+2)...
00:1F:32:95:EF:B0                   # このアドレスはWiiリモコンによって違うはず
$ wminput                           # オリジナルならsudo wminputじゃないと駄目な気がする
```

もし"ImportError: No module named cwiid"というpythonのエラーっぽいもので止まったら、以下のことをすると良いだろう。僕はこれで動いた。

a. PYTHONPATHにpython2.X/site-packagesを追加する。

```shell-session
$ export $PYTHONPATH="/usr/local/lib/python2.7/site-packages"
```

元から`PYTHONPATH`に何か入っている方は上書きして消さないように注意。このスクリプトは`.bashrc`などに書いておくと便利。ちゃんとパスが通ったかどうかは、たとえば`sys.path`を見ると確認できる。

```shell-session
$ python2.7
>>> import sys
>>> sys.path
```

それでも止まるときは、他のモジュールが干渉しているかもしれない。僕はstraceして変なところから呼んでないか調べた。

```shell-session
$ sudo strace wminput 2> log
$ emacs log
```

b. sudo wminputがうまくいかないとき、これはsudoだとPYTHONPATHが使われないことに起因している。sudoersのenv_keepにPYTHONPATHを追加すると動く。[このStackOverflow](http://stackoverflow.com/questions/7969540/pythonpath-not-working-for-sudo-on-gnu-linux-works-for-root)や、[このブログ](https://osxastrotricks.wordpress.com/2012/02/07/problems-with-sudo-python/)を参考にした。

sudoersは不用意に弄ると死ぬので気をつける。/etc/sudoers.d/やvisudoを使うと安全?

c. ディストリビューションによっては、/usr/local/libを見に行かないこともあるらしい(Ubuntu 14.04だと大丈夫だった)。cwiidのREADMEに解決策が出ているので参考にすると良いかもしれない。また、`./configure`に`-libdir`を指定しないといけないディストリビューションもあるらしい。[こちらを参照](http://abstrakraft.org/cwiid/discussion/topic/65)。

## 5. キーコンフィグやプラグイン

wminputは-cオプションを使うとキーコンフィグファイルを変更できる。`/etc/cwiid/wminput/`や`/usr/local/etc/cwiid/wminput/`にサンプルのコンフィグファイルがいくつかあるので、たとえば

```shell-session
$ ls /usr/local/etc/cwiid/wminput/  # オリジナル版の場合
acc_led  buttons  gamepad  neverball        nunchuk_stick2btn
acc_ptr  default  ir_ptr   nunchuk_acc_ptr
$ sudo wminput -c ir_ptr
```

とするとir_ptrを使用できる。コンフィグファイルの文法についてはwminputのREADME参照。

自分でプラグインを書くこともできる。他のプラグインを参考にしながらCで書いてmakeし直すとできるはず。

## 6. その他

やってるとCWiiDのインストール/アンインストールを繰り返すことがある。そのときは

```shell-session
$ cd cwiid
$ sudo make uninstall && sudo make uninstall_config
$ sudo make clean
```

すると綺麗に諸々削除してくれる。make cleanまで必要なのかどうか知らないが、不安なのでとりあえずしている。`make uninstall`についてはcwiidのREADMEに書いてあった。

ちゃんとmake uninstallできていることは

```shell-session
$ sudo updatedb
$ locate cwiid
```

とかすると分かる。
