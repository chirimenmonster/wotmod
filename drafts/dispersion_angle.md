---
layout: page
title: 照準の拡散と収束
sitemap: false
mathjax: true
---
**(作成中)**

WoT の照準円は、車体の移動や射撃などによって拡散し、また、時間とともに一定量まで収束します。
照準円の拡散と収束のルール (計算方法) についてまとめました。

## 主砲精度のカタログ値と照準円の拡散係数

主砲には固有の精度を表す値が定められています。
カタログ値では 100m 先での照準円の直径として表現されていますが、
クライアント内部では VehicleDescriptor の
プロパティ `gun['shotDispersionAngle']`
として表現されています。
これは定数になります。

照準円はこのプロパティに対する変動する拡散係数で管理されます。

拡散係数には、
現在の照準を表す拡散係数 (aiming factor) と、
車輌の状態に対する理想的な拡散係数 (ideal factor) があります。

ideal factor は収束が完了した状態に対する拡散係数ですが、
車輌の動作や砲撃によって変動する係数です。
例えば車輌が一定速度で移動しているときの ideal factor は一定の値となりますが、
静止状態と比べると大きい値となっています。

aiming factor は時間とともに一定割合で収束する拡散係数です。
最小値は ideal factor で、ideal factor に達すると収束は止まります。
車輌の動きなどによって ideal factor が増加して aiming factor を超えた場合、
aiming factor も ideal factor と同じ値になります。
言い換えると、ideal factor が車輌の停止などによって減少すると、
aiming factor の収束が開始されることになります。


### Aiming Factor

照準の収束計算の結果から求められる、現時点での照準拡散係数です。

aiming factor の下限値は ideal factor で、その値になると収束は終了します。

収束が開始するときの初期値は $f_{start}$ で、時間とともに減少します。

$$
f_{aim} = f_{start} \cdot e^{(T_{start} - T_{current}) / T_{aim}'}
$$

$f_{aim}$: 照準拡散係数 (aiming factor),
$f_{start}$: 収束開始時の照準拡散係数,
$T_{start}$: 収束開始時の時間 (s),
$T_{current}$: 現在の時間 (s),
$T_{aim}'$: スキルなどを考慮した照準時間
 

時間 $T_{aim}'$ が経過したとき、$e^{-1} = 0.367879$ なので $f_{aim} = 0.367879\cdot f_{start}$ になります。


### Ideal Factor

車輌の移動などを考慮した、最も収束した場合の照準拡散係数です。
静止状態であれば 1.0 となります　($F_d$ が 1.0 の場合)。

$$
f_{ideal} = F_d \cdot \sqrt{1 + \{(v \cdot F_m)^2 + (\omega_c \cdot F_r)^2 + (\omega_t \cdot F_t)^2 + (F_s)^2 \} \cdot (F_a)^2}
$$

$f_{ideal}$: 収束完了時の照準拡散係数 (ideal factor),
$F_d$: `shotDispMultiplierFactor`,
$F_a$: `additiveFactor` (`__getAdditiveShotDispersionFactor` で取得) (要調査)
$v$: 移動速度 `vehicleSpeed`,
$F_m$: 移動時拡散係数 `chassisShotDispersionFactorsMovement`
$\omega_c$: 車体旋回速度 `vehicleRSpeed`,
$F_r$: 車体旋回時拡散係数 `chassisShotDispersionFactorsRotation`
$\omega_t$: 砲塔旋回速度 `turretRotationSpeed`,
$F_t$: 砲塔旋回時拡散係数 `gunShotDispersionFactorsTurretRotation`
$F_s$: 砲撃時拡散係数

## getOwnVehicleShotDispersionAngle

`getOwnVehicleShotDispersionAngle` は
`scripts/client/Avatar.py` で定義されている
クラス `PlayerAvatar` の
メンバ関数です。

この関数は、
自車輌の VehicleDescriptor の
プロパティ `gun['shotDispersionAngle']`
に対して `aimingFactor`, `idealFactor` を乗じた値を返します。
`gun['shotDispersionAngle']` は主砲のレティクルが完全に収束したときの拡散の角度です。

```python
return [descr.gun['shotDispersionAngle'] * aimingFactor, descr.gun['shotDispersionAngle'] * idealFactor]
```

### 移動速度

$$
f_m = ( v \cdot F_m ) ^2
$$

$v$: 移動速度 `vehicleSpeed`,
$F_m$: 移動時拡散係数 `chassisShotDispersionFactorsMovement`

### 車体旋回速度

$$
f_r = ( \omega_c \cdot F_r ) ^2
$$

$\omega_c$: 車体旋回速度 `vehicleRSpeed`,
$F_r$: 車体旋回時拡散係数 `chassisShotDispersionFactorsRotation`

### 砲塔旋回速度

$$
f_t = ( \omega_t \cdot F_t )^2
$$

$\omega_t$: 砲塔旋回速度 `turretRotationSpeed`,
$F_t$: 砲塔旋回時拡散係数 `gunShotDispersionFactorsTurretRotation`


### 砲撃

$$
f_s = (F_s) ^2
$$

$F_s$: 砲撃時拡散係数

砲撃時拡散係数は、射撃後の場合 `shotDispersionFactors['afterShot']` が使用され、
主砲破損時には `shotDispersionFactors['afterShotInBurst']` が使用されます。 


### additiveFactor

aimingBooster がないときは VehicleDescriptor のプロパティ `miscAttrs['additiveShotDispersionFactor']` が使用されます。

aimingBooster があるときは
`self.__aimingBooster.updateVehicleAttrFactors(descriptor, factors, None)`
で値が更新されます (要調査)。

aimingBooster はスタビライザー `aimingStabilizerBattleBooster`
を装備している場合に有効になります
(メソッド `__processVehicleEquipments` での処理)。




## updateTargetingInfo

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
|shotDispMultiplierFactor|`__aimingInfo[2]`|砲手スキルによる拡散係数|
|gunShotDispersionFactorsTurretRotation|`__aimingInfo[3]`|砲塔回転時の拡散係数|
|chassisShotDispersionFactorsMovement|`__aimingInfo[4]`|移動時の拡散係数|
|chassisShotDispersionFactorsRotation|`__aimingInfo[5]`|車体旋回時の拡散係数|
|aimingTime|`__aimingInfo[6]`|照準時間|

### shotDispMultiplierFactor

砲手スキルによる拡散係数です。

$$
F_d = \frac{0.875}{0.5 + 0.00375\cdot S_{gunner}}
$$

$F_d$: 拡散係数、
$S_{gunner}$: 砲手のプライマリスキル

砲手スキル補正 (車長補正、換気扇で 115.5) での理論値は 0.937709310114 になります(ただし内部データは 0.9375146627430 となっていました)。

$$
\frac{0.875}{0.5 + 0.00375\times 115.5} = 0.937709310114
$$

### gunShotDispersionFactorsTurretRotation

砲塔回転時の拡散係数です。


### chassisShotDispersionFactorsMovement

移動時の拡散係数です。


### chassisShotDispersionFactorsRotation

車体旋回時の拡散係数です。


### aimingTime

照準時間です。
拡散円の大きさが $e^{-1} = 0.367879$ 倍になるのに必要な時間を表します。
カタログ値の照準時間に対して砲手のプライマリスキルの補正が入ります。

$$
T_{aim}' = T_{aim}\cdot\frac{0.875}{0.5 + 0.00375\cdot S_{gunner}}
$$

$T_{aim}'$: 照準時間、
$T_{aim}$: 照準時間 (カタログ値)、
$S_{gunner}$: 砲手のプライマリスキル

スペック上の IS-7 の照準時間は 3.1秒なので、
砲手スキル補正 (車長補正、換気扇ありで 115.5) で 2.906898861 になります(ただし内部データは 2.90629529953 となっていました)。

$$
T'_{aim} = 3.1\times\frac{0.875}{0.5+0.00375\times 115.5} = 2.906898861
$$



## gunRotator

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
コールバックとして設定したメソッド `__onTick` で現在の照準拡散が計算されます。


## 要確認項目

+ additiveFactor: おそらくスタビライザーの有無による係数
+ shotDispMulitiplierFactor: 砲手のプライマリスキル?
+ aimingTime: $T_{aim}$ 砲手のプライマリスキル?、改良型射撃装置 (ガンレイ) ?
+ gunShotDispersionFactorsTurretRotation: 速射 (スナップショット) スキルと関係?
+ chassisShotDispersionFactorsMovement： スムーズな運転 スキルと関係?
