---
layout: page
title: エンジン出力と最大速度
mathjax: true
sitemap: false
---
WoT の車輌には最大速度が設定されていますが、
この最大速度とは実は速度のリミッターのこと。
AMX40 など最大速度に比べてエンジンが非力な車輌では最大速度には届きません。
そのような場合の実際の最大速度の求め方を解説します。

## 物理モデル

WoT の物理モデルの詳細は公表されていませんが、
車輌のエンジンには出力があり、各種の摩擦の概念があります。
出発点としては、おおむね、実際の物理法則を単純化したところからと仮定してよいでしょう。
仮定して推定した結果が一致しなければそのときに仮定を再検討すればよいのです。


## エンジン出力 (engine power)

エンジン出力 (engin power) はエンジンのパラメータとして定義されています。
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


## 重量 (weight)

重量 (weight) は車体を始めとする各モジュールの重量の合計です。

クライアント内部での単位は kg が使用されています。

重量による鉛直方向の力 kgf から N への換算係数 `KG_TO_NEWTON` は common/items/components/component_conststnt.py で定義されており、
重力加速度として 9.81 m/s<sup><small>2</small></sup> が使用されていることがわかります。

```python
KG_TO_NEWTON = 9.81
```


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

```python
        level = level + levelIncrease
        r = self._terrainResistanceFactors
        r[1] *= max(0.001, 1.0 - level * skillConfig.mediumGroundResistanceFactorPerLevel)
        r[2] *= max(0.001, 1.0 - level * skillConfig.softGroundResistanceFactorPerLevel)
```

```python
        r = vehicleDescr.physics['terrainResistance']
        vehicleDescr.physics['terrainResistance'] = (r[0], r[1] * self.__factorMedium, r[2] * self.__factorSoft)
```

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


## 最大速度の導出

仕事率 (エンジン出力) $P$、仕事 (エンジンから得られるエネルギー) $W_p$、時間 $t$ の関係は
次のようになります。

$$
P = \frac{dW_p}{dt}
$$

運動エネルギー $W_k$、質量 $m$、速度 $v$、加速度 $a$、時間 $t$ の関係は
次のようになります。

$$
W_k = \frac{1}{2} mv^2
$$

$$
\frac{dW_k}{dt} = mv\cdot \frac{dv}{dt} = mv\cdot a
$$


摩擦によるエネルギー損失 $W_f$、
動摩擦係数 $\mu'$、
質量 $m$、
重力加速度 $g$、
移動距離 $x$
の関係は
次のようになります。

$$
W_f = \mu' \cdot mg \cdot x
$$

$$
\frac{dW_f}{dt} = \mu' \cdot mg \cdot \frac{dx}{dt}
= \mu' \cdot mg \cdot v
$$

エンジンから得られるエネルギー $W_p$、
車輌の運動エネルギー $W_k$、
摩擦によるエネルギー損失 $W_f$
には保存則が成り立ちます。

$$
\frac{d W_p}{dt} = \frac{d W_k}{dt} + \frac{d W_f}{dt}
$$

まとめると次のようになります。

$$
\frac{P}{m} = \left( a + \mu' g \right) \cdot v
$$

最大速度となるときは、
出力 $P$ が最大、
$a = 0$
のときなので、
最大速度は次のようになります。

$$
v_\max = \frac{P_\max}{m}\cdot\frac{1}{\mu' g}
$$


### 計算例

フランス Tier 4 軽戦車の AMX 40 を例に計算してみます。
車輌の制限速度に対してエンジンが非力なので、
最高速度は車輌の制限速度ではなくエンジンのパワーで決まります。

AMX 40 のエンジン最大出力は 190 hp,
接地抵抗が (hard, medium, soft) = (1.4, 1.5, 2.5),
総重量が 21.86 t,
動摩擦係数と重力加速度の積が 0.6867、
下式のように最高速度を 23.94, 22.34, 13.41 km/h と求めることができます。

$$
v = \frac{(190 /1.4) \times 735.5}{21860 \times 0.6867}
\times \frac{3600}{1000} = 23.94 ~\mathrm{(km/h)}
$$

$$
v = \frac{(190 /1.5) \times 735.5}{21860 \times 0.6867}
\times \frac{3600}{1000} = 22.34 ~\mathrm{(km/h)}
$$

$$
v = \frac{(190 /2.5) \times 735.5}{21860 \times 0.6867}
\times \frac{3600}{1000} = 13.41 ~\mathrm{(km/h)}
$$

これらの速度はあくまでも上記の仮定による理論値ですので、
実際の WoT での計測値と一致するかについては今後の検証が必要です。


## 最後に

ここまでの話を用いることで、車輌の最大速度を求めることはできますが、
加速度、とくに静止状態からの加速度を求めることはできません。
上に出てきた式の形でわかるように、
任意の速度における加速度を求めるためには、
速度とエンジン出力との関係を知る必要があります。

現実では、エンジン出力と回転数、トルクとの関係、そして変速比、最終減速比が相当します。
WoT ではこれらはどのように表現されているのでしょうか。
これらについてはまた別の機会に触れたいと思います。
