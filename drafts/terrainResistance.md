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

馬力には仏馬力と英馬力の2種がありますが、
後述のように WoT では仏馬力が採用されています。

馬力からワットへの換算は common/items/vehicles.py で、
ワットから馬力への換算は client/gui/shared/items_parameters/param.py などで確認できます。

```python
    item.power = _xml.readPositiveFloat(xmlCtx, section, 'power') * component_constants.HP_TO_WATTS
```

```python
    @property
    def enginePower(self):
        return int(round(self._itemDescr.power / component_constants.HP_TO_WATTS, 0))
```

馬力からワットへの換算係数 `HP_TO_WATTS` は common/items/components/component_conststnt.py で定義されており、
WoT では仏馬力が採用されていることが確認できます。

```python
HP_TO_WATTS = 735.5
```

車輌のマスクデータとして smplEnginePower というパラメータも定義されているのですが、
詳細は不明です。
client/gui/shared/items_parameters/param.py での使用箇所を見ると
限界速度 (制限速度ではなく速度の上限値) の計算に用いられているようです。

```python
    def __getRealSpeedLimit(self):
        enginePower = self.__getEnginePhysics()['smplEnginePower']
        rollingFriction = self.__getChassisPhysics()['grounds']['ground']['rollingFriction']
        return enginePower / self.vehicleWeight.current * METERS_PER_SECOND_TO_KILOMETERS_PER_HOUR * self.__factors['engine/power'] / 12.25 / rollingFriction
```

この中の 12.25 という値もよくわかりませんね。
本来であれば、ここには重力加速度として 9.81 が来るはずです。

この限界速度の値は relativeMobility の計算の一部に使用されます。


## 転がり摩擦 (rolling friction)

転がり摩擦 (rolling friction) は履帯 (chassis) のパラメータとして定義されています。
対象物には ground, sand, wood, snow, stone, metal がありますが、
現在 (0.9.22.0.1) では対象物によらず同じ値が設定されているようです。

値は履帯によって異なりますが、おおむね 0.1 前後です。


## 重量 (weight)

重量 (weight) は車体を始めとする各モジュールの重量の合計です。


## 接地抵抗 (terrain resistance)

接地抵抗 (terrain resistance) は履帯のパラーメータとして定義されています。
地形の種別、hard, medium, soft の3種類それぞれに対して
接地抵抗の値が定義されています。
これらの値はエンジンの出力に対する低減係数として扱われます。

接地抵抗のうち、
medium と soft については、
オフロードスキル、
グロウサーによって軽減されます。
common/items/VehicleDescrCrew.py,
common/items/artefacts.py


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

AMX 40 のエンジン最大出力は 190 hp （smplEnginePower が 200.8834 hp）,
接地抵抗が (hard, medium, soft) = (1.4, 1.5, 2.5),
総重量が 21.86 t,
動摩擦係数と重力加速度の積が 0.6867、
転がり摩擦係数 0.12075、
下式のように最高速度を 23.94 km/h と求めることができます。

$$
v = \frac{(190 /1.4) \times 735.5}{21860 \times 0.6867}
\times \frac{3600}{1000} = 23.94 ~\mathrm{(km/h)}
$$

$$
v = \frac{(200.8834 / 1.4) \times 735.5}{21860 \times 0.12075 \times 9.81}
\times \frac{3600}{1000} = 14.67 ~\mathrm{(km/h)}
$$

$$
v = \frac{(200.8834 / 1.5) \times 735.5}{21860 \times 0.12075 \times 9.81}
\times \frac{3600}{1000} = 13.69 ~\mathrm{(km/h)}
$$

$$
v = \frac{(200.8834 / 2.5) \times 735.5}{21860 \times 0.12075 \times 9.81}
\times \frac{3600}{1000} = 8.22 ~\mathrm{(km/h)}
$$


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
T = 190 \times 735.5 \times \frac{60}{2 \times 3.14 \times 2000} = 667.6 ~\mathrm{(Nm)}
$$

2500 rpm のとき

$$
T = 190 \times 735.5 \times \frac{60}{2 \times 3.14 \times 2500} = 534.1 ~\mathrm{(Nm)}
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