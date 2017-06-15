---
layout: post
title: "wotmod 作成時に非圧縮 ZIP するときの注意点"
date: 2017-05-22 22:30 +0900
last_modified_at: 2017-06-15 18:00 +0900
---
wotmod は非圧縮 ZIP で作成する仕様になっていますが、
普段から非圧縮 ZIP の操作に慣れているという人は多くはないと思います。
そこで wotmod を作成する際の注意点などをまとめました。

## 非圧縮 ZIP の作成方法

### 7-Zip などのツールを使用する場合
7-Zip などの非圧縮 ZIP の作成に対応したツールを使って作成できます。

7-Zip では書庫の作成時に書庫形式を "zip"、圧縮レベルを「無圧縮」に指定すると非圧縮 ZIP となります。

![7-Zip](/resources/image_20170522_04.png "7-Zip")

### Python で作成する場合
Python の zipfile モジュールで圧縮方法に ZIP_STORED を指定するなどしても作成可能です。

ZipFile クラスの write でフォルダ階層付きのファイルを追加すると、
自動で作成される親フォルダの圧縮形式が ZIP_STORED になりません。
面倒でも先に親フォルダを ZIP_STORED 指定で追加しておきましょう (もっと良い方法があるかもしれません)。


## 非圧縮 ZIP の確認方法
非圧縮 ZIP が意図通りに作成できているかどうかは 7-Zip などのツールを使って確認できます。

### 非圧縮 ZIP の例
ファイルのコンテキストメニュー（右クリックメニュー）の「7-Zip」から「開く」を実行します。
すべてのファイル・フォルダの「圧縮方法」の欄が "Store" になっていれば非圧縮ZIPです。

![非圧縮ZIP](/resources/image_20170522_01.png "非圧縮ZIP")

### 圧縮 ZIP の例
一般的な圧縮 ZIP の例です。
フォルダは "Store" ですが
ファイルの「圧縮方法」が "Deflate" になっています。
"Deflate" は圧縮方式を表しています。
ZIP ファイルとしては通常の状態ですが wotmod としては不正な形式です。

![圧縮ZIP](/resources/image_20170522_02.png "圧縮ZIP")

### 不完全な非圧縮 ZIP
ZIP ファイルの作成の仕方によっては、圧縮ファイルを非圧縮ファイルが混在することもあります。
次の例ではファイルは非圧縮 (Store) ですが、フォルダの「圧縮方法」が空欄になっています。
こうしたケースも wotmod では不正な形式となります。

![圧縮ZIP](/resources/image_20170522_03.png "圧縮ZIP")

## 圧縮形式が正しくないときのエラー
圧縮形式が正しくない .wotmod ファイルが検出されると次のようなエラーダイアログが表示され、
python.log には `NOT loaded: unsupported compression type` というエラーメッセージが出力されます。

![エラーダイアログ](/resources/image_20170523_01.png "エラーダイアログ")

```
2017-05-23 20:15:17.524: INFO: [PY_DEBUG] Mod file 'mods/0.9.18.0/chirimen.spotmessanger_1.1-dev.wotmod' NOT loaded: unsupported compression type
```
