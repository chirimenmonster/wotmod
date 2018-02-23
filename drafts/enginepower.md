---
layout: page
title: エンジン出力と最大速度
mathjax: true
sitemap: false
---
WoT の車輌には最大速度の設定がありますが、
この最大速度とは実は速度のリミッターのことを表しています。
AMX40 など最大速度に比べてエンジンが非力な車輌ではリミッターとしての最大速度には届きません。
そういった車輌で得られる実際の最大速度の求め方について解説します。

参照コードは WoT 0.9.22.0.1 です。


## 物理モデル

WoT の物理モデルの詳細は公表されていませんが、
車輌のエンジンには出力があり、各種の摩擦の概念があります。
議論の出発点としては、実際の物理法則をある程度単純化したところから
モデルを仮定して始めるのでよいでしょう。
仮定して推定した結果が計測結果と一致しなければ、
そのときに仮定したモデルを再検討すればよいのです。


## エンジン出力 (engine power)

エンジン出力 (engin power) はエンジンのパラメータとして定義されています。
クライアント内部の定義や tankopedia などで確認できる出力の単位は hp (horse power: 馬力) です。

馬力の物理量は仕事率に相当し、
単位時間内のエネルギーを表します。
馬力には仏馬力と英馬力の2種がありますが、
後述のように WoT では仏馬力が採用されています。
通常、仏馬力の単位は PS と表記するのですが、
ここでは tankopedia に合わせて hp と表記します。

馬力から SI 単位系であるワットへの換算は common/items/vehicles.py で、
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

重量 (weight) は車体を始めとする各モジュールの重量の合計で、
クライアント内部での単位は kg が使用されています。

重量による鉛直方向の力について、
重力単位系 kgf から SI 単位系 N への換算係数 `KG_TO_NEWTON` は common/items/components/component_conststnt.py で定義されており、
重力加速度として 9.81 m/s<sup><small>2</small></sup> が使用されていることがわかります。

```python
KG_TO_NEWTON = 9.81
```


## 地形抵抗 (terrain resistance)

地形抵抗 (terrain resistance) は履帯のパラメータで、
地形の種別、hard, medium, soft の3種類それぞれに対して定義されています。
これらの値はエンジンの出力に対する低減係数として扱われます。

地形抵抗は操縦手のプライマリスキルによって軽減されます。
さらに、
地形の medium と soft については、
オフロードスキルや
グロウサーによって軽減されます。
素の地形抵抗を $r_t$、
操縦手プライマリスキルによる低減係数を $f_d$、
オフロードスキルによる低減係数を $f_b$、
グロウサーによる低減係数を $f_g$ とすると、
各種効果を考慮した地形抵抗 $r'_t$は下式で表せます。

$$
r'_t = r_t \cdot f_d \cdot f_b \cdot f_g
$$

操縦手のプライマリスキルによる低減係数 $f_d$ は、
他の多くのスキルと同様にスキルレベルを $S_\mathrm{driver}$ (%) として下式で表せます。
スキルレベルが 100+10% の場合は約 0.959 です。

$$
f_d = \frac{1}{0.57 + 0.43\cdot S_\mathrm{driver}/100}
$$

コード上では
common/items/VehicleDescrCrew.py
の以下の処理になります。

```python
        for skillName, efficiency, baseAvgLevel in skillEfficiencies:
            factor = 0.57 + 0.43 * efficiency
            skillToBoost.discard(skillName)
            self.callSkillProcessor(skillName, factor, baseAvgLevel)
```

```python
    def _updateDriverFactors(self, factor, baseAvgLevel):
        factor = 1.0 / factor
        r = self._terrainResistanceFactors
        r[0] *= factor
        r[1] *= factor
        r[2] *= factor
```

オフロードスキルによる低減係数 $f_b$ は、
スキルレベル $S_\mathrm{offroad}$ に対し、
medium で 0.00025、 soft で 0.001 の係数を乗じ、
下式で求められます。
スキルレベルが 100+10% の場合、
medium では 0.9725、
soft では 0.89 になります。

$$
f_b =
\begin{cases}
1 - 0.00025 \cdot S_\mathrm{offroad} & \text{(medium)}\\
1 - 0.001 \cdot S_\mathrm{offroad} & \text{(soft)}
\end{cases}
$$


コード上では
common/items/VehicleDescrCrew.py
の以下の処理になります。

```python
        level = level + levelIncrease
        r = self._terrainResistanceFactors
        r[1] *= max(0.001, 1.0 - level * skillConfig.mediumGroundResistanceFactorPerLevel)
        r[2] *= max(0.001, 1.0 - level * skillConfig.softGroundResistanceFactorPerLevel)
```

```xml
<softGroundResistanceFactorPerLevel>0.001</softGroundResistanceFactorPerLevel>
<mediumGroundResistanceFactorPerLevel>0.00025</mediumGroundResistanceFactorPerLevel>
```

グロウサーによる低減係数 $f_g$ は定数で、
medium で 0.952、
soft で 0.909 に設定されています。
これらはそれぞれ、105%、110% の逆数になっています。

$$
f_g =
\begin{cases}
0.952 \fallingdotseq 100 / (100 + 5) & \text{(medium)} \\
0.909 \fallingdotseq 100 / (100 + 10) & \text{(soft)}
\end{cases}
$$

コード上では
common/items/artefacts.py
の以下の処理になります。

```python
        r = vehicleDescr.physics['terrainResistance']
        vehicleDescr.physics['terrainResistance'] = (r[0], r[1] * self.__factorMedium, r[2] * self.__factorSoft)
```

```python
        self.__factorSoft = _xml.readPositiveFloat(xmlCtx, section, 'softGroundResistanceFactor')
        self.__factorMedium = _xml.readPositiveFloat(xmlCtx, section, 'mediumGroundResistanceFactor')
```

```xml
<softGroundResistanceFactor>0.909</softGroundResistanceFactor>
<mediumGroundResistanceFactor>0.952</mediumGroundResistanceFactor>
```


## 摩擦係数 (specific friction)

摩擦係数 (specific friction) は共通パラメータです。
クライアント内部では
common/items/components/component_conststnt.py の
定数 `DEFAULT_SPECIFIC_FRICTION` として
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
地形抵抗が (hard, medium, soft) = (1.4, 1.5, 2.5),
総重量が 21.86 t,
動摩擦係数と重力加速度の積が 0.6867
なので、
車長補正込みの操縦手プライマリスキル 110% に対する低減係数を 0.959 としたとき、
下式のように勾配がない場合の最高速度を 24.96, 23.30, 13.98 km/h と求めることができます。

|項目|値|
|:--:|:--:|
|最大出力 (hp)| 190|
|総重量 (ton)| 21.86 |
|摩擦係数|0.07|
|地形抵抗 (hard/medium/soft)|1.4 / 1.5 / 2.5 |
|操縦手スキル (%)| 100 + 10 |

$$
v = \frac{(190 /1.4 /0.959) \times 735.5}{21860 \times 0.6867}
\times \frac{3600}{1000} = 24.96 ~\mathrm{(km/h)}
$$

$$
v = \frac{(190 /1.5 /0.959) \times 735.5}{21860 \times 0.6867}
\times \frac{3600}{1000} = 23.30 ~\mathrm{(km/h)}
$$

$$
v = \frac{(190 /2.5 /0.959) \times 735.5}{21860 \times 0.6867}
\times \frac{3600}{1000} = 13.98 ~\mathrm{(km/h)}
$$

これらの速度はあくまでも上記の仮定による理論値ですので、
実際の WoT での計測値と一致するかについては今後の検証が必要です。


## 最後に

ここまでの話を用いることで、車輌の最大速度を求めることはできますが、
加速度、とくに静止状態からの加速度を求めることはできません。
上に出てきた式の形でわかるように、
任意の速度における加速度を求めるためには、
速度とエンジン出力との関係を知る必要があります。

現実では、エンジン出力と回転数、トルクとの関係、そして変速比、最終減速比がそれに相当します。
WoT ではこれらはどのように表現されているのでしょうか。
これらについてはまた別の機会に触れたいと思います。
