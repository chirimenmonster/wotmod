---
layout: post
title: 砲弾の速度
mathjax: true
date: 2020-01-19 21:50 +0900
---

## tl;tr

+ WoT の砲弾の軌道は放物線を描く
+ 空気抵抗による減速は考慮しない
+ 自走砲の砲弾は曲率を大きくするため重力加速度を大きく調整している
+ 砲弾の速度、重力加速度はXMLファイルで定義されている
+ 通常砲弾の重力加速度は 9.81 m/s<sup>2</sup>
+ クライアント内での砲弾の速度はカタログスペックの0.8倍、重力加速度は0.64倍


## 砲弾の定義

Jg.Pz. E 100 の HEAT 弾の定義を調べてみましょう。

Jg.Pz. E100 の砲と砲弾の定義は
scripts/item_defs/vehicles/germany/components/guns.xml
にあり、
砲 17 cm Pak は _170mm_PaK_K72 というタグ、
HEAT 弾 Gr 46 H1A は _170mm_PaK_K72_PzGr45 という識別子になっています。

該当箇所を以下に示します。
関係のある個所以外は省略しました。

```xml
    <_170mm_PaK_K72>
      <userString>#germany_vehicles:_170mm_PaK_K72</userString>
      （略）
      <shots>
        （略）
        <_170mm_PaK_K72_PzGr45>
          <defaultPortion>0.0</defaultPortion>
          <speed>925</speed>
          <gravity>9.81</gravity>
          <maxDistance>720</maxDistance>
          <piercingPower>420 420</piercingPower>
        </_170mm_PaK_K72_PzGr45>
        （略）
      </shots>
    </_170mm_PaK_K72>
```

砲弾の速度が speed で定義されており値は 925 （単位は m/s）、
重力加速度が gravity で定義され値は 9.81 （単位は m/s<sup>2</sup>）
であることが確認できます。


## クライアント内部の値

クライアントが XML から砲弾のスペックを取り出すのは
common/items/readers/gun_readers.py
の readShot です。

```python
def readShot(xmlCtx, section, nationID, projectileSpeedFactor, cache):
    shellName = section.name
    shellID = cache.shellIDs(nationID).get(shellName)
    if shellID is None:
        _xml.raiseWrongXml(xmlCtx, '', 'unknown shell type name')
    shellDescr = cache.shells(nationID)[shellID]
    return gun_components.GunShot(shellDescr, ZERO_FLOAT if not section.has_key('defaultPortion') else _xml.readFraction(xmlCtx, section, 'defaultPortion'), _xml.readVector2(xmlCtx, section, 'piercingPower'), _xml.readPositiveFloat(xmlCtx, section, 'speed') * projectileSpeedFactor, _xml.readNonNegativeFloat(xmlCtx, section, 'gravity') * projectileSpeedFactor ** 2, _xml.readPositiveFloat(xmlCtx, section, 'maxDistance'), _xml.readFloat(xmlCtx, section, 'maxHeight', 1000000.0))
```

この中で speed の項には下記のように projectileSpeedFactor が乗じられています。

```python
_xml.readPositiveFloat(xmlCtx, section, 'speed') * projectileSpeedFactor
```

また、gravity の項には projectileSpeedFactor の2乗が乗じられています。

```python
_xml.readNonNegativeFloat(xmlCtx, section, 'gravity') * projectileSpeedFactor ** 2
```

projectileSpeedFactor は全車輌全砲弾で共通のパラメータとして
item_defs/vehicles/common/vehicle.xml
で定義されています。
値は 0.8 です。

```xml
  <miscParams>
    （略）
    <projectileSpeedFactor>0.8</projectileSpeedFactor>
```

砲弾の速度に 0.8、重力加速度に 0.8<sup>2</sup> が乗じられているので、
弾速は当初の定義よりも遅くなっていますが、砲弾の軌跡は変わりません。


## 自走砲の砲弾

自走砲の砲弾は、ゲームの演出のため砲弾の軌跡の曲率を大きくする調整が行われており、
重力加速度を大きくすることで表現されています。

調整された重力加速度は車輌毎に異なります（定義個所は砲になります）。

G.W. E 100 の砲弾 Gr. 18 Stg の定義 (_210mm_Sprg18) は
下記のようになっています。

```
    <_210mm_Morser_18>
      <userString>#germany_vehicles:_210mm_Morser_18</userString>
      （略）
        <_210mm_Sprg18>
          <defaultPortion>1.00</defaultPortion>
          <speed>435</speed>
          <gravity>142</gravity>
          <maxDistance>10000</maxDistance>
          <piercingPower>53 53</piercingPower>
          <maxHeight>411</maxHeight>
        </_210mm_Sprg18>
      （略）
    </_210mm_Morser_18>
```

ここで重力加速度 gravity は 142 m/s<sup>2</sup>、
9.81 のおよそ14.5倍です。

ちなみに B-C 155 58 の場合は 170 m/s<sup>2</sup> でした。
大きいですね。
