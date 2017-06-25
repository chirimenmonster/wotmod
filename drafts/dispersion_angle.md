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

