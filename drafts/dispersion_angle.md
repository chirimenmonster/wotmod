---
layout: page
title: 照準の拡散と収束
sitemap: false
mathjax: true
---
**(作成中)**

WoT の照準円は、車体の移動や射撃などによって拡散し、また、時間とともに一定量まで収束します。
照準円の拡散と収束のルール (計算方法) についてまとめました。

この文書には一部推測による、未検証の情報が含まれています。


## 主砲精度のカタログ値と照準円の拡散係数

主砲には固有の精度を表す値が定められています。
カタログ値では 100m 先での照準円の直径として表現されていますが、
クライアント内部では VehicleDescriptor の
プロパティ `gun['shotDispersionAngle']`
として表現されています。
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

収束が開始するときの初期値は $f_{start}$ で、時間とともに減少します。
aiming factor の下限値は ideal factor で、その値になると収束は終了します。

$$
f_{aim} = f_{start} \cdot e^{(t_{start} - t_{current}) / T_{aim}'}
$$

* $f_{aim}$: 照準拡散係数 (aiming factor)
* $f_{start}$: 収束開始時の照準拡散係数
* $t_{start}$: 収束開始時の時間 (s)
* $t_{current}$: 現在の時間 (s)
* $T_{aim}'$: スキルなどを考慮した照準時間 (s)
 

時間 $T_{aim}'$ が経過したとき、$e^{-1} = 0.367879$ なので $f_{aim} = 0.367879\cdot f_{start}$ になります。
つまり、$T_{aim}'$ は拡散円の大きさが現在の $0.367879$ 倍にまで収束するのに要する時間です。


#### 照準時間 aimingTime

照準時間です。
拡散円の大きさが $e^{-1} = 0.367879$ 倍になるのに必要な時間を表します。
カタログ値の照準時間に対して砲手のプライマリスキルの補正が入ります。

$$
T_{aim}' = T_{aim}\cdot\frac{0.875}{0.5 + 0.00375\cdot S_{gunner}}
$$

* $T_{aim}'$: 照準時間
* $T_{aim}$: 照準時間 (カタログ値)
* $S_{gunner}$: 砲手のプライマリスキル

スペック上の IS-7 の照準時間は 3.1秒なので、
砲手スキル補正 (車長補正、換気扇ありで 115.5) で 2.906898861 になります(ただし内部データは 2.90629529953 となっていました)。

$$
T'_{aim} = 3.1\times\frac{0.875}{0.5+0.00375\times 115.5} = 2.906898861
$$



### 目標拡散係数 ideal factor

車輌の移動などを考慮した、最も収束した場合の照準拡散係数です。
車輌の状態はルート内の項で表現されており、完全に静止した状態であれば 1.0 となります。

$$
f_{ideal} = P \cdot \sqrt{1 + \{(v \cdot F_v)^2 + (\omega_c \cdot F_r)^2 + (\omega_t \cdot F_t)^2 + (F_s)^2 \} \cdot (F_a)^2}
$$

* $f_{ideal}$: 目標拡散係数 (ideal factor)
* $P$: 拡散乗数 (砲手のプライマリスキルの効果) `shotDispMultiplierFactor`
* $F_v$: 移動時拡散係数 `chassisShotDispersionFactorsMovement`
* $F_r$: 車体旋回時拡散係数 `chassisShotDispersionFactorsRotation`
* $F_t$: 砲塔旋回時拡散係数 `gunShotDispersionFactorsTurretRotation`
* $F_s$: 砲撃時拡散係数 `shotDispersionFactors`
* $F_a$: 拡散低減係数 (スタビライザーの効果) `additiveFactor` (要調査)
* $v$: 移動速度 `vehicleSpeed` (m/s)
* $\omega_c$: 車体旋回速度 `vehicleRSpeed` (rad/s)
* $\omega_t$: 砲塔旋回速度 `turretRotationSpeed` (rad/s)


#### 拡散乗数 shotDispMultiplierFactor

拡散係数全体にかかる乗数です。砲手のプライマリスキルによる影響を受けます。

$$
P = \frac{0.875}{0.5 + 0.00375\cdot S_{gunner}}
$$

* $P$: 拡散乗数
* $S_{gunner}$: 砲手のプライマリスキル

砲手スキル補正 (車長補正、換気扇で 115.5) での理論値は 0.937709310114 になります(ただし内部データは 0.9375146627430 となっていました)。

$$
\frac{0.875}{0.5 + 0.00375\times 115.5} = 0.937709310114
$$


#### 移動時拡散係数 chassisShotDispersionFactorsMovement

移動時の拡散係数です。


#### 車体旋回時拡散係数 chassisShotDispersionFactorsRotation

車体旋回時の拡散係数です。


#### 砲塔旋回時拡散係数 gunShotDispersionFactorsTurretRotation

砲塔旋回時の拡散係数です。



#### 砲撃時拡散係数　shotDispersionFactors

砲撃後の精度低下を表現する拡散係数です。
主砲破損時の精度低下もこの係数で表現されます。

砲撃時拡散係数は状態に応じて以下の値を取ります。

|:---|:---|
|通常時| 0 |
|砲撃後| `shotDispersionFactors['afterShot']` |
|主砲破損時| `shotDispersionFactors['afterShotInBurst']`| 


#### 拡散低減係数 additiveFactor

aimingBooster がないときは VehicleDescriptor のプロパティ `miscAttrs['additiveShotDispersionFactor']` が使用されます。

aimingBooster があるときは
`self.__aimingBooster.updateVehicleAttrFactors(descriptor, factors, None)`
で値が更新されます (要調査)。

aimingBooster はスタビライザー `aimingStabilizerBattleBooster`
を装備している場合に有効になります
(メソッド `__processVehicleEquipments` での処理)。


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


## 要確認項目

+ additiveFactor: おそらくスタビライザーの有無による係数
+ aimingTime: 改良型射撃装置 (ガンレイ) による効果の有無
+ gunShotDispersionFactorsTurretRotation: 速射 (スナップショット) スキルと関係?
+ chassisShotDispersionFactorsMovement： スムーズな運転 スキルと関係?
