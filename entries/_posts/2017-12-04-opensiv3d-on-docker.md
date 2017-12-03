---
title: OpenSiv3D を Docker 上で動かす
issue: 3
published: true
last-modified: 2017-12-04
---

こんにちは、[熱血][25]です。これは [Siv3D Advent Calendar 2017][1]、4 日目の記事です。

<!--more-->

昨日は wynd2608 さんの [「OpenSiv3D Linux版の紹介」][2] でした。

注意: この記事では、OpenSiv3D でどうモノ作りするかではなく、OpenSiv3D が動く環境を整えるために Docker や Linux 上のソフトウェアをどう設定するかについて書いています。ご了承下さい。

## 目次
{:.no_toc}

* TOC
{:toc}

## OpenSiv3D と動作環境

OpenSiv3D は、[Siv3D][4] のマルチプラットフォーム版であり、Windows だけではなく macOS や Linux で動作するように開発が進められています。Linux を常用している僕でも Siv3D が使えるようになるわけです。やったー(ω)♪

ただし 2017 年 12 月現在、Linux 上で OpenSiv3D を使うためには色々と準備が必要です。たとえばいくつかのライブラリを事前にインストールする必要があります。画像や音などを処理するために外部のライブラリが必要なのです。また、OpenSiv3D をビルドするためには、C++17 が扱える C++ コンパイラをインストールしたり、[Boost C++ Libraries][6] の指定されたバージョンを用意したりする必要もあります。様々な Linux 環境に合わせてそれぞれにこれらを用意するのは、いささか大変です。

そこで、それぞれの環境差をなるべく吸収して OpenSiv3D を動かすことを目標に、Docker 上で OpenSiv3D を動かせるようにしてみました。以下の URL に置いてあります。この記事では、Docker 上で OpenSiv3D を動かすために必要となった知見を紹介したいと思います。

<https://github.com/nekketsuuu/opensiv3d-dockerfiles>

## Docker とは

そもそも [Docker][7] とは、皆さんが普段使っているメインの OS 環境とは独立した、仮想的な実行環境においてアプリケーションを実行させるためのソフトウェアです。Docker を使うと、ホスト環境から隔離された環境の上で、通常のプロセスを立ち上げるような形でアプリを実行することができるようになります。詳しくは @IT の連載[「超入門 Docker」][9]が分かりやすいです。

Docker が行いたいことは VMware や VirtualBox などのいわゆる「仮想環境 (VM)」と似ていますが、それらとは異なり、機能の多くをホストの OS カーネル側で共有することによって、ある種の "軽量化" を行っています。より詳しくは[この投稿][8]が参考になると思います。

今回作った Docker イメージを試してみたい場合、Docker のインストールが必要です。Docker の公式ホームページ <https://www.docker.com/> の "Get Docker" から、指示に従ってインストールしてください。ただし今回のイメージは Linux 向けに作成したため、Windows 上で動かすためにはハックが必要です。

また、僕自身はホスト OS として Ubuntu 17.04 しか試していないので、それ以外の OS で動作した場合はコメント頂けると嬉しいです :)

<div style="font-size: 0.8em;">※ Docker のインストール方法は時折変わるので、インターネットの記事を参考にする場合は注意してください。2017 年 12 月現在、Linux では Docker CE をインストールすれば良いです。また、BIOS の仮想化機能を有効にしていないと動かない場合があるので注意してください。</div>

## 実際に動かしてみる

今回作ったものは、Linux 環境下では以下のようにして動かすことができるはずです。ホストPC が Ubuntu 17.04 の場合動くことは確認しています。macOS 上では動かないかもしれません…… (ホスト側の設定を変えればいけると思いますが、それは本末転倒ですよね)。

```sh
git clone https://github.com/nekketsuuu/opensiv3d-dockerfiles.git
cd opensiv3d-dockerfiles
# 以下のコマンドは内部で sudo を呼び出すので、念の為シェルスクリプトの中身を確認しておいて下さい。
./sudo-docker-run.sh 0.1.7.1900-Debug
```

コンテナの上でアプリケーションをビルドして動かす方法については、[レポジトリ][20]の Quickstart などをご覧ください。

## OpenSiv3D に必要なもの

今回はコンテナとして `ubuntu:16.04` の上で OpenSiv3D を動かすことにしました。さて、OpenSiv3D を Ubuntu 16.04 の上で動かすために必要なものは、以下の通りです。このことは OpenSiv3D レポジトリの[このファイル][10]に書かれていることと同じです。

*  ビルドシステム
    * CMake >=2.8
    * C++17 が扱えるコンパイラ。Clang >=5 や GCC >=7.2.0 など。
    * Boost C++ Library >=1.65.0 (2017年12月現在; 最新情報は `OpenSiv3D/Dependencies/README.md` に書かれています)
* マルチメディアのためのライブラリ
    * GLib >=2 …… GNOME のために必要
    * OpenGL, GLU, GLEW
        * OpenGL >=4.1 が扱える環境も必要
    * 画像のためのライブラリ: libpng, libturbojpeg
    * X11 のライブラリたち
    * FreeType >=2.8 …… フォントレンダリングに必要
    * OpenAL …… 音の再生に必要
        * また、OpenAL のバックエンドとなる音声デバイス管理ソフトウェアも必要になります。ALSA や PulseAudio のことです。

Docker の上で OpenSiv3D のプロジェクトを動かすためには、

1. Docker イメージ上にこれらのライブラリをインストールする。
2. OpenSiv3D をビルドして、OpenSiv3D のライブラリを作る。
3. OpenSiv3D のライブラリを利用して、自分のプロジェクトをビルドする。
4. 実行ファイルを動かす。もし Docker コンテナの上で実行する場合は、ホスト PC の持つ各種デバイスへのアクセス権を与えておく必要がある。

という流れになります。

今回メインで解説したいのは手順 4 の部分です。そこを解説する前にちょっと寄り道をして、手順 1 についてメモを残しておきます。手順 4 の理解には必要ない情報なので、読み飛ばして下さい。

### ビルド環境を整える

CMake は `apt install` すればよいです。

Ubuntu 16.04 のデフォルトの `gcc` は C++17 が扱えるほど新しいものではないので、新しい `clang` か `gcc` を自分でビルドするか、適当な PPA からインストールしてくる必要があります。PPA の場合、たとえば [PPA for Ubuntu Toolchain Uploads][11] には `gcc-7` が含まれています。

Boost は、公式サイトの情報を頼りに、今回用いるコンパイラに合わせてビルドする必要があります。複数バージョンの boost ライブラリを使う場合はインストールディレクトリに気をつけましょう。僕は今回[こうやってインストールしました][12]。ググると情報が色々あるので、参考になると思います。

### ライブラリのインストール

ほとんどのライブラリは単に `apt install` すればインストールできるのですが、いくつか例外があります (新しいバージョンの Ubuntu だと全て `apt install` で済むはずです)。

* Ubuntu 16.04 にデフォルトでインストールされる libturbojpeg には、`pkgconfig` 用のファイルが用意されないため `cmake` から使えないという[既知のバグがあります][13] (新しいバージョンのパッケージでは修正されているようです)。僕は自前で設定ファイルを[用意しました][24]。
* Ubuntu 16.04 にデフォルトでインストールされる FreeType はバージョンが古く、現在の OpenSiv3D が使っている API と合致しないため、新しいものを自前でビルドしてインストールする必要があります。

## Docker コンテナからホストのデバイスを触りたい！

OpenSiv3D のライブラリが出来て、自分のプロジェクトがビルドできたとしても、Docker コンテナの上からその実行ファイルを動かすためにはまだ壁があります。というのも、デフォルトでは Docker コンテナからホストのデバイスを叩くことができないため、個別に設定して使えるようにしてあげる必要があるからです。

具体的には、以下の部分について設定が必要です。

* Docker コンテナから GUI のウィンドウを出したい！ → ディスプレイサーバーを触れるようにしてあげる。
* Docker コンテナから OpenGL でグラフィックカードを触りたい！ → 対象のデバイスを触れるようにしてあげる。
* Docker コンテナから音を出したい！ → 複数の選択肢があります。①音声デバイスを直接触れるようにしてあげる。②音声デバイスを管理しているソフトウェアと通信できるようにしてあげる (ALSA? PulseAudio? JACK?)

以下、これらについて詳しく説明していきます。

## グラフィカルなウィンドウを出したい！

OpenSiv3D for Linux では、現状ディスプレイシステムとしてひとまずは X11 をサポートしています。X11 をコンテナの中から利用するためには、ホスト側で動いている X11 のサーバーと通信すれば良いです。X11 のサーバー自体はホスト PC で動いているので、Docker コンテナからサーバーへ繋げるようにする必要があります。

ホスト OS も Ubuntu の場合、このためのよく知られたハックがあります。

* 環境変数 `DISPLAY` をホスト PC と共有する。
    * POSIX 環境において、X11 は `$DISPLAY` に `hostname:displaynumber.screennumber` の形式でディスプレイ名を保存しています。Docker コンテナ上の Ubuntu はこの環境変数を知らないので、教えてあげる必要があります。
* `/tmp/.X11-unix` をホスト PC と共有する。
    * このフォルダの中に、X11 のサーバーと通信するためのソケットがあります。
* Docker からサーバーへのアクセスを、ホスト側で許可する。
    * 具体的には `xhost` コマンドを使って許可することになります。セキュリティを気にする必要が無い場合、単に `xhost +` とすれば全てのクライアントからのアクセスを許可することになります。普通はセキュリティを気にしたいと思いますが、その場合ローカルからのアクセスを全て許可する `xhost +local:` が使えます。ローカルも信用ならない場合、たとえば Docker コンテナの `.Config.Hostname` が分かっていれば、`xhost +local:ホストネーム` で個別に許可が出せます。
    * 許可を取り消すには `+` を `-` に変えて実行すれば良いです。`xhost -` みたく。
    * よって今回は次のようにして動かしています。
      {% raw %}```sh
      docker run --detach -it --name example_container ubuntu:16.04 /bin/bash
      DOCKER_HOSTNAME="$(docker inspect --format='{{ .Config.Hostname }}' example_container)"
      xhost +"local:${DOCKER_HOSTNAME}"
      docker attach example_container
      xhost -"local:${DOCKER_HOSTNAME}"
      ```{% endraw %}

このあたりのことは、ROS.org のチュートリアルに[まとめられています][14]。マニュアルとしては [XSERVER のマニュアル][15] や X そのもののマニュアルなどに書いてあります。

## OpenGL でグラフィックカードを触りたい！

Intel 製のグラフィックカードがついているコンピュータの場合、以下のようにすると動きました。

* デバイス `/dev/dri` を使えるようにする。つまり、`docker run` 時に `--device /dev/dri` オプションをつける。
* ユーザーグループ `video` にコンテナ内のユーザーを追加する。つまり、`docker run` 時に `--group-add $(getent group video | cut -d: -f3)` オプションをつける。

このあたりのことは ROS.org のチュートリアルに[まとめられています][16]。

Intel 製以外のハードウェアがついている場合については、よく知りません……。

なお、OpenSiv3D を動かすためには OpenGL のバージョン 4.1 以上に対応したデバイスドライバが必要です。自分の環境が対応しているかどうかは、`glxinfo` コマンドを使うことで[確認することができます][17]。僕の環境では以下のようにして確認できました。

```console
$ glxinfo | grep version
server glx version string: 1.4
client glx version string: 1.4
GLX version: 1.4
    Max core profile version: 4.5
    Max compat profile version: 3.0
    Max GLES1 profile version: 1.1
    Max GLES[23] profile version: 3.2
OpenGL core profile version string: 4.5 (Core Profile) Mesa 17.4.0-devel
OpenGL core profile shading language version string: 4.50
OpenGL version string: 3.0 Mesa 17.4.0-devel
OpenGL shading language version string: 1.30
OpenGL ES profile version string: OpenGL ES 3.2 Mesa 17.4.0-devel
OpenGL ES profile shading language version string: OpenGL ES GLSL ES 3.20
```

また、理由は不明ですが、現段階の OpenSiv3D を動かすためには、`glewExperimental` を `GL_TRUE` にするパッチを当てる必要がありました ([パッチ][22])。ホスト側のパソコンではこのパッチを当てなくても OpenSiv3D で作ったアプリが動いたので、何かしら Docker 特有の制限が効いているのだと思います。(このあたりのことを解決する際、[@Reputeless さん][23]に助けて頂きました。ありがとうございました。)

## 音を出したい！

個人的に一番苦労したのが音声デバイスまわりでした……。

そもそも僕自身、Ubuntu の上で動いているサウンドシステムについてよく理解していませんでした。Ubuntu 上で音が鳴る際には、ALSA や PulseAudio など複数のソフトウェアが協調して動いています。このあたりを理解するためには、以下の記事が参考になりました。

* [サウンドシステムの使いこなし][18] -- Ubuntu Weekly Recipe (第177回)
* [UbuntuStudioTips/Setup/UbuntuSoundSystem][19] -- Ubuntu Japanese Team Wiki

要するに標準の Ubuntu では、サウンドデバイスを叩くための部分で ALSA というのが動いており、さらに ALSA を叩くための部分で PulseAudio が動いています。PulseAudio のようなソフトは、複数のアプリケーションが同時に音を流そうとしたときに音声のミックスが必要になるので存在します。

OpenSiv3D (Linux) が音声を流すために使っている OpenAL ライブラリも、実際に音声を流す際はこのようなサウンドシステムを利用しています。なのでこれを Docker コンテナから使う場合、ホスト側の ALSA や PulseAudio がコンテナから使えないといけません。幸いなことに PulseAudio には、PulseAudio サーバーに接続すれば外部からでも音声を再生できるという仕組みがあるので、今回はこれを利用します。

今回の場合、次のようにすると動きました。大抵のことは [PulseAudio FAQ][21] に書いてありました。

* Docker イメージに pulseaudio をインストールしておく。
* 環境変数 `PULSE_SERVER` を設定する。
* ユーザーグループ `audio` にコンテナ内のユーザーを追加する。
* PulseAudio 用のソケットが見えるように共有する。`${XDG_RUNTIME_DIR}/pulse/native` がそれで、僕の環境だと `/run/user/1000/pulse/native` でした。
* PulseAudio サーバー接続の認証のために使う「クッキー」と呼ばれるファイルを共有する。`~/.config/pulse/cookie` にあります。

最終的にどういうオプションが必要になったかは、レポジトリの `docker_run.sh` などをご覧ください。 <https://github.com/nekketsuuu/opensiv3d-dockerfiles>

ネット上の記事だとデバイス `/dev/snd` を使えるようにしろ、と書いてあることもありますが、今回は ALSA を叩くのではなく PulseAudio サーバーが見えれば充分なので、これは必要ありませんでした。なお、PulseAudio を通さず直接 ALSA を叩いて音を鳴らすようにすることも可能なのですが、こうするとミキサー不在のため同時に1つの音声しか流せなくなります。"device or resource busy" とエラーが出てきたらこれです。

動作確認には、デフォルトサウンドデバイスが確認できる `aplay -L` や、PulseAudio を通じて音声ファイルを再生する `paplay /usr/share/sounds/alsa/Front_Center.wav` が便利でした。

なおこの方法は、ホスト側のサウンドシステムが PulseAudio を使っていない場合、意味がありません。

## OpenSiv3D on Docker の難点

ここまでで見てきたように、OpenSiv3D for Linux 自体をコンテナ上でビルドしたり、OpenSiv3D を使ったプロジェクトをコンテナ上でビルドしたり、その結果できた実行ファイルをコンテナ上で実行することはできます。

ただし、はじめ考えていたような「OpenSiv3D を使ったプロジェクトがポータブルに動かせるようにしたい」という目標は、あまり達成できていません。グラフィカルなアプリケーションを実行したいため、コンテナ上でアプリを実行するためにはどうしてもホスト側のデバイスを弄ったり、サウンドシステムを利用したりする必要があるためです。更に言うと今後 Leap Motion などのデバイスも弄りたくなることでしょう。こうなってくるとコンテナを走らせるためのオプション部分がホスト側の環境依存となるため、Docker を使う旨みが小さくなってしまいました。

環境ごとの差異を打ち消しつつ OpenSiv3D を試せるようなものが作れるようにすることは、future work です。

## OpenSiv3D で遊ぼう！

さて、長くなってしまいましたが、これでようやく僕の Docker 環境でも Siv3D が使えるようになりました。

ここまで書いておいてアレゲですが、安定動作することを目標にするのであればホスト環境にライブラリを揃える方がラクそうです。グラフィックや音のインターフェースを共通化するのは大変なんだなあという気分でした。今回の Docker イメージはミニマルな環境においても OpenSiv3D がビルドできるのか、などといった特殊な状況において活用できそうだと思っています。毎回クリーンな環境を用意するのは大変ですしね。

僕自身はまだあまり Siv3D を使ったプログラミングをしておらず、これから発展的な内容に入門するところです。明日以降のアドベント・カレンダーの記事には作品紹介もあると思うので、しっかり参考にしていきたいです :-)

というわけで、[Siv3D Advent Calendar 2017][1]、4 日目の記事でした(ω) 明日は Reputeless さんの記事です。


   [1]: https://qiita.com/advent-calendar/2017/siv3d
   [2]: https://qiita.com/wynd2608/items/9299f57a337578d5161c
   [3]: http://example.com
   [4]: http://play-siv3d.hateblo.jp/
   [5]: https://www.ubuntulinux.jp/
   [6]: http://www.boost.org/
   [7]: https://www.docker.com/
   [8]: https://stackoverflow.com/q/16047306/5989200
   [9]: http://www.atmarkit.co.jp/ait/articles/1701/30/news037.html
   [10]: https://github.com/wynd2608/OpenSiv3D/tree/a565ce51adff1b7aaf8cdb67fa35af21edf36ec8/Linux
   [11]: https://launchpad.net/~ubuntu-toolchain-r/+archive/ubuntu/test
   [12]: https://github.com/nekketsuuu/boost-dockerfiles/blob/32596abd939509905e8ec60a5c96d829c0814eb2/docker/Dockerfile
   [13]: https://bugs.launchpad.net/ubuntu/+source/libjpeg-turbo/+bug/1056566
   [14]: http://wiki.ros.org/docker/Tutorials/GUI
   [15]: https://www.x.org/archive/current/doc/man/man1/Xserver.1.xhtml
   [16]: http://wiki.ros.org/docker/Tutorials/Hardware%20Acceleration
   [17]: https://askubuntu.com/q/47062/644471
   [18]: http://gihyo.jp/admin/serial/01/ubuntu-recipe/0177
   [19]: https://wiki.ubuntulinux.jp/UbuntuStudioTips/Setup/UbuntuSoundSystem
   [20]: https://github.com/nekketsuuu/opensiv3d-dockerfiles
   [21]: https://www.freedesktop.org/wiki/Software/PulseAudio/FAQ/
   [22]: https://github.com/nekketsuuu/opensiv3d-dockerfiles/blob/e099f084eb4e4426f7d4faa8c5e64145bfb1b3ee/docker/Dockerfile#L83
   [23]: http://reputeless.hatenablog.jp/
   [24]: https://github.com/nekketsuuu/opensiv3d-dockerfiles/blob/e099f084eb4e4426f7d4faa8c5e64145bfb1b3ee/docker/Dockerfile#L47
   [25]: https://twitter.com/nekketsuuu
