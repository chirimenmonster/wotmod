---
layout: page
title: Flash オブジェクトの貼り付け
sitemap: false
---
**(作成中)**

WoT の画面に Flash オブジェクトを貼り付けてみます。

## WoT 上での Flash の扱い

WoT のエンジンである BigWorld の関数 GUI.Flash()
で Flash ムービーを再生することは可能ですが、
もっと簡便に Flash を利用できるクラス Flash が
`scripts/client/gui/Scaleform/Flash.py`
で定義されているので通常はこちらか、その派生クラスを使います。

## wot-debugserver による Flash オブジェクトの貼付け

wot-debugserver を使えば
リプレイ画面などに　Flash オブジェクトを貼り付けることができます。

あらかじめ、`res_mods/<WOT_VERSION>` に `test.swf` を置いておきます。
この `test.swf` は FlashDevelop を使って作成した黒背景に
黄色の円を描画する SWF です。
背景色の黒は FlashDevelop のプロジェクト設定で指定しておきます。

```actionscript
package
{
	import flash.display.Shape;
	import flash.display.Sprite;
	import flash.events.Event;
	
	/**
	 * ...
	 * @author Chirimen
	 */
	public class Main extends Sprite 
	{
		private var shape1:Shape = new Shape();
		
		public function Main() 
		{
			if (stage) init();
			else addEventListener(Event.ADDED_TO_STAGE, init);
		}
		
		private function init(e:Event = null):void 
		{
			removeEventListener(Event.ADDED_TO_STAGE, init);
			// entry point
			shape1.graphics.beginFill(0xFFFF00, 0.8);
			shape1.graphics.drawCircle(100, 100, 20);
			stage.addChild(shape1);
		}
	}
}
```

適当なリプレイファイルを再生して適当なところで止めた後、
wot-debugserver で接続してコンソールから以下のようなスクリプトを入力します。

path の '.' はスクリプトの起点のフォルダを表し、
`res_mods/<WOT_VERSION>`
になります。
つまり、先程置いた `test.swf` を読み込んだ Flash クラスのインスタンスを作成し、
変数 f に格納します。
そしてメソッドの active() を用いて WoT クライアントに貼り付けます。

```python
from gui.Scaleform.Flash import Flash
f = Flash('test.swf', path='.')
f.active(True)
```

無事、`test.swf` が表示されたでしょうか。

黄色の円は表示されましたが、
一部を除いて WoT の背景が黒になっています。
また、マウスカーソルが表示されなくなりました。


ここでオプション引数 `path` は1番目の引数で指定した SWF ファイルのパスです。
`.` は起点で `res_mods/<WOT_VERSION>` となりますが、
`path` を省略した場合はデフォルトの `gui/scaleform` が設定され、
`res_mods/<WOT_VERSION>/gui/scaleform` となります。
(実際には仮想ファイルシステムの検索ルールに従います)


```python
from gui.Scaleform.Flash import Flash
f = Flash('test.swf', path='.')
f.movie.backgroundAlpha = 0.0
f.movie.scaleMode = 'NoScale'
f.component.heightMode = 'PIXEL'
f.component.widthMode = 'PIXEL'
f.component.wg_inputKeyMode = 2
f.component.focus = False
f.component.moveFocus = False
f.active(True)
```

```actionscript
package
{
	import flash.display.Shape;
	import flash.display.Sprite;
	import flash.events.Event;
	import flash.text.TextField;
	import flash.text.TextFieldAutoSize;
	import flash.text.TextFormat;
	import flash.text.TextFormatAlign;
	import flash.display.StageScaleMode;
	import flash.display.StageAlign;
	
	/**
	 * ...
	 * @author Chirimen
	 */
	public class Main extends Sprite 
	{
		private var shape1:Shape = new Shape();
		private var text1:TextField = new TextField();
		private var counter:int = 0;
		
		public function Main() 
		{
			if (stage) init();
			else addEventListener(Event.ADDED_TO_STAGE, init);
		}
		
		private function init(e:Event = null):void 
		{
			removeEventListener(Event.ADDED_TO_STAGE, init);
			// entry point
			stage.scaleMode = StageScaleMode.NO_SCALE;
			stage.align = StageAlign.TOP_LEFT;
			
			shape1.graphics.beginFill(0xFFFF00, 0.8);
			shape1.graphics.drawCircle(0, 0, 20);
			stage.addChild(shape1);
			
			text1.autoSize = TextFieldAutoSize.LEFT;
			text1.x = 50;
			text1.y = 50;
			text1.htmlText = "<font color='#ffff00'>Hello World: " + stage.stageWidth + ", " + stage.stageHeight + "</font>";
			stage.addChild(text1);
			
			stage.addEventListener(Event.RESIZE, resizeHandler);
		}
		
		private function resizeHandler(e:Event):void {
			x = stage.stageWidth / 2;
			y = stage.stageHeight / 2;
			text1.htmlText = "<font color='#ffff00' size='18'>Resize(" + counter + "): " + stage.stageWidth + ", " + stage.stageHeight + "</font>";
			text1.x = x - text1.width / 2;
			text1.y = y + 40;
			shape1.x = x;
			shape1.y = y;
			counter = counter + 1;
		}
	}
	
}
```
