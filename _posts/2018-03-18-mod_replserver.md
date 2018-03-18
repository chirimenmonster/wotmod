---
layout: post
title: mod replserver による WoT 操作
date: 2018-03-18 09:40 +0900
---
WoT に REPL (対話型評価環境) を提供する mod replserver の紹介です。
WoT クライアントの実行中に端末から対話的に接続して操作をすることが可能になります。
インターフェースは Python になるので Python について基本的なところは知ってることを前提としています。

## 参考

以下のページを参考にしました。
このページで取り扱っている replserver は wot-debugserver を元に、
可搬性、安定性、機能向上を目指して開発した mod です。
使い方は wot-debugserver を踏襲しているので、
wot-debuserver について書かれた記事なども使い方の参考になると思います。

+ ["Hello wot" WoTにおけるPythonプログラミングのスタート - ’s blog](http://tkyma.hatenablog.jp/entry/2015/12/31/225737)
+ [GutHub: wot-debugserver](https://github.com/juho-p/wot-debugserver)
+ [wot-debugserver を使ったデバッグ](/2017/06/17/wot-debugserver) (旧ページ)

## 準備

以下のものが必要になります。

+ WoT
+ replserver
+ Python 2.7 が動作する環境 または TELNET クライアント

### WoT

あたりまえですね。

### replserver

あらかじめ、この mod を WoT にインストールしておく必要があります。
通常の wotmod と同様に、.wotmod の拡張子がついたファイルを所定のフォルダ
`<WoT_game_folder>/mods/<WoT_version>`
に置きます。

ファイルは
[MOD HUB の Replserver](https://wgmods.net/786/)
または
[GitHub のリリースページ](https://github.com/chirimenmonster/wotmods-replserver/releases/latest)
からダウンロードできます。
MOD HUB は WG による動作確認が行われるので安定していますが、
GitHub のものより更新のタイミングが遅くなります。

### Python 2.7 が動作する環境 または TELNET クライアント

接続の方法には大きく分けて2種類、
replserver に同梱の client.py を Python 2.7 で実行する方法と、
一般の TELNET クライアントを使って接続する方法があります。

おすすめは　MSYS2 または Cygwin 上の Python 2.7 で client.py を実行する方法です。
この場合、GNU readline を用いて、変数や属性、メソッドなどに対する補完機能を利用できます。
Windows 用 Python 2.7 でも client.py は動作しますが、
この場合は補完機能は使用できません。

TELNET クライアントを使用する場合は、
接続先ホストに localhost,　ポートに 2222 を指定して接続します。
この場合も補完機能は使用できません。


## 起動方法

1. WoT を実行する (通常、またはリプレイ再生)
2. python2 client.py を実行、または TELNET で localhost:2222 に接続する

## コマンドの入力例

WoT の画面に文字を貼り付けてみます。
client.py、または TELNET の入出力 (以下、コンソールと呼びます)
に以下のコードを入力します。

戦闘中に行うのは他の人の迷惑 (AFK) なので、リプレイ再生で試しましょう。

```python2
import GUI
t = GUI.Text()
t.text = 'Hello World'
t.colour = (255, 255, 0, 240)
GUI.addRoot(t)
```

上手く行けば、
画面中央に 'Hello World' の文字が出力されるはずです。

![文字の表示](/resources/image_20170617_16.png)


### GUI, GUI.Text の仕様

GUI, GUI.Text は BigWorld で定義されているモジュールです。
仕様についてはたぶん以下のリンクが参考になります。

+ [BigWorld client Documentation - GUI Module Reference](http://monstrofil.xyz/docs/client/doc/GUI.html)


## replserver の動作の仕組み

replserver は mod が実行されると別スレッドを作成して接続待機状態になります。
接続してきた端末 (client.py や TELNET) から
python の式、または文を1行受け取り、
式を評価 (eval) または文を実行 (exec) して、
式であればその評価結果とその際の出力結果を、
文であれば実行時の出力結果を端末に返します。

実行時の名前空間は mod 内ローカルに保持していて、
WoT のメインスレッドには影響を与えませんが、
import した WoT 内のグローバル変数や、
WoT のオブジェクトを取得する関数を通じて
WoT 本体の情報を取得、操作することができます。

先の GUI.Text の例で言うと、
GUI.Text() で作成した Text オブジェクトは replserver のローカル変数 ｔ に代入されるだけですが、
GUI.addRoot() はグローバルオブジェクトへの操作になるので、
生成した Text オブジェクトの情報を送ることで
WoT 本体に影響を与えることができます。


## 他の mod の情報

replyserver を使用して他の mod の情報を (部分的に) 得ることができます。

mod のローダーは `scripts/client/gui/mods/__init__.pyc` にあって、
WoT 内の Python からは `gui/mods/__init__.py` として参照されます。
なので、`gui.mods` を import することで、
mods フォルダにある mod をまとめて import することができます。

```python
from gui import mods
```

![mod のインポート](/resources/image_20180318_01.png)

mod の規約により mod ファイル名は mod_ で開始することになっていますから、
mods.mod_* が mod を表します。

replserver では Python 組み込み関数の `help()` が使用できるので、
`help()` により mod の情報を参照できます。
`help()` は Python のコードから生成したモジュール、関数、クラスなどの情報を生成します。

下は著名な mod の一つである PMOD を対象とした help 表示例です。
PMOD は難読化されていてデコンパイルに失敗しますが、
replyserver を使用することで情報の一端に容易にアクセスすることができます。

![mod のインポート](/resources/image_20180318_02.png)


## おわりに

WoT への接続は PjOrion によっても可能ですが、
大型開発環境である PjOrion と違って replserver はよりコンパクトなアクセスを提供することで
mod 開発への敷居を下げる一助になるのではと思います。
意見やアイデア、バグ報告、質問などあれば twitter にメンション飛ばしてください。
反応します。
