---
layout: post
title: WoT の View に関するライフサイクル
date: 2019-04-17 19:45 +0900
---
## 概要

WoT において Flash で書かれた GUI などを取り扱うための基本となるクラス View について、
生成と破棄に関する部分のメモです。

※ WoT 1.4.1.2 のコードを参照しています。

## View のクラス構成

クラス View とそのスーパークラスの関係を示します。
大半はモジュールツリー gui.Scaleform.framework.entities の下で定義されています。
View の生成と破棄の基本構造は DisposableEntity にあります。

```
View
    AbstractViewMeta
        BaseDAAPIComponent
            BaseDAAPIComponentMeta
                DAAPIEntity
                    EventSystemEntity
                        DisposableEntity
                            Object
    ViewInterface
        Object
```


### 作成情報の登録

View を使用するのに先立って、View の作成情報 (ViewSettings) を View のファクトリに登録する必要があります。
作成情報は View の名称 （alias）、クラス名、関連付けられる SWF ファイルのパス、
View のタイプやモードなどです。

```python
from gui.Scaleform.framework import ViewSettings, ViewTypes, ScopeTemplates, g_entitiesFactories

viewSettings = ViewSettings(
    MY_VIEW_ALIAS,
    MyView,
    MY_VIEW_SWF_PATH,
    ViewTypes.WINDOW,
    None,
    ScopeTemplates.DEFAULT_SCOPE
)
g_entitiesFactories.addSettings(viewSettings)
```

クラス ViewSettings はモジュール gui.Scaleform.framework で定義されます。


### View の読み込み

View の読み込みには　LobbyEntry または BattleEntry の loadView メソッドを使用します。
LobbyEntry, BattleEntry はいずれも AppEntry のサブクラスで、
g_appLoader の getApp, getDefLobbyApp, getDefBattleApp メソッドで取得できます。
g_appLoader はクラス _AppLoader のインスタンスです。

```python
from gui.app_loader import g_appLoader
from gui.Scaleform.framework.managers.loaders import SFViewLoadParams

app = g_appLoader.getDefLobbyApp()
app.loadView(SFViewLoadParams(alias, name))
```

loadView の引数には SFVIewLoadParams のインスタンスを渡します。
SFVIewLoadParams の引数 alias は ViewSettings で指定した View の名称、
name は、同一の alias に対して複数の View を作成する場合の識別名です。
一つしか View を作成しない場合は省略できます。

クラス SFVIewLoadParams はモジュール gui.Scaleform.framework.managers.loaders で定義されています。


### View の取得

app.loadView は View のインスタンスを返さないので、
View のインスタンスはコンテナマネージャから検索用のキーを指定して取得する必要があります。

コンテナマネージャは LobbyEntry, BattleEntry のプロパティ containerManager として取得でき、
そのメソッド getViewByKey を用いて View のインスタンスを取得できます。
検索キーはクラス ViewKey のインスタンスで、loadView の際に指定した alias と name のペアから生成されます。

```python
from gui.Scaleform.framework.entities.View import ViewKey

pyEntity = app.containerManager.getViewByKey(ViewKey(alias, name))
```

### View の生成と破棄

View の生成と破棄は自動的に行われます。
通常の View であれば、
生成は loadView によって View を読み込んだとき、
破棄は View が読み込まれている AppEntry が終了するときです。


#### DisposableEntity

View の生成と破棄に関するクラスです。
モジュール gui.Scaleform.framework.entities.DisposableEntity で定義されています。

主要なメソッドに create, destroy があります。

メソッド create のコードの一部です。
生成のための具体的な処理はサブクラスの _populate で行われることを想定しています。
onCreate, onCreated はクラス Event のインスタンスで、
生成開始時、生成完了時それぞれにフックを設定することができます。

```python
            self.__changeStateTo(EntityState.CREATING)
            self.onCreate(self)
            self._populate()
            self.__changeStateTo(EntityState.CREATED)
            self.onCreated(self)
            self.__invalidatePostponedState()
```

メソッド destroy のコードの一部です。
破棄のための具体的な処理はサブクラスの _dispose, _destroy で行われることを想定しています。
onDispose, onDisposed はクラス Event のインスタンスで、
破棄開始時、破棄完了時それぞれにフックを登録することができます。
破棄完了後はイベントマネージャもクリアされます。

```python
            needToBeDisposed = self.__lcState == EntityState.CREATED
            self.__changeStateTo(EntityState.DISPOSING)
            self.onDispose(self)
            if needToBeDisposed:
                self._dispose()
            self._destroy()
            self.__changeStateTo(EntityState.DISPOSED)
            self.onDisposed(self)
            self.__eManager.clear()
            self.__invalidatePostponedState()
```

onCreate, onCreated, onDispose, onDisposed に登録したフックには
当該 View のインスタンスが引数として渡されます。

以下は View に関連付けられた Flash オブジェクトが生成された後で
設定値を渡す関数を呼び出す例です。
loadView によって View がロードされた時点では Flash の読み込みがまだ完了していないので、
Flash が確実に読み込まれた後で実行するように onCreate にフックを設定しています。

```python
def init():
    app = g_appLoader.getDefBattleApp()
    app.loadView(SFViewLoadParams(alias, name))
    pyEntity = app.containerManager.getViewByKey(ViewKey(alias, name))
    pyEntity.onCreate += setConfig

def setConfig(pyEntity):
    pyEntity.as_setConfigS(config)
```

View の破棄の過程では、関連付けられている SWF の破棄や、
イベントフックの消去、メモリの解放などの処理が行われます。
外部からの参照が残っているとメモリの解放処理が正しく行われなくなるので、
参照を持たせる場合には weakref.proxy() を使用して弱参照とする必要があることに注意してください。

```python
    global g_pyEntity
    g_pyEntity = weakref.proxy(pyEntity)
```
