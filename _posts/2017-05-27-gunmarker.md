---
layout: post
title: WoT の主砲マーカーの画像
date: 2017-05-27 14:00 +0900
---
WoT の主砲マーカー (Gun Marker) は `AvatarInputHandler.control.modes._FlashGunMarker` で作成されます。
表示系のスクリプトやデータは SWF ファイル `sniperCrosshair.swf` から読まれますが、
データの実体の多くは `sniperCrosshair.swf` からインポートされている `crosshairControls.swf` に置かれています。

これらの SWF ファイルは `res/packages/gui.pkg` 内の `gui/flash` に置かれており、
他の多くのリソースと同様に、`res_mods/<WOT_VERSION>/gui/flash` に同名のファイルを置くことでその内容を置き換えることができます。


## 貫通インジケータ

クラス `_FlashGunMarker` 内の定義では、
貫通インジケータは貫通可能性とカラーブラインドモードの有無に応じて以下の色となります。

ここで色の名前は後述するように SWF 内で定義される sprite の各フレームにつけられたラベル FrameLabel.name を指します。

| color blind mode | stats | colors |
|:---:|:---:|:---:|
| default     | not_pierced    | red    |
| default     | little_pierced | orange |
| default     | great_pierced  | green  |
| color_blind | not_pierced    | purple |
| color_blind | little_pierced | yellow |
| color_blind | great_pierced  | green  |

## crosshairControls.swf

### sprites

表示用の画像のセットが sprites の領域にあります。
各 sprite はFlashアニメーションの形式になっていますが、実際には複数の画像をグループ化するためのコンテナとして使用されています。

例えば WoT 0.9.18.0 の `crosshairControls.swf` 内で定義されている
ID=66 の sprite (crosshairControls_fla.A_3) では、
6個の frame 、つまり、6個の画像を格納 (ID による参照) しており、
表示するフレーム番号、またはラベル名を指定することで画像を切り替える仕組みとなっています。

ここでの参照先はいずれも shape となっています。

| ID | 名称 | UI上の表示 |
|:---:|:---:|:---|
| 66 | crosshairControls_fla.A_3 | 主砲マーカー A字型I (装甲貫通インジケータ付き) |
| 67 | crosshairControls_fla.A_4 | 主砲マーカー A字型II (装填インジケータ付き) |
| 68 | -                         | 主砲マーカー A字型III |
| 80 | crosshairControls_fla.X_6 | 主砲マーカー 十字型I (装甲貫通インジケータ付き) |
| 81 | crosshairControls_fla.X_8 | 主砲マーカー 十字型II (装填インジケータ付き) |
| 82 | -                         | 主砲マーカー 十字型III |
| 93 | crosshairControls_fla.Dot_10 | 主砲マーカー 照準点I (装甲貫通インジケータ付き) |
| 94 | crosshairControls_fla.Dot_11 | 主砲マーカー 照準点II (装填インジケータ付き) |
| 95 | crosshairControls_fla.Dot_12 | 主砲マーカー 照準点III |
| 106 | crosshairControls_fla.O_13 | 主砲マーカー O字型I (装甲貫通インジケータ付き) |
| 107 | crosshairControls_fla.O_14 | 主砲マーカー O字型II (装填インジケータ付き) |
| 108 | crosshairControls_fla.O_15 | 主砲マーカー O字型III |
| 119 | crosshairControls_fla.A_16 | 主砲マーカー A字型IV (装甲貫通インジケータ付き) |
| 120 | crosshairControls_fla.A_17 | 主砲マーカー A字型V (装填インジケータ付き) |
| 121 | crosshairControls_fla.A_18 | 主砲マーカー A字型VI |


#### 66: crosshairControls_fla.A_3 (主砲マーカー A字型I)

| 名称 | 参照ID | FrameLabel.name |
|:---:|:---:|:---|
| frame 1 | 55 | normal |
| frame 2 | 57 | red    |
| frame 3 | 59 | orange |
| frame 4 | 61 | green  |
| frame 5 | 63 | purple |
| frame 6 | 65 | yellow |


### shapes

画像の形状が定義されて、画像の原点の位置 (参照点) などはここで指定されます。
画像データそのものは別に定義された image の ID を参照する形で埋め込んでいます。

FFdec を使った SVG 形式でのエクスポートや置き換えが可能で、
エクスポート時には、画像のデータが実際に埋め込まれた状態でエクスポートされます。

次の表は crosshairControls_fla.A_3 (ID=66) で使用されている shape の一覧です。
Needed は埋め込み画像の参照先 ID 、
Dependent は各 shape を参照しているオブジェクト (ここでは sprite) の ID を表します。 

| ID | Needed | Dependent |
|:---:|:---:|:---|
| 55 | 54 | 66, 67, 68, 122, 132, 123 |
| 57 | 56 | 66, 122, 132, 123 |
| 59 | 58 | 66, 122, 132, 123 |
| 61 | 60 | 66, 67, 122, 132, 123 |
| 63 | 62 | 66, 122, 132, 123 |
| 65 | 64 | 66, 122, 132, 123 |

上記の Dependent ID のうち、
122, 132, 123 は照準設定 UI のための sprite で、
戦闘中に使用されるのは 66 (A字型I 貫通インジケータ付き), 67 (A字型II 装填インジケータ付き), 68 (A字型III) です。

A字型I ～ III を照準タイプ別にまとめると以下のようになります。

| shape | image | A字型I (ID=66) |
|:---:|:---:|:---:|
| 55 | 54 | 照準していないとき
| 57 | 56 | not_pierced
| 59 | 58 | little_pierced
| 61 | 60 | great_pierced
| 63 | 62 | not_pierced (color_blind)
| 65 | 64 | little_pierced （color_blind）

| shape | image | A字型II (ID=67) |
|:---:|:---:|:---:|
| 55 | 54 | 装填中 |
| 61 | 60 | 装填完了 |

| shape | image | A字型III (ID=68) |
|:---:|:---:|:---:|
| 55 | 54 | 装填中 |

### images

実際の画像データが定義されます。
幅や高さなどは定義されるが、原点の位置などの情報は持ちません。

FFdec により PNG 形式でのエクスポートや置き換えが可能です。

