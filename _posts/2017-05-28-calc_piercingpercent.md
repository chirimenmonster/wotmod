---
layout: post
title: WoT 装甲貫通インジケータにおける装甲貫通率の計算
mathjax: true
date: 2017-05-28 08:00 +0900
last_modified_at: 2017-06-07 12:00 +0900
---
WoT の装甲貫通インジケータで使用されている装甲貫通率の算定方法をまとめました。

## 装甲貫通率の計算

### getShotResult

装甲貫通率の計算は
`scripts/client/AvatarInputHandler/gun_marker_ctrl.py` の
関数 `getShotResult` で行われています (WoT 0.9.19.0.1 の場合)。

`getShotResult` は判定結果として
クラス `SHOT_RESULT` 内で定義されている定数のいずれかを返します。
`SHOT_RESULT` は `scripts/client/AvatarInputHandler/aih_constants.py` で定義されています。

| 値 | 変数名 | 概要 |
|:---:|:---|:---|
| 0 | SHOT_RESULT.UNDEFINED | 判定対象外 (味方など) |
| 1 | SHOT_RESULT.NOT_PIERCED | 非貫通 |
| 2 | SHOT_RESULT.LITTLE_PIERCED　| やや貫通 |
| 3 | SHOT_RESULT.GREAT_PIERCED | ほぼ貫通 |


### 着弾距離の算定

着弾点 $X_{\text{hit}}$ と自車輌の位置 $X_{\text{me}}$ の差から着弾距離 $L$ を求めます。

$$
L = X_{\text{hit}} - X_\text{me}
$$

### 貫通力の算定

主砲の特性値として
距離 100m の貫通力 $P_{100}$、
距離 500m の貫通力 $P_{500}$、
最大到達距離 $L_{\text{max}}$
が定数として定められています。

このうち、
距離 100m の貫通力 $P_{100}$、
距離 500m の貫通力 $P_{500}$ から
距離 $L$ の貫通力 $P$ を求めます。

貫通力は距離に応じて減少するものとして、
距離 100m から最大到達距離 $L_{\text{max}}$ までの区間の貫通力 $P$ は線形補間によって求めます。
ただし、距離が 100m 以下の貫通力は $P_{100}$ で一定とし、
距離 $L$ が最大到達距離 $L_{\text{max}}$ を超える場合には貫通力は 0 とみなします。
最大到達距離のところで貫通力の値が不連続になる可能性に注意してください。

$$
P =
\begin{cases}
P_{100} & \text{($L \leq 100$)}\\
P_{100} + ( P_{500} - P_{100} ) \cdot (L - 100)/(500 - 100) & \text{($100 < L < L_{\text{max}}$)} \\
0 & \text{($L \geq L_{\text{max}}$)}
\end{cases}
$$


### 貫通率の算定

装甲厚 $t$ と貫通力 $P$ から貫通率 $R$ を求めます。
貫通力が装甲厚に等しい ($P = t$) とき、
貫通率 $R$ は 100 となり、
貫通力が高いほど 0 に近づきます。 

ここでの装甲厚は垂直方向の厚さであって避弾経始の効果は考慮されません。

$$
\begin{align}
R &= 100 + (t - P) / P \times 100　\\
&= t / P \times 100
\end{align}
$$

## 装甲貫通インジケータの画像の選択

### ShotResultIndicatorPlugin

装甲貫通インジケータの画像の選択は
クラス `ShotResultIndicatorPlugin` の
関数 `__updateColor` で行われています。
`ShotResultIndicatorPlugin` は
`scripts/client/gui/Scaleform/daapi/view/battle/shared/crosshair/plugins.py`
で定義されています (WoT 0.9.19.0.1 の場合)。

### 画像の選択

貫通率 $R$ の値によって、装甲貫通インジケータの評価が決まります。
評価は3段階で、評価に応じて装甲貫通インジケータの画像が選択されます。

| 貫通率 R | 評価 | default | color blind |
|:---:|:---:|:---:|:---:|
| 90 以下 | great pierced (ほぼ貫通) | green | green |
| 90 ～ 150 | little pierced (やや貫通) | orange | yellow |
| 150 以上 | not pierced (非貫通) | red | purple |
