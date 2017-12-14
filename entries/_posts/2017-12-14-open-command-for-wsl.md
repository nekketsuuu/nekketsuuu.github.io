---
title: WSL で open コマンドが使いたい
issue: 4
published: true
last-modified: 2017-12-14
---

小ネタ。`cmd.exe` の `start` を使うと、`gnome-open` コマンドのようなものが作れます。

<!--more-->

以下のエイリアスを使います。`~/.bash_aliases` か `~/.bashrc` に追加すれば良いです。

```sh
alias open='cmd.exe /c start ""'
```

コマンドプロンプトの組み込みコマンドに [`start`](https://ss64.com/nt/start.html) というのがあり、これに単なるファイルを渡すと、ファイルの種類とソフトウェアの関連付けに基づいてファイルを開いてくれます。それを bash から呼び出しています。

便利ですね。

### 注意

* WSL のファイルシステム内にあるファイルは Windows 側のソフトウェアから見えないので、WSL 側のファイルは `open` できません。`/mnt/c/` 下など、Windows 側のファイルに対してのみ使用できます。WSL 側のファイルが Windows 側から開けちゃうと現状駄目なので、それはそうですね。
* Windows 10 build >= 14951 の WSL Ubuntu bash で使えます。古いバージョンの WSL だと使えません。
