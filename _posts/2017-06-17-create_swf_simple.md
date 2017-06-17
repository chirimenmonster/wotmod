---
layout: post
title: 単純な図形の作成 - WoT mod のための SWF 作成 (2)
Date: 2017-06-17 08:00 +0900
---
WoT に貼り付ける SWF のサンプルを
FlashDevelop を使って作成します。

## FlashDevelop の起動と新規プロジェクトの作成

FlashDevelop を起動し、
[プロジェクト]-[新規プロジェクト(N)...]
を実行します。

![新規プロジェクトの作成](/resources/image_20170617_01.png)

プロジェクトの種類一覧から
"AS3 Project"
を選択します。
プロジェクトの名前や場所は任意です。
ここではプロジェクトの名前を "Test" とし、
ドキュメントフォルダの "FlashDevelop" フォルダの下に作成されるようにしました。

![新規AS3プロジェクトを作成](/resources/image_20170617_02.png)

右側ペインに、作成したAS3プロジェクトのツリーが表示されます。
"src" の下の "Main.as" をダブルクリックで選択します。

![作成されたAS3プロジェクト](/resources/image_20170617_03.png)

左側ペインに Main.as の編集画面が開きます。
このテンプレートを元に、
ActionScript のコードを記述し、SWF を作成します。

![Main.asテンプレート](/resources/image_20170617_04.png)


## 図形の作成

### Main.as の編集

Main.as に円を描くコードを追加します。

先頭の `import` がある行の後に以下のコードを追加します。
Shape 定義のインポートです。

```actionscript
import flash.display.Shape;
```

クラス Main に
プライベート変数 `shape1` を追加します。
`shape1` は Shape のインスタンスです。

```actionscript
private var shape1:Shape = new Shape();
```

また、関数 `init()` のコメント `// entry point` の後に
以下のコードを追加します。
ここでは `shape1` の塗りつぶし色に黄色 (0xFFFF00) を指定して、
座標 (100, 100) を中心とした半径 20 の円を定義し、
`stage` の子要素として追加しています。

```actionscript
shape1.graphics.beginFill(0xFFFF00);
shape1.graphics.drawCircle(100, 100, 20);
stage.addChild(shape1);
```

入力後の Main.as はこのようになります。

![Main.asテンプレート](/resources/image_20170617_05.png)


### 背景色の変更

先程入力したコードは黄色の円を描くコードです。
背景が白だと確認しづらいので黒に変更しておきます。

メニューの
[プロジェクト]-[プロジェクト設定(P)...]
を選びます。

![プロジェクト設定](/resources/image_20170617_06.png)

プロジェクト設定のウィンドウが開くので、
[書き出し]
タブで
背景色を黒 (#000000) に設定します。

![背景色設定](/resources/image_20170617_07.png)


### プロジェクトのテスト

ツールバーの ▶ マーク (右向きの三角印) のボタンを押すと、プロジェクトのテストを実行できます。
メニューの
[プロジェクト]-[プロジェクトをテスト（T)]
からも実行できます。

![テストボタン](/resources/image_20170617_08.png)

![テストメニュー](/resources/image_20170617_09.png)

### 動作の確認

プロジェクトのテストを実行すると、
コードにエラーがなければ
Flash Player が起動し、結果を確認することができます。

![結果確認](/resources/image_20170617_10.png)

作成された `Test.swf` は右側ペインで確認できます。

![Test.swf](/resources/image_20170617_11.png)
