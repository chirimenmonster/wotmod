---
layout: page
title: WoT 装甲貫通インジケータにおける装甲貫通率 (2)
mathjax: true
---
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

