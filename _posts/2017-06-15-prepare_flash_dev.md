---
layout: post
title: Flash 開発環境の作成
date: 2017-06-15 17:45 +0900
---
Flash オブジェクト （SWF ファイル) の作成には
Apache Flex SDK と FlashDevelop という無料のツールが利用できます。
このうち、Flash スクリプト (ActionScript) のソースから SWF をコンパイルするツールが
Apache Flex SDK で、
FlashDevelop はその統合開発環境です。

## FlashDevelop のインストール

先に FlashDevelop からインストールします。
というのは Flex SDK など必要なツールは FlashDevelp からインストールして管理できるようになっているからです。
ただし、後述するようにわたしが試したときはエラーが出て動作しませんでした。
それでも、本来の動作を期待して FlashDevelop からインストールすることにします。

### ダウンロード

[FlashDevelop の公式サイト](http://www.flashdevelop.org/)
から最新版の FlashDevelop をダウンロードします。
現在の最新版は 5.2.0 です。

### インストーラの実行

ダウンロードしたインストーラ FlashDevelop-5.2.0.exe を実行します。
インストーラを実行すると、
次のような警告ダイアログが表示されます。
32-bit Java の 1.6 以降をインストールしてくようにとのこと。
(64-bit 版のみしかインストールされていない状態でも動作しているようには見えましたが何か動かない機能があるかもしれません)

![警告ダイアログ](/resources/image_20170615_01.png)

インストールするコンポーネント選択画面で日本語言語パックを指定しておきます。
これでインスール直後から日本語メニューが出るかと思ったのですが出なかったので、
後から設定で変更しました。

![コンポーネント選択](/resources/image_20170615_02.png)

### FlashDevelop の実行

表示言語の設定画面です。
FlashDevelop を起動したあと、
メニューの [Tools]-[Settings] から呼び出せます。

![環境設定-ロケール](/resources/image_20170615_03.png)

### 外部プログラムのインストールと設定

Flash のコンパイルなどに必要な外部プログラム、
具体的に言うと Apache Flex SDK をインストールします。

メニューの [ツール]-[ソフトウェアをインストール...] から管理ツール AppMan を開きます。

![外部プログラムインストールメニュー](/resources/image_20170615_04.png)

AppMan の下の方 (スクロールバーのある領域の外側) に
"Select: All None ..." という行がありますが、
そこの Flex を指定します。
そうすると、上の枠内のアプリケーションリストで Flex に関連するアプリケーションの全てに
チェックマークが付きます。
具体的には Apache Flex SDK と　Flash Player (SA) です。

この状態で右下のボタン "Install 2 items." を押すと、ダウンロードとインストールが行われます。

![インストール対称の外部プログラムの指定](/resources/image_20170615_05.png)

インストールされるはずだったのですが、このようなエラーになってしまいました。

このとき失敗したのは Apache Flex SDK の方で、
Flash Player の方はインストールされています。

![外部プログラムインストールエラー](/resources/image_20170615_06.png)

このように現時点では FlashDevelop による Apache Flex SDK のインストールに失敗するようなので、
別途マニュアルで Apache Flex SDK をインストールしてからFlashDevelop
FlashDevelop にインストール場所を設定することにします。


## Apache Flex SDK のインストール

FlashDevelop からの自動インストールが失敗したので、
マニュアルで Apache Flex SDK をインストールします。

### ダウンロード

[FlashDevelop の公式サイト](http://flex.apache.org/)
から Apache Flex SDK Installer をダウンロードします。
SDK Installer の現在の最新版は 3.2 です。

### Apache Flex SDK Installer のインストール

Installer をインストールします。
何を言ってるかよくわからないですね。

ダウンロードした
apache-flex-sdk-installer-3.2.0-bin.exe
を実行すると、
Apache Flex SDK Installer
がインストールされます。

この Installer を実行することでようやく Flex SDK がインストールされます。

### Apache Flex SDK Installer の実行

Apache Flex SDK Installer を実行すると次のようなウィンドウが表示されます。

Flex, AIR, Flash Player のバージョンが選択できるようになっています。
AIR, Flash Player は初期表示よりも新しいバージョンが選べるようになっていますが、
指定したらその後のインストールでエラーになったので、
よくわからなければそのまま "NEXT" を押してデフォルトの状態でインストールするのが良いと思います。

![Flexインストーラ](/resources/image_20170615_07.png)

次に Flex SDK をインストールする場所を指定します。
インストールフォルダは、あらかじめ空のフォルダを作成しておく必要があります。
フォルダ名は任意につけられますが、バージョン名などを含めて管理しやすいようにしておきましょう。

ここではCドライブの直下に Apache_Flex_4_16_0 というフォルダを作成したものを指定しました。

![Flex SDK インストールフォルダ指定](/resources/image_20170615_08.png)

許諾するライセンスにチェックを入れ、"Install" ボタンを押せば
インストールが開始されます。

![Flex SDK ライセンス許諾](/resources/image_20170615_09.png)


## FlashDevelop の追加設定

先程インストールした Apache Flex SDK は FlashDevelop の管理下にはないので、
Flex SDK のインストールフォルダを FlashDevelop にマニュアルで設定する必要があります。

FlashDevelop を起動して [ツール]-[環境設定] から、
左側のプラグインで AS3Context を選択します。
すると、右側の設定項目一覧に "Installed Flex SDKs" が現れます。

![FlashDevelop環境設定](/resources/image_20170615_10.png)

ここで、"Installed Flex SDKs" を選択し、
"InstallSDK[] 配列"
の右の方にあるボタン "..." をクリックすると、
InstallSDK コレクションエディタ
が開きます。

ここで、左下の "追加" ボタンを押し、
右側の "Location" "Path" を選択して、
先程インストールした Flex SDK のフォルダを指定します。

指定すると次の図のように Properties に Version などの情報が自動的に
現れるはずです。
OK ボタンを押して設定を保存して終了します。

![FlashDevelop環境設定](/resources/image_20170615_11.png)

これで FlashDevelop を使う準備ができました。
