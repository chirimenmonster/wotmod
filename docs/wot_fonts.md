---
layout: page
title: フォント
date: 2017-05-21 17:30 +0900
---

## WoT のフォント設定の概要

WoT クライアントで使用するフォントの指定は `res/gui/flash/fontconfig.xml` で行われており、
使用するフォントデータは `fonts_*.swf` ファイルにまとめられています。

### fontconfig.xml

`fontconfig.xml` は WoT クライアントが使用するフォントを指定する設定ファイルです。

データの形式は XML ですが、WoT の他の多くの XML ファイルと同様にバイナリに変換されているので、
内容の確認には WoT XML Editor を使用してテキストに変換する必要があります。
テキストに変換した XML ファイルは WoT クライアントでそのまま使用できるので、
修正した `fontconfig.xml` を使用する場合は再度バイナリに変換する必要はなく、そのまま設置すれば有効になります。

### fonts_*.swf

`fonts_*.swf` は Flash の SWF ファイルで、ここではフォントデータをパッケージしたものを指します。

SWF ファイルの中身を確認したり修正するには FFDec (JPEXS Free FLash Decompiler) を使用します。


## WoT クライアントの fontconfig.xml 

### 0.9.18.0 日本語クライアントの fontconfig.xml

WoT 0.9.18.0 日本語クライアントの fontconfig.xml を示します。

このうち、WoT から参照する際のキーとなる要素は変更できない（変更しても意味がない）ので、
変更可能なのはフォントファイルを指定する `fontlib` と
使用するフォント名を指定する `runtime`、
そしてオプションの `scaleFactor`
のみになります。

サイズは異なりますが、日本語クライアントでは `WarHeliGothicJK` というただ一種類のフォントが使用されています。

```xml
<fontconfig.xml>
 <config>
  <name>All</name>
  <fontlib>fonts_ja.swf</fontlib>
  <map>
   <alias>
    <embedded>$TitleFont</embedded>
    <runtime>WarHeliGothicJK</runtime>
    <scaleFactor>0.9</scaleFactor>
   </alias>
   <alias>
    <embedded>$ChinaNameFont</embedded>
    <runtime>WarHeliGothicJK</runtime>
   </alias>
   <alias>
    <embedded>$FieldFont</embedded>
    <runtime>WarHeliGothicJK</runtime>
    <scaleFactor>0.9</scaleFactor>
   </alias>
   <alias>
    <embedded>$IMELanguageBar</embedded>
    <runtime>WarHeliGothicJK</runtime>
   </alias>
   <alias>
    <embedded>$TextFont</embedded>
    <runtime>WarHeliGothicJK</runtime>
   </alias>
   <alias>
    <embedded>$PartnerCondensed</embedded>
    <runtime>WarHeliGothicJK</runtime>
   </alias>
   <alias>
    <embedded>$PartnerLightCondensed</embedded>
    <runtime>WarHeliGothicJK</runtime>
   </alias>
   <alias>
    <embedded>$UniversCondC</embedded>
    <runtime>WarHeliGothicJK</runtime>
    <scaleFactor>0.98</scaleFactor>
   </alias>
  </map>
 </config>
</fontconfig.xml>
```

### 0.9.18.0 英語クライアントの fontconfig.xml
WoT 0.9.18.0 英語クライアントの fontconfig.xml を示します。

日本語クライアントの場合と比べると、`fontlib`, `runtime`, `scaleFactor` のみが異なります。
また、一種類ではなく複数のフォントが指定されています。

ここで指定されているフォントには日本語用の文字のグリフは含まれていないので、
日本語を表示するには `fontconfig.xml` を修正または差し替えるか、
日本語のグリフを追加した合成フォントを作成する必要があります。

```xml
<fontconfig.xml>
 <config>
  <name>All</name>
  <fontlib>fonts_sd.swf</fontlib>
  <map>
   <alias>
    <embedded>$TitleFont</embedded>
    <runtime>ZurichCondBold</runtime>
   </alias>
   <alias>
    <embedded>$ChinaNameFont</embedded>
    <runtime>ZurichCond</runtime>
   </alias>
   <alias>
    <embedded>$FieldFont</embedded>
    <runtime>ZurichCond</runtime>
   </alias>
   <alias>
    <embedded>$IMELanguageBar</embedded>
    <runtime>ZurichCond</runtime>
   </alias>
   <alias>
    <embedded>$TextFont</embedded>
    <runtime>Tahoma</runtime>
   </alias>
   <alias>
    <embedded>$PartnerCondensed</embedded>
    <runtime>PartnerCondensed</runtime>
   </alias>
   <alias>
    <embedded>$PartnerLightCondensed</embedded>
    <runtime>PartnerLightCondensed</runtime>
   </alias>
   <alias>
    <embedded>$UniversCondC</embedded>
    <runtime>UniversCondC</runtime>
   </alias>
  </map>
 </config>
</fontconfig.xml>
```


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