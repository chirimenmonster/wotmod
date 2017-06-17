---
layout: post
title: Resize イベントの捕捉 - WoT mod のための SWF 作成 (3)
date: 2017-06-17 09:25 +0900
---
[前回](/2017/06/17/create_swf_simple.html)
作成した SWF を FlashPlayer などで再生しているときに
ウィンドウサイズを変更すると、
表示内容もウィンドウサイズに合わせて拡大縮小されてしまいます。

ここでは、ウィンドウサイズが変更されても表示内容の大きさは変化せず、
かつ、ウィンドウの中心に位置するような SWF の作成例を示します。

## 作成の要点

### ウィンドウサイズが変化しても内容物の大きさは固定にする

ウィンドウサイズが変化したときに、
内容物の大きさを変えるかどうかは `stage` のプロパティ `scaleMode` で制御されます。
変化させないようにするには `scaleMode` に `StageScaleMode.NO_SCALE` を設定します。

同時に ｀stage` の位置合わせは左上を基準にしておきます。

```actionscript
import flash.display.StageScaleMode;
import flash.display.StageAlign;

stage.scaleMode = StageScaleMode.NO_SCALE;
stage.align = StageAlign.TOP_LEFT;
```

### ウィンドウサイズが変化したときのイベントを捕捉する

ウィンドウサイズが変化したときに、
内容物の位置を再設定する必要があります。

ウインドウサイズが変化すると `Event.RESIZE` イベントが `stage` に送られるので、
`stage` にハンドラを登録します。
ここで `resizeHandler` は後で説明するプライベート関数です。

```actionscript
stage.addEventListener(Event.RESIZE, resizeHandler);
```

### ウィンドウサイズが変化したときの処理

作成した図が常に中央に位置するように、
ウィンドウサイズをもとに位置を計算します。
この処理は先程設定したイベントハンドラ `resizeHandler` で行います。

ウィンドウのサイズは `stage` のプロパティ
`stageWidth`, `stageHeight` で取得できます。

```actionscript
private function resizeHandler(e:Event = null):void {
    x = stage.stageWidth / 2;
    y = stage.stageHeight / 2;
    shape1.x = x;
    shape1.y = y;
}
```

### 初期設定

図の初期位置の設定も `resizeHandler` で行います。
イベントは存在しないので、引数は空にして直接呼び出します。

```actionscript
resizeHandler();
```

## 完成形

まとめると次のようなコードになります。

```actionscript
package
{
	import flash.display.Sprite;
	import flash.display.Shape;
	import flash.display.StageScaleMode;
	import flash.display.StageAlign;
	import flash.events.Event;
	import flash.text.TextField;
	import flash.text.TextFieldAutoSize;
	import flash.text.TextFormat;
	import flash.text.TextFormatAlign;
	
	public class Main extends Sprite 
	{
		private var shape1:Shape = new Shape();
		private var text1:TextField = new TextField();
		
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
			stage.addChild(text1);
			
			resizeHandler();
			
			stage.addEventListener(Event.RESIZE, resizeHandler);
		}
		
		private function resizeHandler(e:Event = null):void {
			x = stage.stageWidth / 2;
			y = stage.stageHeight / 2;
			shape1.x = x;
			shape1.y = y;
			text1.htmlText = "<font color='#ffff00' size='18'>Stage: (" + stage.stageWidth + ", " + stage.stageHeight + ")</font>";
			text1.x = x - text1.width / 2;
			text1.y = y + 40;
		}
		
	}
	
}
```

実行結果です。
ウィンドウをリサイズしても円は常に中央に位置します。

![Test2.swf実行結果](/resources/image_20170617_12.png)