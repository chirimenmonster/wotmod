---
layout: page
title: wot-debugserver を使ったデバッグ
sitemap: false
---
**(作成中)**

REPL (Read-Eval-Print Loop) による WoT のデバッグ手法を使ってみました。
実行中に WoT クライアントに対話的に接続して、あれこれできます。
mod の開発とかに有効です。


## 参考

以下のページを参考にしました。

+ ["Hello wot" WoTにおけるPythonプログラミングのスタート - ’s blog](http://tkyma.hatenablog.jp/entry/2015/12/31/225737)


## 準備

必要なもの

+ WoT
+ XVM
+ wot-debugserver
+ Python 2.7 が動作する環境 (MSYS2)

Python 2.7 が動作すれば使用可能ですが、
入力の自動補完とかが利用できる MSYS2 とか Cygwin 環境で Python を使うのがいいみたいです。
わたしは普段 MSYS2 を使ってます。


## インストール

1. GitHub から wot-debugserver のファイル一式をダウンロードする
2. wot-debugserver の python フォルダに移動する
3. python2 -m compileall . でコンパイルする (.pyc ファイルの作成)
4. XVM の packages フォルダ (<WoT_game_folder>/res_mods/mods/packages) に replserver というフォルダを作成
5. 2 の python フォルダを 4 の replserver フォルダにコピーする (<WoT_game_folder>/res_mods/mods/packages/replserver/python)

## 実行

1. WoT のリプレイを再生する
2. 途中で止める (止めなくても可)
3. MSYS2 のコンソールを立ち上げる
4. wot-debugserver の python フォルダの client.py を実行する

