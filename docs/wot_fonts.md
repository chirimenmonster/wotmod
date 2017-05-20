---
layout: page
title: フォント
date: 2017-05-20 21:30 +0900
---

## WoT のフォント設定

`res/gui/flash/fontconfig.xml`

+ WoT クライアントに含まれている XML ファイルのほとんどはバイナリで内容の確認に WoT XML Editor が必要
+ テキスト形式に変換した XML ファイルは WoT クライアントでそのまま使用できる（編集後、再度バイナリに変換する必要はない）

## WoT クライアントに含まれているフォント

フォントファイルの場所 `res/gui/flash`


| ファイル名        | 概要  |
| ------------- | ---- |
| fonts_sd.swf  | 標準 |
| fonts_ja.swf  | 日本語クライアント用 |
| fonts_zh_cn_sg.swf  | 中国語（簡体字）クライアント用 |
| fonts_zh_tw.swf     | 中国語（繁体字）クライアント用 |
| fonts_ko.swf        | 韓国語クライアント用 |
| fonts_th.swf        | タイ語クライアント用 |
| fonts_vi.swf        | ベトナム語クライアント用 |
| fonts_el.swf        | ギリシア語クライアント用 |


+ 各フォントファイル (`*.swf`) には複数のフォントが含まれている（1つしかない場合もある）
+ `*.swf` に含まれているフォントの確認、編集、差し替えなどは FFDec というツールでできる
+ 使用できるフォントは TrueType 形式のフォントのみ （ PostScript 形式のフォントは使用できない）