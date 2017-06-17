---
layout: post
title: Python スクリプトからの SWF 制御
date: 2017-06-17 20:50 +0900
last_modified_at: 2017-06-18 07:30 +0900
---
Python スクリプトで WoT 画面に表示した SWF
の制御を行う方法についての解説です。

## 概要

Python スクリプトによって SWF を WoT の画面に表示する手順を
[WoT に SWF を貼り付ける](/2017/06/17/repl_add_flash.html)
で解説しました。
では、読み込んだあとの SWF の制御はどのようにするのでしょうか。


## SWF にパブリック関数を定義する

SWF の外部から制御を行うにはパブリック関数を SWF 内に定義します。

例えば
[Resize イベントの捕捉 - WoT mod のための SWF 作成 (3)](/2017/06/17/create_swf_catch_resized.html)
でいうと、
クラス `Main` に `public function` を定義します。
関数名は任意ですが、
WoT 内部では慣習的に `as_` で始まる関数名がつけられているようなので
合わせておきます。

```actionscript
public function as_test(param1:int):void
{
    shape1.x = shape1.x + param1;
}
```

見ての通り、引数 `param1` で渡した値をの分だけ `shpae1` をx軸方向に移動するコードです。


## Python スクリプトからの呼び出し

作成した SWF を
[WoT に SWF を貼り付ける](/2017/06/17/repl_add_flash.html)
と同様の手順で WoT クライアント画面に表示させます。

そして、コンソールから以下のように入力します。

```python
f.movie.root.as_test(100)
```

`f` は SWF を読んだ Flash クラスのインスタンスです。
そのプロパティ `movie.root` に
先程定義したパブリック関数 `as_test` がぶら下がっています。

予定通り中央の黄色の円が右に100ピクセル移動しました。

![SWF制御](/resources/image_20170617_17.png)


これで、Python スクリプトから SWF 内にある AS3 関数の呼び出し方法と、
データの渡し方の基本が理解できたと思います。

実際の WoT での各種 UI は数多くの制御用の外部関数が定義されているので、
もっと複雑な構成となっていますが、基本的な部分は変わりません。
