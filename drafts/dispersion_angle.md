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
|shotDispMultiplierFactor|`__aimingInfo[2]`||
|gunShotDispersionFactorsTurretRotation|`__aimingInfo[3]`|砲塔回転時の拡散係数|
|chassisShotDispersionFactorsMovement|`__aimingInfo[4]`|移動時の拡散係数|
|chassisShotDispersionFactorsRotation|`__aimingInfo[5]`|車体旋回時の拡散係数|
|aimingTime|`__aimingInfo[6]`|照準時間|


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

