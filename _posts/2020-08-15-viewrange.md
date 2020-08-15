---
layout: post
title: 視認範囲の計算
mathjax: true
date: 2020-08-15 23:30 +0900
---

WoT の視認範囲について、計算方法をまとめました。

## はじめに

WoT の発見・観測メカニズムについては、公式・非公式を含めた解説ページや動画が作成されています。
しかし、その中で視認範囲の計算方法については詳細な記述がなかったり、
(おそらくは古い情報をもとにしているため) 不正確な情報となっている例が見られます。

この記事は、WoT の現時点のバージョン (1.10.0.0) をもとに、
視認範囲計算式の解説と、クライアント内部との比較による確認を示すことで、
実際に計算を行うために必要な情報を提供することを目指しています。

+ (公式) [[動画] メカニズム解説: 発見・観測1](https://worldoftanks.asia/ja/media/5/video-explaining-mechanics-spotting-system-1/)
+ (公式) [[動画] メカニズム解説: 発見・観測２](https://worldoftanks.asia/ja/news/general-news/explaining-mechanics-spotting-system/)
+ (公式) [(公式) 週刊ヴィクトリヤ日記 Vol.18 『視認システムについて学ぼう！前編』](https://worldoftanks.asia/ja/news/column/victoriadiary_018/)
+ (公式) [週刊ヴィクトリヤ日記 Vol.19 『視認システムについて学ぼう！後編』](https://worldoftanks.asia/ja/news/column/victoriadiary_019/)
+ [World of Tanks 日本語Wiki: ゲームシステム: 視認範囲](https://wikiwiki.jp/wotanks/ゲームシステム#had76155)
+ [World of Tanks Wiki: Battle Mechanics: View Range](https://wiki.wargaming.net/en/Battle_Mechanics#View_Range)
+ [WOTINFO.NET](http://www.wotinfo.net/en/camo-calculator)


## 視認範囲の計算式

視認範囲は車輌 (砲塔) の視認範囲カタログ値を基準に、
スキルや拡張パーツによる補正係数を作用させ、下式のように求められます。

$$
R_t = R_c \times P \times E \times F_{R} \times F_{SA}
$$

+ $R_t$ 実際の視認範囲
+ $R_c$ 視認範囲カタログ値
+ $P$ プライマリスキル (車長) による補正係数
+ $E$ 拡張パーツによる補正係数
+ $F_{R}$ 車長スキル偵察 (Recon) による補正係数
+ $F_{SA}$ 無線手スキル状況判断力 (Situational Awareness) による補正係数

各補正係数は以下のようになります。

### プライマリスキルによる係数

プライマリスキル (車長) による補正係数 $P$ は下式で計算されます。

$$
P = 0.57 + 0.43 \times S_c / 100
$$

+ $S_c$ 車長プライマリスキルレベル (補正後)

プライマリスキルが 100 であれば係数は 1 になります。
プライマリスキルレベルがスキルや拡張パーツの補正によって 100 を超える場合、
係数は 1 を超えます。

### 搭乗員スキル偵察による補正係数

車長の搭乗員スキル偵察 (Recon) による補正係数 $F_R$ は下式で計算されます。

$$
F_{R} = 1 + 0.0002 \times S_{R}
$$

+ $S_{R}$ 偵察 (Recon) スキルレベル (補正後)

偵察スキルレベルには車長プライマリスキルと戦友による補正、拡張パーツによる補正が適用されます。


### 搭乗員スキル状況判断力による補正係数

無線手の搭乗員スキル状況判断力 (Situational Awareness) による補正係数 $F_{SA}$ は下式で計算されます。

$$
F_{SA} = 1 + 0.0003 \times S_{SA}
$$

+ $S_{SA}$ 状況判断力 (Situational Awareness) スキルレベル (補正後)

状況判断力スキルレベルには車長プライマリスキルと戦友による補正、拡張パーツによる補正が適用されます。


### スキル効果についての補足

状況判断などのスキル効果は
スキルレベルごとに効果に対する係数が一定値上昇するという形で表現されています。

例えば無線手の状況判断力の場合は `item_defs/tankmen/tankmen.xml` の
`radioman_finder/visionRadiusFactorPerLevel`
で下記のように設定されており、
視界に対する係数が 1 レベル毎に 0.0003 上昇するようになっています。

```xml
    <radioman_finder>
      <userString>#item_types:tankman/skills/radioman_finder</userString>
      <description>#item_types:tankman/skills/radioman_finder_descr</description>
      <icon>radioman_finder.png</icon>
      <visionRadiusFactorPerLevel>0.0003</visionRadiusFactorPerLevel>
    </radioman_finder>
```

### 拡張パーツによる補正係数

拡張パーツによる補正係数は、各拡張パーツごとの固有の数値として定義されています。
対象となる拡張パーツは、レンズ被膜と双眼鏡の2種類で、
双眼鏡は車輌静止後一定時間 (3秒) 経過後に効果が発動するタイプの拡張パーツです。
双眼鏡とレンズ皮膜は、補正値の高いほうが有効となります (双眼鏡の効果が発動している場合には双眼鏡の補正値が採用されます)。

拡張パーツの補正値は `item_defs/vehicles/common/optional_devices.xml` で定義されています。
たとえば レンズ皮膜 クラス1 の定義は以下のように、
通常の補正値が 1.1、スロットが適合する場合の補正値が 1.115 になっています。

```xml
  <coatedOptics_tier1>
    ...
    <script>StaticOptionalDevice
      <factors>
        <factor>
          <attribute>miscAttrs/circularVisionRadiusFactor</attribute>
          <type>mul</type>
          <valueByLevel>1.1 1.115</valueByLevel>
        </factor>
      </factors>
    </script>
  </coatedOptics_tier3>
```

## 視認範囲現在値の取得と検証

### クライアント内部値の取得

[mod replserver](https://github.com/chirimenmonster/wotmods-replserver)
を使用して Super Hellcat の視認範囲を取得しました。

視認範囲のカタログ値は 370m で、
搭乗員のプライマリスキルはすべて 100%、
車長の偵察スキルはなく、無選手は状況判断 77% です。
拡張パーツはレンズ被膜と双眼鏡の両方を装備しています。

```python
> import BigWorld
> player = BigWorld.player()
> session = player.guiSessionProvider
> session.shared.feedback
<gui.battle_control.controllers.feedback_adaptor.BattleFeedbackAdaptor object at 0x0000000060369BA0>
> session.shared.feedback.getVehicleAttrs()
{'gunPiercing': 1.0, 'circularVisionRadius': 417}
> session.shared.feedback.getVehicleAttrs()
{'gunPiercing': 1.0, 'circularVisionRadius': 474}
```

視認範囲は、通常時で 417m、双眼鏡発動時で 474m となっています。
同様に、車長のプライマリスキルが 50% の場合についても確認すると、
通常時で 327m、双眼鏡発動時で 371m となっていました。

取得した視認範囲の計算はサーバ側で行われていて、
小数点以下が切り捨てられた値となっていますが、
発見判定に切り捨てられた値が使用されているのかどうかは不明です。

### 取得した値と計算結果との比較

上記の条件で、視認範囲を計算してみます。

#### 車長プライマリスキルが 100% の場合

車長プライマリスキルが 100% なので、プライマリスキルによる係数 $P$ は 1 になります。

$$
P = 0.57 + 0.43 \times 100 / 100 = 1
$$

スキル偵察を保有していないので、偵察による補正係数 $F_{R}$ は 1 になります。

$$
F_{R} = 1 + 0.0002 \times 0 = 1
$$

無線手の状況判断スキルが 77%、
車長によるボーナスが 10%、
戦友は保有していないので、
状況判断力による補正係数 $F_{SA}$ は下式で計算されます。

$$
F_{SA} = 1 + 0.0003 \times (77 + 10) = 1.0261
$$

通常時はレンズ被膜が有効となり、拡張パーツによる補正係数 $E$ は 1.1 になります。

$$
\lfloor R_c \times P \times E \times F_{R} \times F_{SA} \rfloor =
\lfloor 370 \times 1 \times 1.1 \times 1 \times 1.0261 \rfloor =
\lfloor 417.62... \rfloor = 417
$$

静止時は双眼鏡が有効となり、拡張パーツによる補正係数 $E$ は 1.25 になります。

$$
\lfloor R_c \times P \times E \times F_{R} \times F_{SA} \rfloor =
\lfloor 370 \times 1 \times 1.25 \times 1 \times 1.0261 \rfloor =
\lfloor 474.57... \rfloor = 474
$$

視認範囲は、通常時で 417m、双眼鏡発動時で 474m となり、
クライアントの内部値と一致していることが確認できます。


#### 車長プライマリスキルが 50% の場合

車長プライマリスキルが 50% なので、プライマリスキルによる係数 $P$ は下式で計算されます。

$$
P = 0.57 + 0.43 \times 50 / 100 = 0.785
$$

スキル偵察を保有していないので、偵察による補正係数 $F_{R}$ は 1 になります。

$$
F_{R} = 1 + 0.0002 \times 0 = 1
$$

無線手の状況判断スキルが 77%、
車長によるボーナスが 5%、
戦友は保有していないので、
状況判断力による補正係数 $F_{SA}$ は下式で計算されます。

$$
F_{SA} = 1 + 0.0003 \times (77 + 5) = 1.0246
$$

通常時はレンズ被膜が有効となり、拡張パーツによる補正係数 $E$ は 1.1 になります。

$$
\lfloor R_c \times P \times E \times F_{R} \times F_{SA} \rfloor =
\lfloor 370 \times 0.785  \times 1.1 \times 1 \times 1.0246 \rfloor =
\lfloor 327.35... \rfloor = 327
$$

静止時は双眼鏡が有効となり、拡張パーツによる補正係数 $E$ は 1.25 になります。

$$
\lfloor R_c \times P \times E \times F_{R} \times F_{SA} \rfloor =
\lfloor 370 \times 0.785 \times 1.25 \times 1 \times 1.0246 \rfloor =
\lfloor 371.99... \rfloor = 371
$$

視認範囲は、通常時で 327m、双眼鏡発動時で 371m となり、 クライアントの内部値と一致していることが確認できます。


#### 計算結果との比較 (セカンダリスキルが高めの場合)

別の例は Progetto 46 で、視認範囲のカタログ値は 390m です。
100% 乗員で戦友が有効、車長は偵察 93% と状況判断 100% のスキルを保有しています。 

クライアントから取得した有効な視認範囲は 418m となっていました。

プライマリスキルによる係数 $P$ は下式となります。
戦友によるボーナス 5% がスキルレベルに加算されています。

$$
P = 0.57 + 0.43 \times (100 + 5)/ 100 = 1.0215
$$

スキル偵察による補正係数 $F_{R}$ は下式となります。
偵察のスキルレベル 93% に、
車長のプライマリスキルによるボーナス 10.5% と、
戦友によるボーナス 5% が加算されます。
ただし、合計のスキルレベルが 100% を超えるので 100% に丸められます
(注: 100% への丸めについてはまだ検証が十分ではありません)。

$$
F_{R} = 1 + 0.0002 \times \min(93 + 10.5 + 5, 100) = 1.02
$$

スキル状況判断力による補正係数 $F_{SA}$ は下式となります。
状況判断力のスキルレベル 100% に、
車長のプライマリスキルによるボーナス 10.5% と、
戦友によるボーナス 5% が加算されます。
ただし、合計のスキルレベルが 100% を超えるので 100% に丸められます。

$$
F_{SA} = 1 + 0.0003 \times \min(100 + 10.5 + 5, 100) = 1.03
$$

視認範囲に影響を与える拡張パーツを搭載していないので、
拡張パーツによる補正係数 $E$ は 1 になります。

視認範囲は下式のように計算され、クライアント内部から取得した値 418m と一致していることが確認できます。

$$
\lfloor R_c \times P \times E \times F_{R} \times F_{SA} \rfloor =
\lfloor 390 \times 1.0215 \times 1 \times 1.02 \times 1.03 \rfloor =
\lfloor 418.54... \rfloor = 418
$$
