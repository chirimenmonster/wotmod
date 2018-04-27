---
layout: post
title: WoT scripts メモ
date: 2018-04-27 08:00 +0900
---

WoT の scripts に関するメモです。

## イベント

グローバル変数 g_playerEvents が scripts/client/PlayerEvents.py で定義されています。

WoT クライアントの状態変化にかかわる主要なイベントに対応するメソッドが
"on" + イベント名
の形式で g_playerEvents の属性として定義されています。

これらの属性はクラス Events のインスタンスを値として設定されており、
イベント発生時に処理すべき関数を複数登録しておくことができます。
登録した関数は、イベント発生時に順に実行されます。

イベント AccountBecomePlayer 発生時に処理する関数として self.__subscribe を追加
```python
g_playerEvents.onAccountBecomePlayer += self.__subscribe
```

イベント AccountBecomePlayer 発生時に処理する関数から self.__subscribe を削除
```python
g_playerEvents.onAccountBecomePlayer -= self.__subscribe
```


## view

多くの GUI は AS3 (Flash) と Python スクリプトのセットとして提供されていて、
それは view と呼ばれています。
view には制御のための名称が付けられており、view_alias と呼ばれています。

WoT 内で使用される view_alias は scripts/client/gui/Scaleform/daapi/settings/views.py の
VIEW_ALIAS で定義されています。

