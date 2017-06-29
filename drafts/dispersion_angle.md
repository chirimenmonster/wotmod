---
layout: page
title: 拡散
sitemap: false
mathjax: true
---
**(作成中)**

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


### idealFactor

車輌の移動などを考慮した、最も収束した場合の照準拡散係数です。
静止状態であれば 1.0 となります　($F_d$ が 1.0 の場合)。

$$
f_{ideal} = F_d \cdot \sqrt{1 + (f_m + f_r + f_t + f_s) \cdot (F_a)^2}
$$

$F_d$: `shotDispMultiplierFactor`,
$F_a$: `additiveFactor` (`__getAdditiveShotDispersionFactor` で取得) (要調査)



### aimingFactor

照準の収束計算の結果から求められる、現時点での照準拡散係数です。

計算の結果 $f_{ideal}$ を下回る場合には $f_{aim} = f_{ideal}$ に設定されます (つまり照準が収束しきったことを表します)。
同時に $f_{start} = f_{ideal}$ も設定されます。

収束が開始するときの初期値は $f_{start}$ で、時間とともに減少します。

$$
f_{aim} = f_{start} \cdot e^{(T_{start} - T_{current}) / T_{aim}'}
$$

時間 $T_{aim}'$ が経過したとき、$e^{-1} = 0.367879$ なので $f_{aim} = 0.367879\cdot f_{start}$ になります。



## updateTargetingInfo

`updateTargetingInfo` は戦闘開始時にクライアント内部から呼び出され、
拡散に関わる各種の内部データを設定します。
内部データは `updateTargetingInfo` の引数としてわたされ、
`PlayerAvatar` のインスタンス変数 `__aimingInfo` に格納します。
データの一覧を以下の表に示します。

|:---:|:---|:---|
|turretYaw|(gunRotator 用データ)|砲塔回転角|
|gunPitch|(gunRotator 用データ)|主砲仰俯角|
|maxTurretRotationSpeed|(gunRotator 用データ)|最大砲塔旋回速度|
|maxGunRotationSpeed|(gunRotator 用データ)|最大主砲回転速度|
|shotDispMultiplierFactor|`__aimingInfo[2]`|砲手スキルによる拡散係数|
|gunShotDispersionFactorsTurretRotation|`__aimingInfo[3]`|砲塔回転時の拡散係数|
|chassisShotDispersionFactorsMovement|`__aimingInfo[4]`|移動時の拡散係数|
|chassisShotDispersionFactorsRotation|`__aimingInfo[5]`|車体旋回時の拡散係数|
|aimingTime|`__aimingInfo[6]`|照準時間|

### shotDispMultiplierFactor

砲手スキルによる拡散係数です。

$$
F_d = \frac{0.875}{0.5 + 0.00375\cdot _cS}
$$

砲手スキル補正 (車長補正、戦友ありで 115.5) での理論値は 0.937709310114 になります(ただし内部データは 0.9375146627430 となっていました)。

$$
\frac{0.875}{0.5 + 0.00375\times 115.5} = 0.937709310114
$$


### aimingTime

照準時間です。

$$
T'_{aim} = T_{aim}\cdot\frac{0.875}{0.5 + 0.00375\cdot _cS}
$$

スペック上の IS-7 の照準時間は 3.1秒なので、
砲手スキル補正 (車長補正、戦友ありで 115.5) で 2.906898861 になります(ただし内部データは 2.90629529953 となっていました)。

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
