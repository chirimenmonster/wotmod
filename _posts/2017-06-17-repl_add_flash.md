---
layout: post
title: WoT に SWF を貼り付ける
date: 2017-06-17 11:05 +0900
last_modified_at: 2017-06-18 06:50 +0900
---
WoT の画面に SWF を貼り付けてみます。
[wot-debugserver を使ったデバッグ](/2017/06/17/wot-debugserver.html)
で解説した REPL (対話型評価環境) を使います。


## WoT 上での Flash の扱い

WoT のエンジンである BigWorld の関数 GUI.Flash()
で Flash ムービーを再生することは可能ですが、
もっと簡便に Flash を利用できるクラス Flash が
`scripts/client/gui/Scaleform/Flash.py`
で定義されているので通常はこちらか、その派生クラスを使います。

## wot-debugserver による Flash オブジェクトの貼付け

wot-debugserver を使えば
リプレイ画面などに　Flash オブジェクトを貼り付けることができます。

### SWF ファイルの設置

あらかじめ、`res_mods/<WOT_VERSION>` に `Test.swf` を置いておきます。
この `test.swf` は FlashDevelop を使って作成した黒背景に
黄色の円を描画する SWF で
[単純な図形の作成 - WoT mod のための SWF 作成 (2)](/2017/06/17/create_swf_simple.html) で作成したものです。

### Flash クラスのインスタンス作成と有効化

適当なリプレイファイルを再生して適当なところで止めた後、
wot-debugserver で接続してコンソールから以下のようなスクリプトを入力します。

path の '.' はスクリプトの起点のフォルダを表し、
`res_mods/<WOT_VERSION>`
になります。
つまり、先程置いた `Test.swf` を読み込んだ Flash クラスのインスタンスを作成し、
変数 f に格納します。
そしてメソッドの active() を用いて WoT クライアントに貼り付けます。

```python
from gui.Scaleform.Flash import Flash
f = Flash('Test.swf', path='.')
f.active(True)
```

ここでオプション引数 `path` は1番目の引数で指定した SWF ファイルのパスです。
`.` は起点で `res_mods/<WOT_VERSION>` となりますが、
`path` を省略した場合はデフォルトの `gui/scaleform` が設定され、
`res_mods/<WOT_VERSION>/gui/scaleform` となります。
(実際には仮想ファイルシステムの検索ルールに従います)


`Test.swf` の貼付け結果です。
黄色の円は表示されましたが、表示位置も大きさも思っていたのと違います。
何よりいくつかの UI を除いて画面全体が `Test.swf` で設定した背景色の黒で隠されてしまっています。
あと、スクリーンショットではわかりませんが、マウスカーソルが消えてしまっていて、
CTRL キーを押してもウィンドウ外にマウスカーソルを出すことができません。

![Test.swf 表示結果](/resources/image_20170617_13.png)

### Flash クラスのプロパティ

今度は Flash のインスタンスにプロパティを追加してみます。

プロパティ名から推測できるように、
`backgroundAlpha`
は背景の透明度を指定するもので 0.0 で透明になります。
また、
`wg_inputkeyMode`
は、入力モードを制御するプロパティで、
入力を処理しない場合は 2 を指定します。

```python
from gui.Scaleform.Flash import Flash
f = Flash('Test.swf', path='.')
f.movie.backgroundAlpha = 0.0
f.movie.scaleMode = 'NoScale'
f.component.heightMode = 'PIXEL'
f.component.widthMode = 'PIXEL'
f.component.wg_inputKeyMode = 2
f.component.focus = False
f.component.moveFocus = False
f.active(True)
```

どうでしょうか。
背景は透明になり、マウスの制御も通常と同様になりました。
黄色の円の大きさは指定通りになりましたが、位置がどのように決まっているかがわかりません。

![Test.swf プロパティ追加後の表示結果](/resources/image_20170617_14.png)


## Resize の制御

実は SWF を WoT に貼り付けた時点で、
SWF の描画領域が WoT の表示領域にリサイズされます。

そのため、SWF 側でリサイズのイベントを捕捉して
表示するオブジェクトの位置を再調整する必要があります。

具体的には
[Resize イベントの捕捉 - WoT mod のための SWF 作成 (3)](/2017/06/17/create_swf_catch_resized.html)
で解説したような形で、リサイズ時の処理を SWF 側で行います。

リサイズ時の処理を行った SWF を `Test2.swf` として
同様に WoT に貼り付けます。

SWF ファイルを上書きする場合は、
WoT の仮想ファイルシステムによって古い SWF がキャッシュされるので、
WoT を再起動する必要があることに注意してください。

```python
from gui.Scaleform.Flash import Flash
f = Flash('Test2.swf', path='.')
f.movie.backgroundAlpha = 0.0
f.movie.scaleMode = 'NoScale'
f.component.heightMode = 'PIXEL'
f.component.widthMode = 'PIXEL'
f.component.wg_inputKeyMode = 2
f.component.focus = False
f.component.moveFocus = False
f.active(True)
```

結果です。
確認しやすいように描画領域のサイズを表示するコードも `Test2.swf` には含めています。
取得した描画領域のサイズを元に中央の位置を計算して円を配置しています。

![Test.swf リサイズ制御の表示結果](/resources/image_20170617_15.png)
