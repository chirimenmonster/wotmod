---
layout: post
title: WoT 装甲貫通インジケータにおける装甲貫通率
mathjax: true
date: 2017-08-01 22:10 +0900
last_modified_at: 2017-08-12 17:45 +0900
---
WoT の装甲貫通インジケータで使用されている装甲貫通率の算定方法です。
0.9.19.1 で改定されました。

## はじめに

WoT の装甲貫通インジケータは 0.9.19.1 の改定で、
ほぼ実際の装甲貫通判定と同様の計算方式となりました
(発砲後の弾着までの時間差による標的の移動や、跳弾後の再衝突は考慮されません)。

具体的には跳弾判定、砲弾の入射角を考慮した有効装甲厚の計算、
空間装甲および HEAT が空間装甲を通過する場合の貫通力減衰などが考慮されています。

## 計算の流れ

最初に、以下の値が計算または取得されます。

* 距離減衰を考慮した弾着時貫通力の計算
* 貫通力のばらつきの上限および下限値の計算
* 砲弾の軌跡上に位置するオブジェクトのリストの取得

次に、砲弾の軌跡上に位置するオブジェクトに対し、距離の近い方から順に、
以下の判定が行われます。
貫通し、かつダメージを与えることがない場合 (空間装甲など) は、
装甲を貫通する場合は貫通力から装甲厚を減じ、
次に距離の近いオブジェクトに対して判定が行われます。
この処理はダメージを与えるか、貫通力を失うかするまで継続されます。

* 跳弾判定
* 貫通判定
* ダメージを与えるかどうか

貫通し、かつダメージを与える場合は、
貫通力のばらつきに対する貫通率が計算されます。
貫通インジケータの色はこの貫通率に対して設定されます。

これらの処理は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
クラス `_CrosshairShotResults` の `getShotResult` で行われます。

## 貫通力のばらつきと判定基準

砲弾の貫通力には上下 25% のばらつきが設定されていますが、
そのうちの半分、つまり 12.5% 下振れしても貫通可能な場合を GREAT_PIERCED、
逆に 12.5% 上振れしても貫通不能な場合を NOT_PIERCED、
その中間を LITTLE_PIERCED として、
貫通インジケータの表示を切り替えています。

|貫通率 $R$|評価|概要|default|color blind|
|:---|:---|:---|:---|:---|
|$R \le 87.5$|GREAT_PIERCED|下振れしても貫通可能|green|green|
|$87.5 < R < 112.5$|LITTLE_PIERCED|貫通可|orange|yellow|
|$R \ge 112.5$ または跳弾|NOT_PIERCED|上振れしても非貫通|red|purple|

$R$: 貫通率 (%) (100% を下回ると貫通)

貫通の判定基準値の計算は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
クラス `_CrosshairShotResults` の `_computePiercingPowerRandomization` で、
判定は同クラスの　`getShotResult` で行われます。


### 貫通判定基準の解釈

前述したように砲弾の貫通力は ±25% のばらつきが与えられます。
貫通力のばらつき分布は公式には発表されていませんが
[FTR の古い (2013年) 記事](http://ftr-wot.blogspot.jp/2013/03/1832013.html)
によれば照準のばらつきと異なり完全な乱数だそうです。

それを考慮して上記の貫通率の判定を整理すると
$R=87.5$ で貫通するケースには、貫通力のばらつきのうち 75% が含まれ、
$R=112.5$ で貫通しないケースには、貫通力のばらつきの 75% が含まれることになります。
言い換えると
$R \le 87.5$ は貫通可能性 75% 以上、
$R \ge 112.5$ は貫通可能性 25% 以下ということになります。

|貫通率 $R$|評価|貫通可能性|default|color blind|
|:---|:---|:---|:---|:---|
|$R \le 87.5$|GREAT_PIERCED|75%以上|green|green|
|$87.5 < R < 112.5$|LITTLE_PIERCED|25～75%|orange|yellow|
|$R \ge 112.5$ または跳弾|NOT_PIERCED|25%以下|red|purple|


### 貫通率 $R$

貫通率 $R$ は下式で計算されます。
装甲傾斜角を考慮した有効装甲厚 $T$ に対して
砲弾の貫通力 $P_e$ が等しいときに $R = 100$ となり、
貫通力が高いときは $R < 100$、
低い時は $R > 100$ です。

$$
R = 100 + \frac{T - P_e}{P_d} \times 100
$$

* $t_e$: 有効装甲厚 (mm)
* $P_e$: 貫通力 (mm)
* $P_d$: 距離減衰を考慮した弾着時貫通力 (mm)

貫通率の計算は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
クラス `_CrosshairShotResults` の `getShotResult` で行われます。


### 距離減衰を考慮した弾着時貫通力 $P_d$

主砲の特性値として
距離 100m の貫通力 $P_{100}$、
距離 500m の貫通力 $P_{500}$、
最大到達距離 $L_{\text{max}}$
が定数として定められています。

このうち、
距離 100m の貫通力 $P_{100}$、
距離 500m の貫通力 $P_{500}$ から
距離 $L$ の貫通力 $P_d$ を求めます。

貫通力は距離に応じて減少するものとして、
距離 100m から最大到達距離 $L_{\text{max}}$ までの区間の貫通力 $P_d$ は線形補間によって求めます。
ただし、距離が 100m 以下の貫通力は $P_{100}$ で一定とし、
距離 $L$ が最大到達距離 $L_{\text{max}}$ を超える場合には貫通力は 0 とみなします。
最大到達距離のところで貫通力の値が不連続になる可能性に注意してください。

$$
P_d =
\begin{cases}
P_{100} & \text{($L \leq 100$)}\\
P_{100} + ( P_{500} - P_{100} ) \cdot (L - 100)/(500 - 100) & \text{($100 < L < L_{\text{max}}$)} \\
0 & \text{($L \geq L_{\text{max}}$)}
\end{cases}
$$

* $P_{100}$: 距離 100m の貫通力カタログ値 (mm)
* $P_{500}$: 距離 500m の貫通力カタログ値 (mm)
* $L_{\text{max}}$: 最大到達距離 (内部設定値) (m)
* $L$: 着弾距離 (m)

弾着時貫通力の計算は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
クラス `_CrosshairShotResults` の `_computePiercingPowerAtDist` で行われます。


## 跳弾判定

AP、APCR, HEAT で砲弾の入射角が跳弾角度を超える場合は跳弾として処理されます。
ただし、AR, APCR で3倍ルールが適用される場合は跳弾せず、貫通判定が行われます。

跳弾判定は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
クラス `_CrosshairShotResults` の
`_shouldRicochet` で行われています。

|弾種| 跳弾角度 | 3倍ルール
|:--:|:--:|:---:|
|AP, APCR| 70° | あり
|HEAT| 85° | なし
|その他| --- | ---

### 3倍ルール

AP, APCR で砲弾の口径が命中箇所の装甲厚の3倍以上ある場合は跳弾と判定されず、
貫通判定に移行します。


## 有効装甲厚

有効装甲厚 $T$ は砲弾の入射角 $\alpha$ の方向に対する見かけの装甲厚です。
したがって、実装甲厚 $h$ と有効装甲厚 $T$ の関係は $T=h/\cos\alpha$ のようになりますが、
実際には後述するように標準化 (normalize) と呼ばれる入射角の補正が行われます。
標準化は装甲に対し垂直に近づくような方向への補正です。

有効装甲厚の計算は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
クラス `_CrosshairShotResults` の
`_computePenetrationArmor` で行われています。

### 有効装甲厚の計算と砲弾入射角の標準化 (normalize)

AP の場合で 5°、APCR の場合で 2° の標準化 (normalize) が行われます。
他の弾種では標準化は行われません。

|弾種| 標準化角度 $\beta$
|:--|:--:
|AP| 5°
|APCR| 2°
|その他| 0°

標準化を考慮した有効装甲厚の計算は下式のようになります。

$$
T = \frac{h}{\cos (\alpha - \beta)}
$$

* $T$: 有効装甲厚 (mm)
* $h$: 装甲厚 (mm)
* $\alpha$: 砲弾の入射角 (rad)
* $\beta$: 標準化角度 (rad) (後述の2倍ルールが適用される場合は補正後の $\beta_T$ を用いる)


### 標準化に対する2倍ルール

口径が装甲厚の2倍を超える場合には、標準化の角度は下式で補正されます。
公式の
[解説動画 (YouTube: World of Tanks PC - Explaining Mechanics - Armor Penetration)](https://youtu.be/UFktFSJZPsQ)
や
[解説ページ (週刊ヴィクトリヤ日記 Vol.32『砲弾と貫通のメカニズム 後編』)](https://worldoftanks.asia/ja/news/column/victoriadiary_032/)
の 1/2 になっているので注意してください。

$$
\beta_T = \frac{\beta \times 1.4 \times C}{2 \times h}
$$

* $\beta_T$: 2倍ルール適用後の標準化角度 (rad)
* $\beta$: 標準化角度 (rad)
* $C$: 口径 (mm)
* $h$: 装甲厚 (mm)


## 空間装甲

空間装甲がある場合、
外側の装甲に対する貫通判定が行われたあと、
残存する貫通力を用いて本装甲に対する計算が行われます。

空間装甲は1枚とは限らず、
砲弾が空間装甲を通過するたびに貫通力を減じていきます。

空間装甲に関する貫通力の計算は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
クラス `_CrosshairShotResults` の
`getShotResult` で行われています。

### 貫通力の減衰

砲弾が保持する貫通力 (mm) から有効装甲厚 (mm) を引いたものが
残存貫通力になります。

$$
P_r = P_e - T
$$

* $P_r$: 残存貫通力 (mm)
* $P_e$: 貫通力 (mm)
* $T$: 有効装甲厚 (mm)


### 空間装甲内の距離減衰 (HEAT のみ)

砲弾が HEAT の場合はメタルジェットの距離減衰効果が計算されます。
この距離減衰は10cmにつき5%の割合で貫通力が失われ、
下式で計算されます。

$$
P_r' = (1 - 0.5 \times d) \times P_r
$$

* $P_r'$: 減衰後の残存貫通力 (mm)
* $P_r$: 残存貫通力 (mm)
* $d$: 空間装甲の距離 (m)

空間装甲の距離は、
手前の装甲の裏から奥の装甲の表面までの長さです。


## 参考

* [週刊ヴィクトリヤ日記 Vol.31『砲弾と貫通のメカニズム 前編』](https://worldoftanks.asia/ja/news/column/victoriadiary_031/)
* [週刊ヴィクトリヤ日記 Vol.32『砲弾と貫通のメカニズム 後編』](https://worldoftanks.asia/ja/news/column/victoriadiary_032/)
* [YouTube: World of Tanks PC - Explaining Mechanics - Armor Penetration ](https://youtu.be/UFktFSJZPsQ)

