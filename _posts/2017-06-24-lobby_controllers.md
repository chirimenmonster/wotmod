---
layout: post
title: WoT のロビー制御
date: 2017-06-24 17:25 +0900
---
WoT のクライアントはその大部分が Python で書かれていて、
フロントエンドとなる swf のコンポーネントを制御する形になっています。
それらのコントローラは Python のインスタンスとして提供されますが、
ここではガレージを例にとってコントローラ取得の手順を紹介します。

（この文書は WoT 0.9.19.0.2 での実装に基づいています）


## APP_NAME_SPACE

WoT アプリケーションの名前空間が
`scripts/client/gui/app_loader/settings.py`
に定義されています。

定義されているのは `scaleform/lobby` と `scaleform/battle` の2つで、
それぞれロビーと戦闘時に対応します。
これらは
`APP_NAME_SPACE.SF_LOBBY`, `APP_NAME_SPACE.SF_BATLE`
として参照できます。

ログインした後のクライアントの状態は、
ロビーと戦闘という、2つの状態を切り替えながら動作することになります。


## ロビーのコントローラの取得

`g_appLoader` のメソッド `getApp` を使ってロビーのコントローラを取得することができます。

`g_appLoader` はクラス `_AppLoader` のシングルトンで
`scripts/client/gui/app_loader/loader.py`
で定義されています。

```python
from gui.app_loader import g_appLoader

lobby = g_appLoader.getApp()
```

ここで得られるロビーのコントローラは
クラス `gui.Scaleform.lobby_entry.LobbyEntry`
のインスタンスです。


## ロビーの画面構成とコントローラ

ロビーの画面は大きく3つの部分に別れており、
上部のメニュー欄 (lobbyHeader)、下部のチャットや通知などのボタン欄 (messengerBar)、
そして中央表示欄となります。
中央の表示欄はガレージを表示しているときであれば hangar が対応します。

![ロビー画面構成](/resources/image_20170624_01.png)

それぞれ以下のように取得できます。`lobby` は先程取得したものを参照します。

```python
from gui.Scaleform.framework import ViewTypes
from gui.Scaleform.daapi.settings.views import VIEW_ALIAS

lobbyView = lobby.containerManager.getView(ViewTypes.VIEW)
lobbyHeader = lobbyView.components[VIEW_ALIAS.LOBBY_HEADER]
messengerBar = lobbyView.components[VIEW_ALIAS.MESSENGER_BAR]
```

`lobbyView` は
クラス　`gui.Scaleform.daapi.view.lobby.LobbyView.LobbyView`
のインスタンス、
`lobbyHeader` は
クラス `gui.Scaleform.daapi.view.lobby.header.LobbyHeader.LobbyHeader`
のインスタンス、
`messengerBar` は
クラス `gui.Scaleform.daapi.view.lobby.messengerBar.messenger_bar.MessengerBar`
のインスタンスです。

中央の表示欄は、現在表示しているものが取得できます。
ガレージを表示しているときはガレージのコントローラが取得できます。

```python
# ガレージを表示している場合
hangar = lobby.containerManager.getView(ViewTypes.LOBBY_SUB)
```

`hangar` は
クラス `gui.Scaleform.daapi.view.lobby.hangar.Hangar.Hangar`
のインスタンスです。

```python
# ショップを表示している場合
storeView = lobby.containerManager.getView(ViewTypes.LOBBY_SUB)
```

`storeView` は
クラス `gui.Scaleform.daapi.view.lobby.store.StoreView.StoreView`
のインスタンスです。


## ガレージのコントローラ

ガレージ (hangar) には幾つかの部品が含まれています。

それらは `hangar` のプロパティ `components` に辞書として格納されています。

```python
hangar.components.keys()  # ['tmenXpPanel', 'header', 'tankCarousel', 'crew', 'questsControl', 'switchModePanel', 'params', 'ammunitionPanel', 'researchPanel']
```

たとえば、車輌選択メニュー (carousel) であれば、以下のように取得できます。

```python
from gui.Scaleform.genConsts.HANGAR_ALIASES import HANGAR_ALIASES

tankCarousel = hangar.components[HANGAR_ALIAS.TANK_CAROUSEL]
```

`tankCarousel` は
クラス `gui.Scaleform.daapi.view.lobby.hangar.carousels.basic.tank_carousel.TankCarousel`
のインスタンスです。


## コントローラの操作

ここで示したコントローラの多くは swf と関連付けられており、
プロパティ `flashObject` を通じて操作が可能です。
`flashObject` にはプロパティ `visible` があり、
表示のON/OFFが可能ですが、コントローラによっては ON→OFF→ON にした後、元の状態に戻らない場合がありました。

```python
tankCarousel.flashObject.visible = False # 初期状態は True。False を代入で表示されなくなる
tankCarousel.flashObject.visible = True  # True に戻すと表示されるが、車輌を選択しても反映されない
```

表示については他にもプロパティ `alpha` があり、
この値を 0 にすると完全透過になって表示されなくなります。
ただし UI 自体は操作可能です。

```python
tankCarousel.flashObject.alpha = 0.0 # 初期状態は 1.0。 0 を代入で見えなくなるが UI 操作は可能
tankCarousel.flashObject.alpha = 1.0 # 1.0 にすると通常の表示。副作用はなさそう
```

以下は `hangar.flashObject.alpha = 0.0` に設定した状態です。

![ロビーUI透明化](/resources/image_20170624_02.jpg)
