---
layout: post
title: WoT の砲弾の速度
mathjax: true
date: 2020-01-20 23:00 +0900
---
WoT の砲弾の速度について調べてみました。
弾速はカタログスペックの0.8倍、重力加速度は0.64倍で、
弾速は遅くなっていますが軌跡は変わりません。

※ WoT 1.7.0.2 で確認しています。

## 概要

+ WoT の砲弾の軌道は放物線を描く
+ 空気抵抗による減速は考慮しない
+ 自走砲の砲弾は曲率を大きくするため重力加速度を大きく調整している
+ 砲弾の速度、重力加速度はXMLファイルで砲弾ごとに定義されている
+ 通常砲弾の重力加速度は 9.81 m/s<sup>2</sup>
+ クライアント内での砲弾の速度はカタログスペックの0.8倍、重力加速度は0.64倍


## はじめに

WoT における砲弾の速度は、公式では明らかにされていません。
これはクライアントの GUI インターフェースや、
公式が提供する[戦車辞典 (Tankopedia)](https://worldoftanks.asia/ja/tankopedia)
で公開されている車輌の特性値には含まれていないということを意味します。

ですが、サードパーティーが提供するデータベース、たとえば [tanks.gg](https://tanks.gg/list) などでは
弾速が記されています。
また、クライアントが保有するデータファイルの中にも弾速に関する記述を見つけることができます。

クライアントのデータファイル（XML 形式）上の定義と、
tanks.gg の弾速に関する記述は一致していて、
tanks.gg はクライアントのデータファイルをソースとしていると推測されます。
公式で公開されているわけではありませんが、
この記述をここでは便宜上カタログスペックとして扱うことにします。


### 弾速

弾速は砲弾毎に異なる値ですが、
砲弾だけでなく、それを発射する主砲にも依存するパラメータとなっています。

それぞれの主砲には、どの砲弾を発射するか（よく知られているように最大で3です）が決められているので、
弾速の定義は XML データの中の主砲のサブツリーに存在します。


### 重力加速度

現実世界の重力加速度は 9.81 m/s<sup>2</sup> ですが、
自走砲が発射する砲弾に作用する重力加速度は、
ゲームの演出のために現実とは大きく異なる値が設定されており、
弾速と同様に主砲と組み合わせた砲弾ごとに固有の値となっています。

その他の車輌が発射する砲弾に作用する重力加速度は、
砲弾ごとに固有の箇所で定義されてはいますが、
（自走砲以外の）全砲弾に対して同一の値となっています。

通常車両の砲弾の重力加速度は、
定義上は現実世界と同じ 9.81 m/s<sup>2</sup> となっていますが、
後述するように一定の係数がかかった異なる値となっています。


### 弾道

発射された砲弾は放物線軌道を描きます。
空気抵抗は考慮されません。


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

実際にクライアント内のデータを確認してみます。
Jg.Pz. E 100 のリプレイ再生中に [replserver]({% post_url 2018-03-18-mod_replserver %}) で接続し、
下記のコードを実行します。
speed、 gravity がそれぞれ XML 内定義値の 0.8 倍、0.64 倍になっていることが確認できます。

```python
> import BigWorld
> player = BigWorld.player()
> shot = player.getVehicleDescriptor().shot
> print 'speed = %.1f m/s, gravity = %.4f m/s2' % (shot.speed, shot.gravity)
speed = 740.0 m/s, gravity = 6.2784 m/s2

```


## 自走砲の砲弾

自走砲の砲弾は、ゲームの演出のため砲弾の軌跡の曲率を大きくする調整が行われており、
重力加速度を大きくすることで表現されています（重力加速度が大きいほど、曲率が大きくなります）。

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

ちなみに重力加速度が大きく設定されている B-C 155 58 の場合は 170 m/s<sup>2</sup> でした。
17.3 倍です。
大きいですね。

自走砲の砲弾にも、通常車輌砲弾と同様に、
クライアント内では速度に 0.8、重力加速度に 0.8<sup>2</sup> が乗じられます。


## Dispertion Indicator による内部データの確認

mod Dispertion Indicator 0.4.2
([Mod Hub](https://wgmods.net/3271/) | [GitHub](https://github.com/chirimenmonster/wotmods-dispersionindicator))
で弾速と重力加速度を確認できるようにしました。
同時に弾着時間も計算しています。

GitHub の方からダウンロードできる zip ファイル付属の config-full.json を config.json にリネームして
WoT を実行すると下記のような表示が得られます。

![Ｄｉｓｐｅｒｔｉｏｎ Ｉｎｄｉｃａｔｏｒ](/resources/image_20200120_01.jpg "Dispertion Indicator 実行時の様子")


## まとめ

WoT の弾速と砲弾に作用する重力加速度は隠しパラメータとなっていて公式には明かされていませんが、
tanks.gg などではクライアント内の定義データを参照した値が公開されています。

ただし、実際のクライアント内でのデータ処理では定義データの弾速を 0.8 倍した値が、
重力加速度については 0.64 倍した値が使用されています。

この調整によって、弾着時間は 1.25 倍長くなる形で影響を受けますが、
砲弾の軌跡は影響を受けません。


## 注意点

この調査はクライアントの挙動について行ったもので、
実際の弾着判定が行われることになるサーバ側の挙動については、
すべて推測によるものです。


