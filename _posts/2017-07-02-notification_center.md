---
layout: post
title: 通知センターにメッセージを表示する
date: 2017-07-02 07:15 +0900
---
mod から WoT クライアントの通知センターにメッセージを表示する方法について解説します。


## 通知センター Notification Center

通知センター (Notificatio Center) は
WoT クライアントの画面右下にあるメッセージ表示欄です。


## サンプル

ゲーム開始時に通知センターにグリーティングメッセージを出力するサンプルです。

以下のコードを `mod_test.py` という名前で保存し、
`python2 -m compileall mod_test.py` などで `mod_test.pyc` に変換したファイルを
`res_mod/<WoT_version>/scripts/client/gui/mods` フォルダに設置してください。

```python
from PlayerEvents import g_playerEvents
from gui import SystemMessages
from gui.SystemMessages import SM_TYPE

def init():
    g_playerEvents.onAccountBecomePlayer += __onAccountBecomePlayer

def __onAccountBecomePlayer():
    SystemMessages.pushMessage('Hello World', type=SM_TYPE.GameGreeting)
```

`mod_test.pyc` の結果は以下のようになります。
通知センターに 'Hello World' の文字が出力されています。

![出力例](/resources/image_20170702_01.jpg)


### サンプルの解説

関数 `init` は mod の初期化用として WoT 内部から呼び出される初期化ルーチンで、
定義されていなければ単に無視されます。
mod の主な初期化処理はここで行います。

```python
def init():
    g_playerEvents.onAccountBecomePlayer += __onAccountBecomePlayer
```

このサンプルでは、アカウントの処理が終了して有効なプレイヤーになるときに発生するイベント (onAccountBecomePlayer) に、
コールバック関数 `__onAccountBecomePlayer`
を追加登録しています。
`__onAccountBecomePlayer` というのは
この mod で定義している関数名で、
名前は任意です。

`init` が呼び出される時点では、通知センターはまだ存在していないので、
通知センターが作成された後のタイミングでメッセージを出力するようにしています。

```python
def __onAccountBecomePlayer():
    SystemMessages.pushMessage('Hello World', type=SM_TYPE.GameGreeting)
```

メッセージの出力処理です。
`SystemMessages.pushMessage` を使用して通知センターにメッセージを出力します。


## gui.SystemMessages

通知欄へのメッセージ出力制御は、
モジュール `gui.SystemMessages` で提供されています。
Python のファイルでは `scripts/client/gui/SystemMessages.py` が対応します。

このモジュールには、メッセージ出力用に
`pushMessage` と `pushI18nMessage`
の2つの関数が提供されています。
両者の違いは、メッセージカタログを使用した翻訳機構を利用するか否かです。

通常は前者で問題ありません。


### pushMessage

`pushMessage` の引数の定義は以下のようになっています。
`text` は必須、他の引数は省略可能な引数です。

```python
def pushMessage(text, type = SM_TYPE.Information, priority = None, messageData = None):
```

#### text

出力するメッセージです。
プレーンテキストのほか、
ActionScript3 の Text コントロールで使われる
html 風のタグを使用した装飾指定が可能です。

#### type

出力するメッセージのタイプ `SM_TYPE` を指定します。

`SM_TYPE` は `SyetemMessages.py` 内で定義されている列挙定数で、
表示するメッセージの種類を指定します。
指定しない場合は `SM_TYPE.Information` になります。

代表的なものを以下に示します。
完全なリストは `SyetemMessages.py` を参照してください。

|:---|:---|
|SM_TYPE.Error|エラー|
|SM_TYPE.Warning|警告|
|SM_TYPE.Information|通常の通知 (デフォルト)|
|SM_TYPE.GameGreeting|ゲーム開始時のメッセージ|




