---
layout: page
title: 接地抵抗　(terrain resistance)
mathjax: true
sitemap: false
---

## 出力 (power)
出力 (power) はエンジンのパラメータとして定義されています。
クライアント内部の定義や tankopedia などで確認できる出力の単位は hp (horse power: 馬力) です。

物理量としては仕事率に相当し、
単位時間内のエネルギーを表します。

馬力には仏馬力と英馬力の2種があって、
WoT でどちらの馬力を使用しているのかは確認できていないのですが、
tankopedia での単位表記が "hp" となっているので、
英馬力が単位として使用されているものと仮定して話を進めることにします。
ちなみに wikipedia によれば
1 仏馬力 (PS) = 約 0.98632 英馬力 (HP)
です。


## 重量 (weight)
重量 (weight) は車体を始めとする各モジュールの重量の合計です。

## 接地抵抗 (terrain resistance)
接地抵抗 (terrain resistance) は履帯のパラーメータとして定義されています。
地形の種別、hard, medium, soft の3種類それぞれに対して
接地抵抗の値が定義されています。
これらの値はエンジンの出力に対する低減係数として扱われます。

## 摩擦係数 (specific friction)
摩擦係数 (specific friction) は共通パラメータです。
クライアント内部では定数 `DEFAULT_SPECIFIC_FRICTION` として
次のように定義されています。

```python
DEFAULT_SPECIFIC_FRICTION = 0.6867000000000001
```

これは $0.6867 = 0.07 \times 9.81$ で、
摩擦係数が 0.07 であることを表しています。

## 出力重量比 (power weight ratio)
重量に対する出力の比です。
日本の自動車業界で用いられるパワーウェイトレシオとは逆数の関係にあります。


## 加速度

仕事率 $P$、仕事 $W_p$、時間 $t$ との関係

$$
P = \frac{dW_p}{dt}
$$

加速度 $a$、速度 $v$、運動エネルギー $W_a$、時間 $t$ の関係

$$
W_a = \frac{1}{2} mv^2
$$

$$
\frac{dW_a}{dt} = mv\cdot \frac{dv}{dt} = mv\cdot a
$$


摩擦によるエネルギー損失 $W_f$,
動摩擦係数 $\mu'$,
質量 $m$,
重力加速度 $g$,
移動距離 $x$

$$
W_f = \mu' \cdot mg \cdot x
$$

$$
\frac{dW_f}{dt} = \mu' \cdot mg \cdot \frac{dx}{dt}
= \mu' \cdot mg \cdot v
$$

まとめると

$$
\frac{P}{m} = \left( a + \mu' g \right) \cdot v
$$

### 計算例

フランス Tier 4 軽戦車の AMX 40 を例に計算してみます。
車輌の制限速度に対してエンジンが非力なので、
最高速度は車輌の制限速度ではなくエンジンのパワーで決まります。

最高速度はエンジンが最大出力にも関わらず、
加速度が 0 となっている状態です。
つまり、下式のようになります。

$$
\frac{P}{m} = \mu'g \cdot v
$$

AMX 40 のエンジン最大出力は 190 hp,
地形 hard の接地抵抗が 1.4,
総重量が 21.86 t,
動摩擦係数と重力加速度の積が 0.6867 なので、
下式のように最高速度を 24.27 km/h と求めることができます。

$$
v = \frac{(190 /1.4) \times 745.7}{21860 \times 0.6867} \times \frac{3600}{1000} = 24.27
$$

ここでは
1 英馬力 (hp) = 745.7 ワット (W)
として計算しました　(正確には 745.69987158227022 ワット)。


## トルクと加速度

トルク (Nm) $T$,
回転速度 (rpm) $n$,
出力 (W) $P$
の関係

$$
P = \frac{2\pi N}{60}\cdot T
$$

車輌速度 (km/h) $v$,
駆動輪半径 (m) $r$,
回転速度 (rpm) $N$,
変速比 transmission ratio $R_t$
減速比 drive ratio $R_d$
の関係

$$
v = \frac{2\pi r \cdot N / 60}{R_t \cdot R_d} \cdot \frac{3600}{1000}
$$

車輌質量 (kg) $m$,
加速度 (m/s<sup><small>2</small></sup>) $a$

$$
m (a + \mu'g) = F = \frac{T \cdot R_t \cdot R_d}{r}
$$

AMX40 のエンジン回転数 2000 - 2500 rpm のとき、
出力 190 hp で一定

2000 rpm のとき

$$
T = 190 \times 745.7 \times \frac{60}{2 \times 3.14 \times 2000} = 676.8
$$

2500 rpm のとき

$$
T = 190 \times 745.7 \times \frac{60}{2 \times 3.14 \times 2500} = 541.5
$$


## scripts/common/items/vehicles.py

```python
            defResistance = chassis.terrainResistance[0]
            rotationEnergy = type.engines[0].power * (weight / defWeight) / (chassis.rotationSpeed * defResistance)
            rotationSpeedLimit = physics['enginePower'] / (rotationEnergy * physics['terrainResistance'][0])
            if not chassis.rotationIsAroundCenter:
                rotationEnergy -= trackCenterOffset * weight * chassis.specificFriction / defResistance
                if rotationEnergy <= 0.0:
                    raise Exception('wrong parameters of rotation of ' + type.name)
            if chassis.rotationSpeedLimit is not None:
                rotationSpeedLimit = min(rotationSpeedLimit, chassis.rotationSpeedLimit)
            physics['rotationSpeedLimit'] = rotationSpeedLimit
            physics['rotationEnergy'] = rotationEnergy
            physics['massRotationFactor'] = defWeight / weight
```