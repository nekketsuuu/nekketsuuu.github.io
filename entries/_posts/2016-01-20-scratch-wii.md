---
title: WiiリモコンをScratch 2.0で使う
imgpath: /assets/entries/2016-01-20-scratch-wii/
issue: 14
published: true
last-modified: 2019-08-31
---

**\# この記事は、2016年1月20日にYahoo!ブログへ投稿した記事を、Yahoo!ブログの終了に伴って2019年8月に移行したものです。**

この記事では、Wiiリモコン(Wiimote)をBluetoothでパソコンに接続し、cwiidというソフトウェアを介してキーボードやマウスと対応づけることでScratchで使うことを解説します。

以下は、**Ubuntu 14.04 LTS** で動作確認しました。確認していませんが他のLinux環境でも同様に可能だと思います。Windows環境でも、ここで用いるソフトと同様のことができるソフトを使えば可能です(最後にリンクします)。Mac OS環境ではよく知りません。

----

まず、WiiリモコンをパソコンにBluetooth接続できるか確認します。

パソコンにBluetoothがついていない場合、[Bluetooth USBアダプタ](https://www.google.co.jp/search?q=usb+bluetooth)を使用すると良いです。僕のパソコンには標準でついていたので使用していません。

![]({{ site.baseurl }}{{ page.imgpath }}img_0.png)

Ubuntu側でBluetoothの待ち受けをした後、Wiiリモコンの①ボタンと②ボタンを同時押しするか、電池の横にある赤いSYNCボタンを押すことでBluetoothの登録をします。

後は待っていれば自動的に成功します。とはいえ繋がるだけなのでこのままでは何もできません。電池がもったいないので一旦接続をオフにします。後で面倒くさいので、Ubuntu側で認識できることが分かれば\[Bluetoothの設定\]からデバイスを削除しておいても良いかもしれません。

----

今回はBluetooth接続したWiiリモコンをジョイスティックとして使うために、CWiiDというソフトを使います。

<https://help.ubuntu.com/community/CWiiD>

libcwiid1が共通ライブラリ、lswmがWiiリモコンのアドレスを出力するソフト、wmguiがGUIでWiiリモコンの接続を確認するソフト、wminputが今回メインで使う、Wiiリモコンの入力をマウスやキーボード入力にマップするソフトです。

まずはインストールします。とりあえず使うだけならaptのパッケージで大丈夫です。pullされていない拡張機能を試したい方はgithubからcloneしてコンパイルする必要があります(後述)。

```shell-session
$ sudo apt-get install libcwiid1 lswm wmgui wminput
```

更にマウスを動かす機能を使うために、カーネルにuinputモジュールをロードしておきます。

```shell-session
$ sudo modprobe uinput
```

この操作はUbuntuを再起動するたびに必要になるはずです。かわりに/etc/modulesに直接書き込んでおく方法もあります。

```shell-session
$ gksudo gedit /etc/modules
$ cat /etc/modules
# /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.

lp
uinput
$
```

さてそれでは、動くかどうかwmguiで確かめてみます。

```shell-session
$ wmgui
```

起動したら、メニューバーから\[File\]->\[Connect\]を押したあと、Wiiリモコンを待ち受け状態にして(①+②またはSYNC)、\[OK\]を押すとWiiリモコンが認識されます。

![]({{ site.baseurl }}{{ page.imgpath }}img_1.png)

この画面の上で、きちんとWiiリモコンの入力が認識されているか確認できます。標準ではボタン入力だけチェックされていますが、メニューバーの\[Settings\]で選択することによって、加速度センサー、赤外線、ヌンチャク、クラシックコントローラの入力を受け取ることができるようになります。また、\[Controls\]から選択することによってWiiリモコンのLEDを光らせたり、Wiiリモコンをバイブさせたりすることができます。

----

動作確認ができたら、今度はwminputを使ってみます。wminputはWiiリモコンの入力をマウスの移動やクリック、キーボード入力などに変換できるソフトです。

```shell-session
$ lswm
Put Wiimotes in discoverable mode now (press 1+2)...
00:1F:32:95:EF:B0                   # このアドレスはWiiリモコンによって違うはずです
$ sudo wminput 00:1F:32:95:EF:B0
```

まずlswmを使ってWiiリモコン固有のアドレスを取得しておきます。これは必須ではないですが、こうしておくとwminputを使うときに接続開始を速くできます。

そしてwminputにこのアドレスを渡して動かします。デフォルトの設定では、Wiiリモコンを傾けるとマウスが動き、Aボタンで左クリック、Bボタンで右クリック、A+Bで中クリック、十字キーが矢印キーなどに対応させています。

このキーマップはコンフィグファイルを使って変更することができます。wminputの-cオプションを使うとできます。コンフィグファイルのサンプルは/usr/local/lib/cwiid/wminput/などにあります。

```shell-session
$ sudo wminput -c ir_ptr
```

みたいにすると使えます。詳しくはwminputのREADMEを読んで下さい。

赤外線センサも上みたいにコンフィグを変更することで使用できます。Wiiに付属のセンサーバーを起動してもいいですが、CWiiDのREADMEによると、赤外線LED、ろうそく、白熱電球などで代用できるようです。センサーバーからはたくさん赤外線が出ていますが、基本的には左右2つあればよいらしく、もっと言うと1つでも大丈夫だそうです。(まだ試していません)

またCWiiDにはプラグインの機能があります。マウス移動の機能はデフォルトで入っているプラグインを使って実装されています。プラグインはC/Pythonで書くことができるのでユーザーがいくつか自作プラグインを開発してpull reqを送っています。完成しているがmergeされていないものも多いですが、[GitHubのレポジトリを確認しつつ](https://github.com/abstrakraft/cwiid/pulls)自分でmergeしてコンパイルしてやると使うことができます。Wiiリモコンの傾きを捻りだと思ってキーボード入力にするScrewdriverとか便利じゃないですかね? 自分でプラグインを書くこともできるので自由度高そうです。

aptで配布されているパッケージはちょっと古くて、ソースコードからコンパイルするとwmguiでモーションプラスの値を取れるようになっていたりします。

ソースコードからコンパイルすることについては、[こっちの記事を参考](/entries/2016/01/21/build-cwiid.html)にしてください。

これ以上の詳細は[CWiiDのコミュニティページ](https://help.ubuntu.com/community/CWiiD)や、[wminputのREADME](https://github.com/abstrakraft/cwiid/tree/master/wminput)を参照してください。

----

wminputのデフォルト設定を使ってWiiリモコンを接続したとして、Wiiリモコンで遊べるサンプルゲームを作ってみました。

<https://scratch.mit.edu/projects/94715425/>

多少他の人に遊んでもらった感想です:

・このゲームの操作にはボタンは要らないので、変な所でボタン操作を要求しない方が良さそう
・傾ける動作と赤外線でポイントする動作だと、子どもには後者の方が直感的っぽい?
・マウスカーソルをわざと大きくして画面から離れてプレイできるようにしないとあんまりWiiっぽくない
・ブラウザを全画面表示(F11)にした上でScratchも大画面モードでプレイした方がそれっぽい
・普通のマウス入力はオフにしておかないと、結局そっちの方に流れる(これはゲーム設計が悪い; Wiiリモコンだからこそできるゲームにすべき)

----

**※ Windows使いの方向けの補足 ※**

CWiiDはLinux用です。Windowsでするならもっと簡単に、一般のBluetoothジョイスティックを入力として扱うソフトウェアがあるのでそれが使えます。

[この記事](http://logiclab.blog.jp/archives/52070140.html)が詳しく解説されています。ソフトウェアとしてGlovePIEや[Wey](http://www.geocities.jp/takomiku39hp/wiimote.html)を紹介されていて、同様の方法でWiiリモコンだけでなく「太鼓の達人 for Wii」のタタコンなどが使用できることが示されています。「Wiimote software」などで調べると色々出てきますね。

(**2016年1月20日16時現在**、GlovePIEの公式ホームページはハックされてしまっているように見えます。復旧まで下手にアクセスしない方が良さそうです)

また、Scratch 1.4のときみたいにプラグインとしてWiiリモコンを使いたいという欲求もあるでしょう。たとえばWiiリモコンの加速度センサーの値をそのままScratchで使いたくなるときもあります。

Scratch 2.0では拡張機能をScratchXとして実装しようとしている(?)ので、ScratchXを使用するならこれを実現することができます。たとえば[iiConnect2Scratch](http://www.creativecomputerlab.com/iiConnect2Scratch.html)というのがBluetooth接続方法から、オリジナルソフトウェアの使い方、Scratchでのサンプルプロジェクトまでの流れが全部分かりやすく説明されていて良いと思いました。特に[このユーザーガイド](http://www.creativecomputerlab.com/extensions/iiConnect2Scratch.pdf)は分かりやすかったです。iiConnect2ScratchではWiiリモコンだけでなくWiiFitボードも使えるようです。面白そう。

----

参考

* 独学Linux
    * [WiiリモコンでBerylを動かす方法《実践編No.1》](http://blog.livedoor.jp/vine_user/archives/51038020.html)
    * [WiiリモコンでBerylを動かす方法《実践編No2》](http://blog.livedoor.jp/vine_user/archives/51039335.html)
    * [WiiリモコンでBerylを動かす方法《実践編No.3》](http://blog.livedoor.jp/vine_user/archives/51050214.html)
* [CwiiD - help.ubuntu.com](https://help.ubuntu.com/community/CWiiD)
* [cwiid - GitHub](https://github.com/abstrakraft/cwiid)
    * 特に、[wminputのREADME](https://github.com/abstrakraft/cwiid/tree/master/wminput)
* ロジックラボ for kids
    * [ScratchをWiiリモコンで動かす-導入編-](http://logiclab.blog.jp/archives/52070140.html)
* [iiConnect2Scratch](http://www.creativecomputerlab.com/iiConnect2Scratch.html)

ScratchXについて、開発者向けにはここが詳しいです: [ScratchX - GitHub](https://github.com/LLK/scratchx/wiki)。

CWiiDの他にXWiimoteというのもあります。こちらの方が設計が良く使いやすいと言っていた人もいるけれどまだ僕は試していません: [XWiimote - Ponyhof](https://dvdhrm.wordpress.com/xwiimote/)。
