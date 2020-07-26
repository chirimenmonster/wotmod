---
layout: post
title: WoT 砲弾速度の設定値と実測の比較検証
mathjax: true
date: 2020-06-28 10:45 +0900
last_modified_at: 2020-06-28 17:00 +0900
---
WoT の砲弾速度はそもそもが隠しパラメータですが、
クライアントのデータベースや、tanks.gg などの情報サイトなどで確認することができます。

以前の記事 [WoT の砲弾の速度]({{site.baseurl}}/2020/01/20/shell_speed.html)
で述べたように、
WoT クライアント内部での処理に使われている砲弾速度は
データベースに記載されている砲弾速度の 80% に設定されています。

この 80% の設定は砲弾の描画にのみ使用されているパラメータという可能性もあるため、
実際の速度を測定することで砲弾速度が 80% になっていることを確認しました。


## WoT の射撃モデル

### 目標位置と射撃方向・速度、到達時間の関係

WoT の射撃のイメージを以下に示します。
自車輌から発射された砲弾の軌跡は、重力加速度の影響を受けて山なりの放物線を描くので、
砲の向き (砲弾を発射する方向) は目標を直接照準する場合よりも上向きになります
(この調整は WoT クライアントによって自動で行われます)。

![射撃モデル]({{site.baseurl}}/resources/fig_20200628_01_shooting.svg)

WoT の砲弾モデルでは空気抵抗などによる減速は生じないので、
到達時間 $T$ と砲弾速度の水平成分 $V_H$、目標までの距離の水平成分 $L_h$ は
下式のような関係にあります。

$$
T = L_H / V_H
$$


### 射撃シーケンス

射撃時のシーケンス図 (推測) を以下に示します。
最初に、クライアントで射撃操作を行うとメッセージがサーバに送られます。
サーバは射撃操作を受信した後、
クライアントに射撃イベントを送信し、
射撃イベントを受信したクライアントは、
射撃エフェクトや、飛翔体としての砲弾の描画などを行います。

次に、サーバ側で弾着の処理が行われると、
着弾した結果がクライアントに送られます。
クライアントは、着弾結果に応じて、
エフェクトの描画やメッセージの表示を行います。

![射撃シーケンス]({{site.baseurl}}/resources/fig_20200628_02_shot_sequence.svg)

図からわかるように、
射撃操作を行ってから結果を得るまでには、
弾着時間に加えて少なくとも ping の時間の遅れが生じます。
真の弾着時間を推測するには、
クライアントで観測される時間にこれらの影響が含まれていることを考慮しておく必要があります。


## 砲弾速度と目標までの距離の取得

`BigWolrd.player()` によって `PlayerAvatar` のインスタンスを得ることができます。
多くのプレイヤーの情報はこのインスタンスを起点として取得することができるようになっています。
ここでは取得したインスタンスを `avatar` として参照することにします。

メソッド `avatar.gunRotator.getCurShotPosition()` で
砲の起点に関する `shotPos`、 `shotVec` の2つのオブジェクトを取得できます。
`shotPos` は砲の起点の位置 (主砲の付け根になります)、
`shotVec` は砲弾の速度ベクトルです。


### 砲弾速度の水平成分

砲弾の速度ベクトルは3次元空間で定義されているので、
水平方向の成分は2次元ベクトルとなり、
大きさを求めるためには2次元ベクトルの大きさを求める計算が必要となります。

ベクトルクラスには水平方向の差分の大きさを求めるためのメソッド `flatDistTo` が用意されているので、
砲弾速度ベクトル `shotVec` の水平成分は
`shotVec.flatDistTo(Math.Vector3((0.0, 0.0, 0.0)))`
として求めることができます。

砲弾速度の内部値は[カタログ値の 80% になっている]({{site.baseurl}}/2020/01/20/shell_speed.html)ので、
カタログ値に対する速度水平成分を求めるときには、
求めた水平成分の値を 1.25 倍します。


### 目標までの距離の水平成分

目標の位置は、
クラス `_GunControlMode` のメソッド `updateGunMarker` の引数 `pos` として渡されるので、
フックをかけて取得しておきます。
ここではわかりやすいように `targetPos` として参照することにします。

`shotPos` から `targetPos` までの水平距離は位置ベクトルのメソッド `flatDistTo` を使用して
`shotPos.flatDistTo(targetPos)` のように求めることができます。


## 射撃時刻と弾着時刻の取得

実際の砲弾速度を計算するには、
砲弾が発射された時刻と弾着時刻を取得する必要があります。

ゲーム内時刻は `BigWorld.time()` で取得できるので、
あとは射撃と弾着それぞれに対応した処理を特定し、
そこにフックをかければ時刻を特定できます。

### 射撃操作時刻

クラス `Avatar.PlayerAvatar` の
メソッド `shoot` が射撃操作をしたときに呼び出されるので、
ここにフックをかけて時刻を取得します。
このメソッドは射撃不能な場合にも呼び出されるので
オリジナルと同様に射撃可能かどうかの判定をしておく必要があります。

なお、このメソッドはリプレイ時には呼び出されないことに注意が必要です。


### 射撃イベント受信時刻

クラス `vehicle.extras.ShowShooting` の
メソッド `__doShot` が射撃イベントを受信した際に呼び出されます。

このメソッドは、他人が射撃した場合にも呼び出され、
すべての射撃エフェクトの処理を行っています。
メソッドに渡される引数 `data` に詳細情報が含まれているので、
`data['entity'].isPlayerVehicle` が真のとき、
つまり自分が射撃したときにのみデータを記録するようにしておきます。

他人の射撃時にも呼び出されることから推測できるように、
このメソッドはサーバからのイベントで実行されます。
そのため、ローカルで射撃した時刻から少し (ping の2倍 + α) 遅れる性質があります。


### 弾着時刻

射撃結果イベントは、砲弾が外れたときなどは送られてこないので、
すべての弾着時刻を捕捉することはできなさそうです。

視認している敵の車輌に当たったときには、
貫通、非貫通に関わらず結果のイベントがサーバーから送られてくるので、
射撃結果イベント受信をクライアント側での弾着時刻とみなせます。
また、至近弾やスタンの場合も同様です。

このように、砲弾が外れたときの弾着時刻を知ることはできませんが、
そのときは弾着地点も当初の目標とは異なるため、
今回の目的である砲弾速度の計測に関しては無視するのでよいでしょう。

クラス `Avatar.PlayerAvatar` の
メソッド `showShotResults` に射撃結果のイベント情報が渡されます。
イベント情報にはどの車輌 (味方を含みます) に当たって何が起きたか (貫通や跳弾など) が記録されています。


## 砲弾速度の実測と評価

### データの取得

クライアントバージョン 1.9.1.1、ASIA サーバ (HK) の環境で射撃データの収録を行いました。
使用した車種は LT, MT, TD、SPG、Tier は 6 ～ 9 です。
土曜日の昼間に収録を実施し、そのときの ping は 60 ～ 70 ms くらいでした。

合計 166 回の射撃データを記録しています。


### 射撃イベント

射撃操作がサーバに送られてから、
サーバから射撃情報が送られてくるまでにラグが生じます。
ラグの分布がどのようになっているかを確認しておきます。

横軸にラグの時間、縦軸に頻度をとったヒストグラムを描いてみます。

![射撃イベント受信時間]({{site.baseurl}}/resources/fig_20200627_01_time_shot_event.svg)

0.13 秒以下のデータを外れ値として除外すると、
データ数 155、
平均 0.133、標準偏差 0.000761 で
ヒストグラムからもわかるようにばらつきはあまりありません。
平均値 0.133 秒は ping の約 2 倍と考えるとつじつまがあいます。


### 射撃結果 (弾着) イベント

射撃結果イベントもサーバから送られてくるイベントです。
目標車輌との距離が一定でないので射撃から弾着の時刻は大きくばらつきます。

射撃結果イベント受信時間の分布は下図のようになっています。

![射撃結果イベント受信時間]({{site.baseurl}}/resources/fig_20200627_02_time_shot_result_event.svg)

射撃イベント受信から射撃結果イベント受信までの時間にすると下図のようになります。

![射撃結果イベント受信時間差分]({{site.baseurl}}/resources/fig_20200627_05_diff_shot_result_event.svg)

なお、射撃イベントと弾着イベントが同時に到着したケースは 166 件中 4 件ありました。
至近距離だとそのような処理になるようです。


### 砲弾速度の検証

砲弾速度と目標までの距離、時間との関係は、
おおむね下式のようになっていると考えられます。

$$
(T_r - T_0) = L_H / V_H + (T_s - T_0)/2 + \Delta t
$$

ただし、
$T_0$: 射撃時刻 (s)、
$T_s$: 射撃イベント受信までの時間 (s)、
$T_r$: 射撃結果イベント受信時刻 (s)、
$L_H$: 目標までの水平距離 (m)、
$V_H$: 砲弾速度の水平成分 (m/s)、
$\Delta t$: その他の要因によるずれ (s)

サーバとの通信による遅れの補正を往復のラグ $(T_s - T_0)$ でなく
片道の $(T_s - T_0)/2$ としているのは、
射撃時間はサーバ側で補正されるとの仮定によります。

前節で示した射撃データを元に、
弾速がカタログスペック通り (100%) と仮定して求めたずれ時間 $\Delta t$、
つまり、弾着予想時間と計測結果との差を示します。
グラフは予想よりも計測結果の方が長い場合を正として描画しています。

![弾着予想時間との差 (弾速100%仮定)]({{site.baseurl}}/resources/fig_20200627_04_check-speed-100.svg)

弾速を 100% と仮定した場合の平均は 0.197 で、標準偏差は 0.181 です。
平均値は射撃時間補正の影響が含まれているので評価は保留しておいて、
ばらつきの方に着目することにします。

ヒストグラムを見ると、おおむね -0.1 秒から 0.7 秒くらいにばらついており、
射撃後に目標が移動する影響があることを考慮してもばらつきが大きいように思われます。

次に、弾速がクライアント内部の値と同様に、カタログスペックの 80% になっていると仮定した場合の結果を示します。

![弾着予想時間との差 (弾速80%仮定)]({{site.baseurl}}/resources/fig_20200627_03_check-speed-080.svg)

弾速を 80% と仮定した場合は、平均が -0.0246 で標準偏差が 0.0757 となっており、
100% の場合と比べてばらつきが小さく、平均が 0 に近づいています。
ばらつきが小さくなっていることから、弾速は 80% である可能性が高いと考えられ、
平均値が 0 に近くなっていることから、仮定した射撃時間の補正モデルがおおむね妥当であったことがわかります。


## まとめ

WoT クライアントで射撃時間と弾着時間を測定し、砲弾速度との比較を行った結果、
クライアント内部の値と同様に、砲弾速度をカタログスペックの 80% と仮定した場合に
予測値と実測値がよく一致することがわかりました。

これらの結果から、WoT の砲弾速度は、カタログスペックの 80% になっていると考えられます。