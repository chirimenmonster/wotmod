---
layout: post
title: 仮想ファイルシステム
date: 2017-05-20 18:30 +0900
---

## 概要

WoT クライアント上のファイル管理は仮想ファイルシステムを利用していて、
同一ID (同一名) のファイルが複数の場所に存在することができます。

同じ ID のファイルが複数存在する場合は paths.xml で指定されている優先順位の高いものが実際に使われます。
この仕組みを使うことで、オリジナルのファイルを削除することなくファイルを上書きする効果を得ることができます。

仮想ファイルシステム上のファイルの読み書きはモジュール ResMgr を使って行い、
使用されるファイルの優先順位は paths.xml での指定によります。

paths.xml は WoT のインストールフォルダに置かれています。

## paths.xml

paths.xml は WoT クライアントのリソース検索順序を指定するファイルで、
通常のテキスト形式の XML ファイルです。

例えば WoT 0.9.18.0 の paths.xml の一部を抜粋すると以下のようになっています。

```xml
<root>
  <Paths>
    <Path>./res_mods/0.9.18.0</Path>
    <Path mode="recursive" mask="*.wotmod" root="res">./mods/0.9.18.0</Path>
    <Path type="sd,hd">./res/packages/shared_content.pkg</Path>
    <Path type="hd">./res/packages/shared_content_hd.pkg</Path>
    (略)
    <Path>./res</Path>
  </Paths>
</root>
```

XML 要素 Path で指定しているのがリソースの検索パスになります。
Path にはフォルダだけでなく、ZIP でパッケージ化されたファイルも指定することができます。

#### 検索の例
例えば `gui/gui_settings.xml` というファイルを探すとします。

ファイルは `paths.xml` の記述に従って、まず `./res_mods/0.9.18.0/gui/gui_settings.xml` というファイルがあるかどうか調べられます。
フォルダの起点は WoT クライアントのインストールフォルダなので、
クライアントのフォルダが `C:/Games/World_of_Tanks` であれば
`C:/Games/World_of_Tanks/res_mods/0.9.18.0/gui/gui_settings.xml`
というファイルが調べられることになります。

見つからなければ `./mods/0.9.18.0` の下にある `*.wotmod` が調べられ、
その次は `./res/packages/shared_content.pkg` の中が調べられます。
`*.wotmod` や `*.pkg` は ZIP でパッケージされたファイルです。

そうして `gui/gui_settings.xml` が見つかるまで `Path` の最後まで調べられますが、
実際には `./res/packages/gui.pkg` にあるのでそこで検索は終了です。

本当の処理は厳密には違うのですが、概念的にはだいたいこのような感じです。

## ResMgr
ResMgr は WoT の仮想システムを支えるモジュールで、
BigWorld の一部です。

ResMgr が提供する仮想ファイルシステムは
単に複数のフォルダツリーを統合するだけではなく、
ファイル内の XML ツリーも統合可能なようになっています。
