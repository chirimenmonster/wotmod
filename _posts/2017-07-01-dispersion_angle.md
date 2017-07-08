---
layout: post
title: 照準の拡散と収束
date: 2017-07-01 07:50 +0900
last_modified_at: 2017-07-08 13:00 +0900
mathjax: true
---
WoT の照準円は、車体の移動や射撃などによって拡散し、また、時間とともに一定量まで収束します。
照準円の拡散と収束のルール (計算方法) についてまとめました。

この文書には一部推測による、未検証の情報が含まれています。


## 主砲精度のカタログ値と照準円の拡散係数

主砲には固有の精度を表す値が定められています。
カタログ値では 100m 先での照準円の半径として表現されていますが、
クライアント内部では VehicleDescriptor の
プロパティ `gun['shotDispersionAngle']`
として表現されており、
カタログ値の照準円半径から算出します。
これは定数になります。

照準円の大きさはこの定数値のプロパティに、静的および動的に変動する拡散係数を乗ずる形で求められます。

$$
d = f \cdot D
$$

* $D$: 主砲に対して決められている拡散角度の基準値 shotDispersionAngle (rad)
* $f$: 拡散係数
* $d$: 計算された拡散角度 (rad)

この計算は
`scripts/client/Avatar.py` で定義されている
クラス `PlayerAvatar` の
メンバ関数 `getOwnVehicleShotDispersionAngle` で行われています。

#### 拡散角度基準値 shotDispersionAngle

拡散角度の基準値です。カタログ値の 100m 時照準円半径に対応します。

$$
D = \tan^{-1}\left(\frac{R_{100}}{100}\right)
$$

* $D$: 拡散角度の基準値 (rad)
* $R_{100}$: 100m 時照準円半径のカタログ値 (m)

## 照準拡散係数 aiming factor と目標拡散係数 ideal factor

拡散係数には、
現在の照準の状態を表す照準拡散係数 (aiming factor) と、
収束した状態に対応する目標拡散係数 (ideal factor) があります。

目標拡散係数 ideal factor は収束が完了した状態に対する拡散係数ですが、
車輌の動作や砲撃によって変動します。
例えば車輌が一定速度で移動しているときの目標拡散係数は一定の値となりますが、
静止状態と比べると大きい値となっています。

照準拡散係数 aiming factor は時間とともに一定割合で収束する拡散係数です。
照準拡散係数の最小値は目標拡散係数 ideal factor で、目標拡散係数に達すると収束は止まります。
車輌の動きなどによって目標拡散係数が増加して照準拡散係数を超えた場合、
照準拡散係数も目標拡散係数と同じ値になります。
言い換えると、目標拡散係数が車輌の停止などによって減少したときに、
照準拡散係数の収束が開始されることになります。


### 照準拡散係数 aiming factor

照準の収束計算の結果から求められる、現時点での照準拡散係数です。

収束が開始するときの初期値は $f_{\rm start}$ で、時間とともに減少します。
aiming factor の下限値は ideal factor で、その値になると収束は終了します。

$$
f_{\rm aim} = f_{\rm start} \cdot e^{(t_{\rm start} - t_{\rm current}) / T_{\rm aim}'}
$$

* $f_{\rm aim}$: 照準拡散係数 (aiming factor)
* $f_{\rm start}$: 収束開始時の照準拡散係数
* $t_{\rm start}$: 収束開始時の時間 (s)
* $t_{\rm current}$: 現在の時間 (s)
* $T_{\rm aim}'$: スキルなどを考慮した照準時間 (s)
 

時間 $T_{\rm aim}'$ が経過したとき、$e^{-1} = 0.367879$ なので $f_{\rm aim} = 0.367879\cdot f_{\rm start}$ になります。
つまり、$T_{\rm aim}'$ は拡散円の大きさが現在の $0.367879$ 倍にまで収束するのに要する時間です。


#### 照準時間 aimingTime

照準時間です。
拡散円の大きさが $e^{-1} = 0.367879$ 倍になるのに必要な時間を表します。
カタログ値の照準時間に対して砲手のプライマリスキル、改良型射撃装置の補正が入ります。
砲手プライマリスキルの補正は `scripts/common/items/VehicleDescrCrew.py` で行われます。

$$
T_{\rm aim}' = T_{\rm aim}\cdot\frac{1}{0.57 + 0.43\cdot S_{\rm gunner}/100}\cdot E_{\rm gunlay}
$$

* $T_{\rm aim}'$: 照準時間 (砲手プライマリ、改良型射撃装置の効果)
* $T_{\rm aim}$: 照準時間 (カタログ値)
* $S_{\rm gunner}$: 砲手のプライマリスキル (%)
* $E_{\rm gunlay}$: 改良型射撃装置による補正係数

改良型射撃装置による補正係数は次の表のようになります。

|:--|:--|
|なし| 1.0 |
|改良型射撃装置| 0.91 (≒ 100/(100+10))|
|耐摩耗射撃装置| 0.89 (≒ 100/(100+12.5))|

##### 計算例

カタログ値での IS-7 の照準時間は 3.10 秒です。

砲手スキル補正 (車長補正、換気扇ありで 115.5) で 2.906295411 になります(ただし内部データは 2.90629529953 となっていました)。

$$
T'_{\rm aim} = 3.1\times\frac{1}{0.57 + 0.43\times 115.5/100} = 2.906295411
$$

さらに改良型射撃装置による補正 (+10%) がある場合は、 2.64472882389 になります(ただし内部データは 2.64472889900 となっていました)。

$$
T'_{\rm aim} = 3.1\times\frac{1}{0.57 + 0.43\times 115.5/100} \times 0.91 = 2.64472882389
$$


### 目標拡散係数 ideal factor

車輌の移動などを考慮した、最も収束した場合の照準拡散係数です。
車輌の状態はルート内の項で表現されており、完全に静止した状態であれば 1.0 となります。

$$
f_{\rm ideal} = P \cdot \sqrt{1 + \{(v \cdot F_v)^2 + (\omega_c \cdot F_r)^2 + (\omega_t \cdot F_t)^2 + (F_s)^2 \} \cdot B^2}
$$

* $f_{\rm ideal}$: 目標拡散係数 (ideal factor)
* $P$: 拡散乗数 (砲手プライマリスキルの効果) `shotDispMultiplierFactor`
* $F_v$: 移動時拡散係数 (スムーズな運転スキルの効果) `chassisShotDispersionFactorsMovement`
* $F_r$: 車体旋回時拡散係数 `chassisShotDispersionFactorsRotation`
* $F_t$: 砲塔旋回時拡散係数 (速射スキルの効果) `gunShotDispersionFactorsTurretRotation`
* $F_s$: 砲撃時拡散係数 `shotDispersionFactors`
* $B$: 拡散低減係数 (スタビライザーの効果) `additiveFactor`
* $v$: 移動速度 `vehicleSpeed` (m/s)
* $\omega_c$: 車体旋回速度 `vehicleRSpeed` (rad/s)
* $\omega_t$: 砲塔旋回速度 `turretRotationSpeed` (rad/s)


#### 拡散乗数 shotDispMultiplierFactor

拡散係数全体にかかる乗数です。砲手のプライマリスキルによる影響を受けます。

$$
P = \frac{1}{0.57 + 0.43\cdot S_{\rm gunner}/100}
$$

* $P$: 拡散乗数
* $S_{\rm gunner}$: 砲手のプライマリスキル (%)

##### 計算例

砲手スキル補正 (車長補正、換気扇で 115.5) での理論値は 0.937514648666 になります(ただし内部データは 0.9375146627430 となっていました)。

$$
\frac{1}{0.57 + 0.43\times 115.5/100} = 0.937514648666
$$


#### 移動時拡散係数 chassisShotDispersionFactorsMovement

移動時の拡散係数です。
操縦手のスムーズな運転スキルの効果を受けます。

カタログ値 (item_defs) との関係は以下のようになります。

$$
F_v = F_{cv}\cdot\frac{1}{0.27778} \cdot(1 - 0.04 \cdot \frac{S_{\rm smoothride}}{100})
$$

* $F_v$: 移動時拡散係数 (m/s に対する値, $0.27778 \fallingdotseq 1000/3600$) 
* $F_{cv}$: 移動時拡散係数のカタログ値 (km/h に対する値)
* $S_{\rm smoothride}$: スムーズな運転スキルの値 (%)

##### 計算例

O-I, スムーズな運転100%, 換気扇 の場合 (スムーズな運転100%が車長のスキル補正、換気扇の効果を受けて115.5%となります) の移動時拡散係数は、
カタログ値が 0.20 なので, 以下のように計算されます
(ただし内部データでは 0.686730504036 でした)。

$$
0.20 / 0.27778 \times (1 - 0.04 \times 115.5 / 100) = 0.6867305061560
$$


#### 車体旋回時拡散係数 chassisShotDispersionFactorsRotation

車体旋回時の拡散係数です。

カタログ値 (item_defs) との関係は以下のようになります。

$$
F_r = F_{cr} \cdot \frac{180}{\pi}
$$

* $F_r$: 車体旋回時拡散係数 (rad に対する値)
* $F_{cr}$: 車体旋回時拡散係数のカタログ値 (deg に対する値)


#### 砲塔旋回時拡散係数 gunShotDispersionFactorsTurretRotation

砲塔旋回時の拡散係数です。
砲手の速射スキルの効果を受けます。

カタログ値 (item_def) との関係は以下のようになります。

$$
F_t = F_{ct} \cdot \frac{180}{\pi} \cdot(1 - 0.075 \cdot \frac{S_{\rm snapshot}}{100})
$$

* $F_t$: 砲塔旋回時拡散係数 (rad に対する値、スキル補正後)
* $F_{ct}$: 砲塔旋回時拡散係数のカタログ値 (deg に対する値)
* $S_{\rm snapshot}$: 速射スキルの値 (%)

##### 計算例

IS-7, 速射100%, 換気扇 の場合 (速射100%が車長のスキル補正、換気扇の効果を受けて115.5%となります) の砲塔旋回時拡散係数は、
カタログ値が 0.08 なので以下のように計算されます
(ただし内部データでは 4.18660259247 でした)。

$$
0.08 \times 180 / \pi \times (1 - 0.075 \times 115.5 / 100) = 4.1866026090209
$$


#### 砲撃時拡散係数　shotDispersionFactors

砲撃後の精度低下を表現する拡散係数です。
主砲破損時の精度低下もこの係数で表現されます。

砲撃時拡散係数は状態に応じて以下の値を取ります。

|:---|:---|
|通常時| 0 |
|砲撃後| `shotDispersionFactors['afterShot']` |
|主砲破損時| `shotDispersionFactors['afterShotInBurst']`| 

##### 事例

IS-7 の afterShot, afterShotInBurst の値はいずれも 4.0、
Panther II では いずれも 3.5 が設定されていました (内部データによる値)。


#### 拡散低減係数 additiveFactor

スタビライザーなどの効果による拡張低減係数です。

|:--|:--|
|なし| 1.0 |
|砲垂直安定装置 (スタビライザー)| 0.8 |
|高性能安定装置| 0.75 |


## クライアントの内部データ

### updateTargetingInfo

`updateTargetingInfo` は戦闘開始時にクライアント内部から呼び出され、
拡散に関わる各種の内部データを設定します。
内部データは `updateTargetingInfo` の引数としてわたされ、
`PlayerAvatar` のインスタンス変数 `__aimingInfo` に格納します。
データの一覧を以下の表に示します。

|:---:|:---|:---|
|turretYaw|(gunRotator)|砲塔回転角|
|gunPitch|(gunRotator)|主砲仰俯角|
|maxTurretRotationSpeed|(gunRotator)|最大砲塔旋回速度|
|maxGunRotationSpeed|(gunRotator)|最大主砲回転速度|
|shotDispMultiplierFactor|`__aimingInfo[2]`|拡散乗数|
|gunShotDispersionFactorsTurretRotation|`__aimingInfo[3]`|砲塔旋回時拡散係数|
|chassisShotDispersionFactorsMovement|`__aimingInfo[4]`|移動時拡散係数|
|chassisShotDispersionFactorsRotation|`__aimingInfo[5]`|車体旋回時拡散係数|
|aimingTime|`__aimingInfo[6]`|照準時間|



### gunRotator

`gunRotator` は `PlayerAvatar` のプロパティで、
クラス `VehicleGunRotator.VehicleGunRotator` のインスタンスです。
戦闘開始時に `updateTargetingInfo` から呼びされ、内部のプロパティに主砲に関わる各種内部データを格納します。
データの一覧を以下の表に示します。

|:---:|:---|:---|
|turretYaw|砲塔回転角|
|gunPitch|主砲仰俯角|
|maxTurretRotationSpeed|最大砲塔旋回速度|
|maxGunRotationSpeed|最大主砲回転速度|

`SERVER_TICK_LENGTH` ごとに
コールバックとして設定したメソッド `__onTick` を起点に現在の照準拡散が計算されます。

