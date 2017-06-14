---
layout: page
title: Flash オブジェクトの貼り付け
sitemap: false
---
**(作成中)**

WoT の画面に Flash オブジェクトを貼り付けてみます。

## クラス Flash

Flash オブジェクトは　BigWorld の関数 GUI.Flash() で作成することもできますが、
より簡便に利用できるクラス Flash が
`scripts/client/gui/Scaleform/Flash.py`
で定義されているので通常はこちらか、その派生クラスを使います。

### wot-debugserver による Flash オブジェクトの貼付け

wot-debugserver を使って　Flash オブジェクトを貼り付けてみましょう。

リプレイファイルを再生して適当なところで止めた後、
wot-debugserver で接続してコンソールから以下のようなスクリプトを入力します。

このとき、`res_mods/<WOT_VERSION>` に `test.swf` を置いておきます。

```python
from gui.Scaleform.Flash import Flash
flash = Flash('test.swf', path='.')
flash.active(True)
```

無事、`test.swf` が表示されたでしょうか。

ここでオプション引数 `path` は1番目の引数で指定した SWF ファイルのパスです。
`.` は起点で `res_mods/<WOT_VERSION>` となりますが、
`path` を省略した場合はデフォルトの `gui/scaleform` が設定され、
`res_mods/<WOT_VERSION>/gui/scaleform` となります。
(実際には仮想ファイルシステムの検索ルールに従います)
