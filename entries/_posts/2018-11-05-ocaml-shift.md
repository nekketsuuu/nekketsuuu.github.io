---
title: OCaml におけるシフト演算子の実装
issue: 10
published: true
last-modified: 2018-11-05
---

OCaml の [Pervasives][1] にはシフト演算子として `(lsl)`, `(lsr)`, `(asr)` があります。
ところで OCaml の整数は GC のために末尾の 1 bit を使っています。
そこで、シフト演算子も含め OCaml の算術演算子はこのビットを一旦退避させてから計算しています。
この記事ではその部分の実装をメモっておきます。

環境: OCaml 4.07.0

とりあえず Pervasives の[実装][2]を見てみると以下のように書かれています。

```ocaml
(* stdlib/pervasives.ml より引用 *)
external ( lsl ) : int -> int -> int = "%lslint"
external ( lsr ) : int -> int -> int = "%lsrint"
external ( asr ) : int -> int -> int = "%asrint"
```

`external` というのは[外部実装に任せますということです][3]。その外部実装には以下のように[書かれています][4]。マクロがありますが気分で読めます。

```c
/* runtime/interp.c より引用 */
Instruct(LSLINT):
  accu = (value)((((intnat) accu - 1) << Long_val(*sp++)) + 1); Next;
Instruct(LSRINT):
  accu = (value)((((uintnat) accu) >> Long_val(*sp++)) | 1); Next;
Instruct(ASRINT):
  accu = (value)((((intnat) accu) >> Long_val(*sp++)) | 1); Next;
```

※この記事は、大昔に書いたメモが出てきたので情報をアップデートして公開したものです。


  [1]: https://caml.inria.fr/pub/docs/manual-ocaml/libref/Pervasives.html
  [2]: https://github.com/ocaml/ocaml/blob/fd5c692ede5ebb4a6a0b630892201bdaff7844bc/stdlib/pervasives.ml#L66
  [3]: http://caml.inria.fr/pub/docs/manual-ocaml/intfc.html
  [4]: https://github.com/ocaml/ocaml/blob/d3e73595e55e84250fa77f04e9c239dee1224b7b/runtime/interp.c#L1002
