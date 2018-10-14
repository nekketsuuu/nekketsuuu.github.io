---
title: opam2.0 のために Ubuntu へ bwrap をインストールする
issue: 8
published: true
last-modified: 2018-10-09
---

opam2.0 をインストールしようとすると「できれば [bwrap](https://github.com/projectatomic/bubblewrap) をインストールしてください」的な警告メッセージが出てくることがあります。警告メッセージと [OPAM の FAQ](https://opam.ocaml.org/doc/FAQ.html#Why-does-opam-require-bwrap) に書かれているとおり、これは opam2.0 から導入されたサンドボックス機能のために必要とされます。

```
[WARNING] Missing dependencies -- the following commands are required for opam to operate:
  - bwrap: Sandboxing tool bwrap was not found. You should install 'bubblewrap'. See
    http://opam.ocaml.org/doc/2.0/FAQ.html#Why-opam-asks-me-to-install-bwrap.
```

Ubuntu 18.04 に bwrap をインストールするには [bubblewrap](https://packages.ubuntu.com/bionic/bubblewrap) をインストールすれば OK です。

```sh
sudo apt install bubblewrap
```

古い Ubuntu だと APT リポジトリに bubblewrap がありません。特に、Ubuntu 16.04 にはありません。この問題は opam の [Issue #3424](https://github.com/ocaml/opam/issues/3424) で議論されていますが、現状自分でなるべく新しい bubblewrap (少なくとも 0.1.7 以上) をビルドするのが一番簡単そうです。(一応サンドボックス機能を無効化すれば bwrap を使わないのでひとまずは大丈夫になるのですが……。)
