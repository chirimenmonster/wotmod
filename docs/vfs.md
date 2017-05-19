---
layout: page
title: 仮想ファイルシステム
---

## 概要

WoT クライアント上のファイル管理は仮想ファイルシステムを利用していて、
同一ID (同一名) のファイルが複数の場所に存在することができます。

同じ ID のファイルが複数存在する場合は paths.xml で指定されている優先順位の高いものが実際に使われます。
この仕組みを使うことで、オリジナルのファイルを削除することなくファイルを上書きする効果を得ることができます。

仮想ファイルシステム上のファイルの読み書きはモジュール ResMgr を使って行い、
使用されるファイルの優先順位は paths.xml での指定によります。

paths.xml は WoT のインストールフォルダに置かれています。

### paths.xml

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

### ResMgr
