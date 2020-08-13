---
layout: post
title: WoT 1.10 の拡張パーツ一覧
mathjax: true
date: 2020-08-08 21:30 +0900
last_modified_at: 2020-08-13 08:50 +0900
---

WoT クライアント内から WoT 1.10 の拡張パーツ情報を抽出しました。
日本語表示名と説明文は
ASIA 日本語クライアントから、
英語表示名は
EU 英語クライアントから抽出しています。
その他のパラメータは `optional_devices.xml`
(`item_defs/vehicles/common/optional_devices.xml`)
から抽出したものをもとにしています。

(2020.8.11 更新: 各パーツの詳細情報、効果からの逆引き表を追加しました)


## Optional Device List

| name | userString_en | userString_ja |
|:--- |:--- |:--- |
| [deluxImprovedConfiguration](#deluximprovedconfiguration) | Improved Configuration | 高性能モジュール構造 |
| [deluxImprovedVentilation](#deluximprovedventilation) | Venting System | 高性能換気装置 |
| [deluxRammer](#deluxrammer) | Innovative Loading System | 新型装填装置 |
| [deluxCoatedOptics](#deluxcoatedoptics) | Experimental Optics | 試作型高視認性レンズ |
| [deluxAimingStabilizer](#deluxaimingstabilizer) | Stabilizing Equipment System | 高性能安定装置 |
| [deluxEnhancedAimDrives](#deluxenhancedaimdrives) | Wear-Resistant Gun Laying Drive | 耐摩耗射撃装置 |
| [trophyBasicAimDrives](#trophybasicaimdrives) | Bounty Gun Laying Drive | 報酬射撃装置 |
| [trophyUpgradedAimDrives](#trophyupgradedaimdrives) | Bounty Gun Laying Drive | 報酬射撃装置 |
| [trophyBasicTankRammer](#trophybasictankrammer) | Bounty Rammer | 報酬装填棒 |
| [trophyUpgradedTankRammer](#trophyupgradedtankrammer) | Bounty Rammer | 報酬装填棒 |
| [trophyBasicImprovedVentilation](#trophybasicimprovedventilation) | Bounty Ventilation | 報酬換気装置 |
| [trophyUpgradedImprovedVentilation](#trophyupgradedimprovedventilation) | Bounty Ventilation | 報酬換気装置 |
| [trophyBasicAimingStabilizer](#trophybasicaimingstabilizer) | Bounty Stabilizer | 報酬安定装置 |
| [trophyUpgradedAimingStabilizer](#trophyupgradedaimingstabilizer) | Bounty Stabilizer | 報酬安定装置 |
| [trophyBasicCoatedOptics](#trophybasiccoatedoptics) | Bounty Optics | 報酬レンズ |
| [trophyUpgradedCoatedOptics](#trophyupgradedcoatedoptics) | Bounty Optics | 報酬レンズ |
| [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | Bounty Protection Technology | 報酬モジュール構造 |
| [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | Bounty Protection Technology | 報酬モジュール構造 |
| [improvedConfiguration_tier2](#improvedconfiguration_tier2) | Modified Configuration Class 2 | 改修型モジュール クラス2 |
| [improvedConfiguration_tier1](#improvedconfiguration_tier1) | Modified Configuration Class 1 | 改修型モジュール クラス1 |
| [antifragmentationLining_tier4](#antifragmentationlining_tier4) | Light Spall Liner | 内張り装甲 (小) |
| [antifragmentationLining_tier3](#antifragmentationlining_tier3) | Medium Spall Liner | 内張り装甲 (中) |
| [antifragmentationLining_tier2](#antifragmentationlining_tier2) | Heavy Spall Liner | 内張り装甲 (大) |
| [antifragmentationLining_tier1](#antifragmentationlining_tier1) | Superheavy Spall Liner | 内張り装甲 (特大) |
| [stereoscope_tier3](#stereoscope_tier3) | Binocular Telescope Class 3 | 双眼鏡 クラス3 |
| [stereoscope_tier2](#stereoscope_tier2) | Binocular Telescope Class 2 | 双眼鏡 クラス2 |
| [stereoscope_tier1](#stereoscope_tier1) | Binocular Telescope Class 1 | 双眼鏡 クラス1 |
| [coatedOptics_tier3](#coatedoptics_tier3) | Coated Optics Class 3 | レンズ皮膜 クラス3 |
| [coatedOptics_tier2](#coatedoptics_tier2) | Coated Optics Class 2 | レンズ皮膜 クラス2 |
| [coatedOptics_tier1](#coatedoptics_tier1) | Coated Optics Class 1 | レンズ皮膜 クラス1 |
| [camouflageNet_tier3](#camouflagenet_tier3) | Camouflage Net Class 3 | 迷彩ネット クラス3 |
| [camouflageNet_tier2](#camouflagenet_tier2) | Camouflage Net Class 2 | 迷彩ネット クラス2 |
| [improvedVentilation_tier3](#improvedventilation_tier3) | Improved Ventilation Class 3 | 改良型換気装置 クラス 3 |
| [improvedVentilation_tier2](#improvedventilation_tier2) | Improved Ventilation Class 2 | 改良型換気装置 クラス 2 |
| [improvedVentilation_tier1](#improvedventilation_tier1) | Improved Ventilation Class 1 | 改良型換気装置 クラス 1 |
| [grousers_tier3](#grousers_tier3) | Additional Grousers Class 3 | 追加グローサー クラス3 |
| [grousers_tier2](#grousers_tier2) | Additional Grousers Class 2 | 追加グローサー クラス2 |
| [grousers_tier1](#grousers_tier1) | Additional Grousers Class 1 | 追加グローサー クラス1 |
| [tankRammer_tier2](#tankrammer_tier2) | Gun Rammer Class 2 | 装填棒 クラス2 |
| [tankRammer_tier1](#tankrammer_tier1) | Gun Rammer Class 1 | 装填棒 クラス1 |
| [enhancedAimDrives_tier3](#enhancedaimdrives_tier3) | Enhanced Gun Laying Drive Class 3 | 改良型射撃装置 クラス3 |
| [enhancedAimDrives_tier2](#enhancedaimdrives_tier2) | Enhanced Gun Laying Drive Class 2 | 改良型射撃装置 クラス2 |
| [enhancedAimDrives_tier1](#enhancedaimdrives_tier1) | Enhanced Gun Laying Drive Class 1 | 改良型射撃装置 クラス1 |
| [aimingStabilizer_tier2](#aimingstabilizer_tier2) | Vertical Stabilizer Class 2 | 砲垂直安定装置 クラス2 |
| [aimingStabilizer_tier1](#aimingstabilizer_tier1) | Vertical Stabilizer Class 1 | 砲垂直安定装置 クラス1 |
| [additionalInvisibilityDevice_tier3](#additionalinvisibilitydevice_tier3) | Low Noise Exhaust System Class 3 | 消音排気システム クラス3 |
| [additionalInvisibilityDevice_tier2](#additionalinvisibilitydevice_tier2) | Low Noise Exhaust System Class 2 | 消音排気システム クラス2 |
| [additionalInvisibilityDevice_tier1](#additionalinvisibilitydevice_tier1) | Low Noise Exhaust System Class 1 | 消音排気システム クラス1 |
| [extraHealthReserve_tier3](#extrahealthreserve_tier3) | Improved Hardening Class 3 | 改良型装甲材 クラス3 |
| [extraHealthReserve_tier2](#extrahealthreserve_tier2) | Improved Hardening Class 2 | 改良型装甲材 クラス2 |
| [extraHealthReserve_tier1](#extrahealthreserve_tier1) | Improved Hardening Class 1 | 改良型装甲材 クラス1 |
| [improvedRadioCommunication](#improvedradiocommunication) | Improved Radio Set | 改良型無線機 |
| [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | Improved Rotation Mechanism Class 2 | 改良型旋回機構 クラス2 |
| [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | Improved Rotation Mechanism Class 1 | 改良型旋回機構 クラス1 |
| [turbocharger_tier3](#turbocharger_tier3) | Turbocharger Class 3 | ターボチャージャー クラス3 |
| [turbocharger_tier2](#turbocharger_tier2) | Turbocharger Class 2 | ターボチャージャー クラス2 |
| [turbocharger_tier1](#turbocharger_tier1) | Turbocharger Class 1 | ターボチャージャー クラス1 |
| [commandersView](#commandersview) | Commander's Vision System | 車長用視認性向上装置 |
| [improvedSights_tier2](#improvedsights_tier2) | Improved Aiming Class 2 | 改良型照準器 クラス2 |
| [improvedSights_tier1](#improvedsights_tier1) | Improved Aiming Class 1 | 改良型照準器 クラス1 |


## Optional Device Descriptions

### deluxImprovedConfiguration

| Name | Description |
|:--- |:--- |
| id | 43 |
| userString_en | Improved Configuration |
| userString_ja | 高性能モジュール構造 |
| shortDescriptionSpecial_ja | 修理速度を向上させると同時に、燃料タンク、弾薬庫、エンジンを保護する。 |
| longDescriptionSpecial_ja | 改良型モジュール構造のさらなる高性能版で、効果が強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| shortDescriptionSpecial_en | Accelerates repairs, protects ammo rack, fuel tanks, and engine |
| longDescriptionSpecial_en | An analogue of Modified Configuration that provides a stronger effect. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | improvedConfiguration |
| weight | 50 |
| price | 3,000 bonds |
| removable | false |
| tags | improvedConfiguration, deluxe, armor |
| incompatibleTags | improvedConfiguration |

#### Vehicle Filter

| Type | MinLevel |
|:---:|:---:|
| include | 5 |

#### Factors (type=ImprovedConfiguration)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [engineReduceFineFactor](#enginereducefinefactor) | mul | 0.35 | 0.35 |
| [ammoBayReduceFineFactor](#ammobayreducefinefactor) | mul | 0.35 | 0.35 |
| [miscAttrs/repairSpeedFactor](#miscattrsrepairspeedfactor) | mul | 1.45 | 1.45 |
| [miscAttrs/ammoBayHealthFactor](#miscattrsammobayhealthfactor) | mul | 2.5 | 2.5 |
| [miscAttrs/fuelTankHealthFactor](#miscattrsfueltankhealthfactor) | mul | 2.5 | 2.5 |
| [miscAttrs/engineHealthFactor](#miscattrsenginehealthfactor) | mul | 2.5 | 2.5 |
| [miscAttrs/fireStartingChanceFactor](#miscattrsfirestartingchancefactor) | mul | 0.35 | 0.35 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleRepairSpeed | mul | 1.45 |
| vehicleAmmoBayEngineFuelStrength | mul | 2.5 |
| vehPenaltyForDamageEngineAndCombat | mul | 0.35 |
| vehicleFireChance | mul | 0.35 |

### deluxImprovedVentilation

| Name | Description |
|:--- |:--- |
| id | 44 |
| userString_en | Venting System |
| userString_ja | 高性能換気装置 |
| longDescriptionSpecial_ja | 改良型換気装置のさらなる高性能版で、全搭乗員のスキルに対するボーナスが +5% から +8.5% に強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Ventilation ( +5% to all crew skills) that provides a stronger effect:  +8.5%  to all crew skills. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | improvedVentilation |
| weight | 100 |
| price | 5,000 bonds |
| removable | false |
| tags | ventilation, deluxe, armor, firepower, camouflage, reconnaissance, mobility |
| incompatibleTags | ventilation |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | improvedVentilation_class1_user<br>improvedVentilation_class2_user<br>improvedVentilation_class3_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/crewLevelIncrease](#miscattrscrewlevelincrease) | add | 8.5 | 8.5 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| crewLevel | mul | 1.085 |

### deluxRammer

| Name | Description |
|:--- |:--- |
| id | 45 |
| userString_en | Innovative Loading System |
| userString_ja | 新型装填装置 |
| longDescriptionSpecial_ja | 装填棒の高性能版で、装填時間に対するボーナスが -10% から -13.5% に強化されている。国家や車輌タイプを問わず搭載可能で、解除にはボンズが必要となる。 |
| longDescriptionSpecial_en | An improved version of Rammer ( -10%  to loading time) that provides a stronger effect:  -13.5%  to loading time. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | tankRammer |
| weight | 200 |
| price | 5,000 bonds |
| removable | false |
| tags | rammer, deluxe, firepower |
| incompatibleTags | rammer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | tankRammer_class1_user<br>tankRammer_class2_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/gunReloadTimeFactor](#miscattrsgunreloadtimefactor) | mul | 0.865 | 0.865 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunReloadTime | mul | 0.865 |

### deluxCoatedOptics

| Name | Description |
|:--- |:--- |
| id | 46 |
| userString_en | Experimental Optics |
| userString_ja | 試作型高視認性レンズ |
| longDescriptionSpecial_ja | レンズ皮膜の高性能版で、視認範囲に対するボーナスが +10% から +13.5% に強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Coated Optics ( +10%  to view range) that provides a stronger effect:  +13.5%  to view range. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | coatedOptics |
| price | 4,000 bonds |
| removable | false |
| tags | coatedOptics, deluxe, reconnaissance |
| incompatibleTags | coatedOptics |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/circularVisionRadiusFactor](#miscattrscircularvisionradiusfactor) | mul | 1.135 | 1.135 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleCircularVisionRadius | mul | 1.135 |

### deluxAimingStabilizer

| Name | Description |
|:--- |:--- |
| id | 47 |
| userString_en | Stabilizing Equipment System |
| userString_ja | 高性能安定装置 |
| longDescriptionSpecial_ja | 砲垂直安定装置の高性能版で、射撃後、および移動または砲塔旋回に伴う照準拡散に対するボーナスが -20% から -27.5% に強化されている。国家および車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Vertical Stabilizer ( -20%  to dispersion after firing, during movement and turret rotation) that provides a stronger effect:  -27.5%  to dispersion after firing, during movement and turret rotation. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | aimingStabilizer |
| weight | 100 |
| price | 5,000 bonds |
| removable | false |
| tags | aimingStabilizer, deluxe, firepower |
| incompatibleTags | aimingStabilizer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | aimingStabilizer_class1_user<br>aimingStabilizer_class2_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/additiveShotDispersionFactor](#miscattrsadditiveshotdispersionfactor) | mul | 0.725 | 0.725 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunShotDispersion | mul | 0.725 |

### deluxEnhancedAimDrives

| Name | Description |
|:--- |:--- |
| id | 48 |
| userString_en | Wear-Resistant Gun Laying Drive |
| userString_ja | 耐摩耗射撃装置 |
| longDescriptionSpecial_ja | 改良型射撃装置のさらなる高性能版で、照準速度に対するボーナスが +10% から +13.5% に強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Enhanced Gun Laying Drive ( +10%  to aiming speed) that provides a stronger effect:  +13.5%  to aiming speed. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | enhancedAimDrives |
| weight | 100 |
| price | 5,000 bonds |
| removable | false |
| tags | enhancedAimDrives, deluxe, firepower |
| incompatibleTags | enhancedAimDrives |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/gunAimingTimeFactor](#miscattrsgunaimingtimefactor) | mul | 0.881 | 0.881 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunAimSpeed | mul | 1.135 |

### trophyBasicAimDrives

| Name | Description |
|:--- |:--- |
| id | 50 |
| userString_en | Bounty Gun Laying Drive |
| userString_ja | 報酬射撃装置 |
| longDescriptionSpecial_ja | 照準時間を短縮し、射撃精度を向上させる。射撃速度と射撃精度の上昇により、照準の完全収束を待たずとも攻撃を命中させることが可能となる。 |
| longDescriptionSpecial_en | Improved aiming reduces the aiming time. A vehicle that is equipped with Enhanced Gun Laying Drive fires more accurately. In addition, it improves the rate of fire, since reloading is sometimes faster than fully aiming. |
| groupName | enhancedAimDrives |
| weight | 100 |
| price | 100,000 cr. |
| upgradePrice | 3,000,000 cr. |
| removable | false |
| tags | enhancedAimDrives, firepower, trophyBasic |
| incompatibleTags | enhancedAimDrives |

#### Factors (type=UpgradableStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/gunAimingTimeFactor](#miscattrsgunaimingtimefactor) | mul | 0.91 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunAimSpeed | mul | 1.1 |

### trophyUpgradedAimDrives

| Name | Description |
|:--- |:--- |
| id | 51 |
| userString_en | Bounty Gun Laying Drive |
| userString_ja | 報酬射撃装置 |
| longDescriptionSpecial_ja | 改良型射撃装置のさらなる高性能版で、照準速度に対するボーナスが +10% から +13.5% に強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Enhanced Gun Laying Drive ( +10%  to aiming speed) that provides a stronger effect:  +13.5%  to aiming speed. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | enhancedAimDrives |
| weight | 100 |
| price | 100,000 cr. |
| removable | false |
| tags | enhancedAimDrives, firepower, trophyUpgraded |
| incompatibleTags | enhancedAimDrives |

#### Factors (type=UpgradedStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/gunAimingTimeFactor](#miscattrsgunaimingtimefactor) | mul | 0.89 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunAimSpeed | mul | 1.125 |

### trophyBasicTankRammer

| Name | Description |
|:--- |:--- |
| id | 52 |
| userString_en | Bounty Rammer |
| userString_ja | 報酬装填棒 |
| longDescriptionSpecial_ja | 射撃速度を向上させる。射撃速度は最も重要な特性値の1つであるため、優先的に搭載を検討すべき拡張パーツ。弾倉式自動装填システムを備えた車輌には搭載できない。 |
| longDescriptionSpecial_en | Rate of fire is one of the most important characteristics of your vehicle. This equipment is quite beneficial in a battle and should be mounted on your vehicles. However, rammers cannot be mounted on vehicles with a magazine loading system. |
| groupName | tankRammer |
| weight | 200 |
| price | 100,000 cr. |
| upgradePrice | 3,000,000 cr. |
| removable | false |
| tags | rammer, firepower, trophyBasic |
| incompatibleTags | rammer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | tankRammer_class1_user<br>tankRammer_class2_user |

#### Factors (type=UpgradableStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/gunReloadTimeFactor](#miscattrsgunreloadtimefactor) | mul | 0.9 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunReloadTime | mul | 0.9 |

### trophyUpgradedTankRammer

| Name | Description |
|:--- |:--- |
| id | 53 |
| userString_en | Bounty Rammer |
| userString_ja | 報酬装填棒 |
| longDescriptionSpecial_ja | 装填棒の高性能版で、装填時間に対するボーナスが -10% から -13.5% に強化されている。国家や車輌タイプを問わず搭載可能で、解除にはボンズが必要となる。 |
| longDescriptionSpecial_en | An improved version of Rammer ( -10%  to loading time) that provides a stronger effect:  -13.5%  to loading time. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | tankRammer |
| weight | 200 |
| price | 100,000 cr. |
| removable | false |
| tags | rammer, firepower, trophyUpgraded |
| incompatibleTags | rammer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | tankRammer_class1_user<br>tankRammer_class2_user |

#### Factors (type=UpgradedStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/gunReloadTimeFactor](#miscattrsgunreloadtimefactor) | mul | 0.875 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunReloadTime | mul | 0.875 |

### trophyBasicImprovedVentilation

| Name | Description |
|:--- |:--- |
| id | 54 |
| userString_en | Bounty Ventilation |
| userString_ja | 報酬換気装置 |
| longDescriptionSpecial_ja | 改良型換気装置のさらなる高性能版で、全搭乗員のスキルに対するボーナスが +5% から +8.5% に強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Ventilation ( +5% to all crew skills) that provides a stronger effect:  +8.5%  to all crew skills. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | improvedVentilation |
| weight | 100 |
| price | 100,000 cr. |
| upgradePrice | 3,000,000 cr. |
| removable | false |
| tags | ventilation, trophyBasic, armor, firepower, camouflage, reconnaissance, mobility |
| incompatibleTags | ventilation |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | improvedVentilation_class1_user<br>improvedVentilation_class2_user<br>improvedVentilation_class3_user |

#### Factors (type=UpgradableStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/crewLevelIncrease](#miscattrscrewlevelincrease) | add | 5.0 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| crewLevel | mul | 1.05 |

### trophyUpgradedImprovedVentilation

| Name | Description |
|:--- |:--- |
| id | 55 |
| userString_en | Bounty Ventilation |
| userString_ja | 報酬換気装置 |
| longDescriptionSpecial_ja | 改良型換気装置のさらなる高性能版で、全搭乗員のスキルに対するボーナスが +5% から +8.5% に強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Ventilation ( +5% to all crew skills) that provides a stronger effect:  +8.5%  to all crew skills. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | improvedVentilation |
| weight | 100 |
| price | 100,000 cr. |
| removable | false |
| tags | ventilation, trophyUpgraded, armor, firepower, camouflage, reconnaissance, mobility |
| incompatibleTags | ventilation |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | improvedVentilation_class1_user<br>improvedVentilation_class2_user<br>improvedVentilation_class3_user |

#### Factors (type=UpgradedStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/crewLevelIncrease](#miscattrscrewlevelincrease) | add | 7.5 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| crewLevel | mul | 1.075 |

### trophyBasicAimingStabilizer

| Name | Description |
|:--- |:--- |
| id | 56 |
| userString_en | Bounty Stabilizer |
| userString_ja | 報酬安定装置 |
| longDescriptionSpecial_ja | 砲垂直安定装置の高性能版で、射撃後、および移動または砲塔旋回に伴う照準拡散に対するボーナスが -20% から -27.5% に強化されている。国家および車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Vertical Stabilizer ( -20%  to dispersion after firing, during movement and turret rotation) that provides a stronger effect:  -27.5%  to dispersion after firing, during movement and turret rotation. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | aimingStabilizer |
| weight | 100 |
| price | 100,000 cr. |
| upgradePrice | 3,000,000 cr. |
| removable | false |
| tags | aimingStabilizer, trophyBasic, firepower |
| incompatibleTags | aimingStabilizer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | aimingStabilizer_class1_user<br>aimingStabilizer_class2_user |

#### Factors (type=UpgradableStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/additiveShotDispersionFactor](#miscattrsadditiveshotdispersionfactor) | mul | 0.8 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunShotDispersion | mul | 0.8 |

### trophyUpgradedAimingStabilizer

| Name | Description |
|:--- |:--- |
| id | 57 |
| userString_en | Bounty Stabilizer |
| userString_ja | 報酬安定装置 |
| longDescriptionSpecial_ja | 砲垂直安定装置の高性能版で、射撃後、および移動または砲塔旋回に伴う照準拡散に対するボーナスが -20% から -27.5% に強化されている。国家および車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Vertical Stabilizer ( -20%  to dispersion after firing, during movement and turret rotation) that provides a stronger effect:  -27.5%  to dispersion after firing, during movement and turret rotation. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | aimingStabilizer |
| weight | 100 |
| price | 100,000 cr. |
| removable | false |
| tags | aimingStabilizer, trophyUpgraded, firepower |
| incompatibleTags | aimingStabilizer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | aimingStabilizer_class1_user<br>aimingStabilizer_class2_user |

#### Factors (type=UpgradedStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/additiveShotDispersionFactor](#miscattrsadditiveshotdispersionfactor) | mul | 0.75 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunShotDispersion | mul | 0.75 |

### trophyBasicCoatedOptics

| Name | Description |
|:--- |:--- |
| id | 58 |
| userString_en | Bounty Optics |
| userString_ja | 報酬レンズ |
| longDescriptionSpecial_ja | レンズ皮膜の高性能版で、視認範囲に対するボーナスが +10% から +13.5% に強化されている。国家や車輌タイプを問わず搭載可能だが、解除にはボンズが必要。 |
| longDescriptionSpecial_en | An improved version of Coated Optics ( +10%  to view range) that provides a stronger effect:  +13.5%  to view range. This equipment is suitable for vehicles of all nations and types. Can be demounted using bonds only. |
| groupName | coatedOptics |
| price | 100,000 cr. |
| upgradePrice | 3,000,000 cr. |
| removable | false |
| tags | coatedOptics, trophyBasic, reconnaissance |
| incompatibleTags | coatedOptics |

#### Factors (type=UpgradableStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/circularVisionRadiusFactor](#miscattrscircularvisionradiusfactor) | mul | 1.1 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleCircularVisionRadius | mul | 1.1 |

### trophyUpgradedCoatedOptics

| Name | Description |
|:--- |:--- |
| id | 59 |
| userString_en | Bounty Optics |
| userString_ja | 報酬レンズ |
| longDescriptionSpecial_ja | trophyCoatedOptics/long_special |
| longDescriptionSpecial_en | trophyCoatedOptics/long_special |
| groupName | coatedOptics |
| price | 100,000 cr. |
| removable | false |
| tags | coatedOptics, trophyUpgraded, reconnaissance |
| incompatibleTags | coatedOptics |

#### Factors (type=UpgradedStaticDevice)

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| [miscAttrs/circularVisionRadiusFactor](#miscattrscircularvisionradiusfactor) | mul | 1.125 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleCircularVisionRadius | mul | 1.125 |

### trophyBasicImprovedConfiguration

| Name | Description |
|:--- |:--- |
| id | 105 |
| userString_en | Bounty Protection Technology |
| userString_ja | 報酬モジュール構造 |
| shortDescriptionSpecial_ja | 修理速度を向上させると同時に、燃料タンク、弾薬庫、エンジンを保護する。 |
| longDescriptionSpecial_ja | 修理速度に加えて、燃料タンク、弾薬庫、およびエンジンの耐久性を向上させる。また、モジュール損傷時の装填時間やエンジン出力に対するペナルティーを軽減するほか、エンジンの火災発生率を低下させる。さらに、1戦につき1度、燃料タンクの発火、弾薬庫の誘爆、およびエンジンの大破を防ぐ。スキル「修理」、またはパーク「弾薬庫保護」や「こまめな手入れ」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Accelerates repairs, protects ammo rack, fuel tanks, and engine |
| longDescriptionSpecial_en | Accelerates repairs. Increases the durability of the ammo rack, fuel tanks, and engine. Reduces the loading time penalty and loss of engine power upon the damaging of the corresponding modules. Reduces the chance of engine fire. Prevents one fuel tank fire, ammo rack explosion, and engine destruction in a battle upon the destruction of the corresponding module. It is more effective when combined with the Repairs, Safe Stowage, and Preventative Maintenance perks. |
| groupName | improvedConfiguration |
| weight | 50 |
| price | 100,000 cr. |
| upgradePrice | 3,000,000 cr. |
| removable | false |
| tags | improvedConfiguration, armor, trophyBasic |
| incompatibleTags | improvedConfiguration |

#### Factors (type=UpgradableImprovedConfiguration)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [engineReduceFineFactor](#enginereducefinefactor) | mul | 0.5 | 0.5 |
| [ammoBayReduceFineFactor](#ammobayreducefinefactor) | mul | 0.5 | 0.5 |
| [miscAttrs/repairSpeedFactor](#miscattrsrepairspeedfactor) | mul | 1.25 | 1.25 |
| [miscAttrs/ammoBayHealthFactor](#miscattrsammobayhealthfactor) | mul | 2.0 | 2.0 |
| [miscAttrs/fuelTankHealthFactor](#miscattrsfueltankhealthfactor) | mul | 2.0 | 2.0 |
| [miscAttrs/engineHealthFactor](#miscattrsenginehealthfactor) | mul | 2.0 | 2.0 |
| [miscAttrs/fireStartingChanceFactor](#miscattrsfirestartingchancefactor) | mul | 0.5 | 0.5 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleRepairSpeed | mul | 1.25 |
| vehicleAmmoBayEngineFuelStrength | mul | 2.0 |
| vehPenaltyForDamageEngineAndCombat | mul | 0.5 |
| vehicleFireChance | mul | 0.5 |

### trophyUpgradedImprovedConfiguration

| Name | Description |
|:--- |:--- |
| id | 106 |
| userString_en | Bounty Protection Technology |
| userString_ja | 報酬モジュール構造 |
| shortDescriptionSpecial_ja | 修理速度を向上させると同時に、燃料タンク、弾薬庫、エンジンを保護する。 |
| longDescriptionSpecial_ja | 修理速度に加えて、燃料タンク、弾薬庫、およびエンジンの耐久性を向上させる。また、モジュール損傷時の装填時間やエンジン出力に対するペナルティーを軽減するほか、エンジンの火災発生率を低下させる。さらに、1戦につき1度、燃料タンクの発火、弾薬庫の誘爆、およびエンジンの大破を防ぐ。スキル「修理」、またはパーク「弾薬庫保護」や「こまめな手入れ」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Accelerates repairs, protects ammo rack, fuel tanks, and engine |
| longDescriptionSpecial_en | Accelerates repairs. Increases the durability of the ammo rack, fuel tanks, and engine. Reduces the loading time penalty and loss of engine power upon the damaging of the corresponding modules. Reduces the chance of engine fire. Prevents one fuel tank fire, ammo rack explosion, and engine destruction in a battle upon the destruction of the corresponding module. It is more effective when combined with the Repairs, Safe Stowage, and Preventative Maintenance perks. |
| groupName | improvedConfiguration |
| weight | 50 |
| price | 100,000 cr. |
| removable | false |
| tags | improvedConfiguration, armor, trophyUpgraded |
| incompatibleTags | improvedConfiguration |

#### Factors (type=UpgradedImprovedConfiguration)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [engineReduceFineFactor](#enginereducefinefactor) | mul | 0.3 | 0.3 |
| [ammoBayReduceFineFactor](#ammobayreducefinefactor) | mul | 0.3 | 0.3 |
| [miscAttrs/repairSpeedFactor](#miscattrsrepairspeedfactor) | mul | 1.4 | 1.4 |
| [miscAttrs/ammoBayHealthFactor](#miscattrsammobayhealthfactor) | mul | 2.5 | 2.5 |
| [miscAttrs/fuelTankHealthFactor](#miscattrsfueltankhealthfactor) | mul | 2.5 | 2.5 |
| [miscAttrs/engineHealthFactor](#miscattrsenginehealthfactor) | mul | 2.5 | 2.5 |
| [miscAttrs/fireStartingChanceFactor](#miscattrsfirestartingchancefactor) | mul | 0.3 | 0.3 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleRepairSpeed | mul | 1.4 |
| vehicleAmmoBayEngineFuelStrength | mul | 2.5 |
| vehPenaltyForDamageEngineAndCombat | mul | 0.3 |
| vehicleFireChance | mul | 0.3 |

### improvedConfiguration_tier2

| Name | Description |
|:--- |:--- |
| id | 61 |
| userString_en | Modified Configuration Class 2 |
| userString_ja | 改修型モジュール クラス2 |
| shortDescriptionSpecial_ja | 修理速度を向上させると同時に、燃料タンク、弾薬庫、エンジンを保護する。 |
| longDescriptionSpecial_ja | 修理速度に加えて、燃料タンク、弾薬庫、およびエンジンの耐久性を向上させる。また、モジュール損傷時の装填時間やエンジン出力に対するペナルティーを軽減するほか、エンジンの火災発生率を低下させる。さらに、1戦につき1度、燃料タンクの発火、弾薬庫の誘爆、およびエンジンの大破を防ぐ。スキル「修理」、またはパーク「弾薬庫保護」や「こまめな手入れ」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Accelerates repairs, protects ammo rack, fuel tanks, and engine |
| longDescriptionSpecial_en | Accelerates repairs. Increases the durability of the ammo rack, fuel tanks, and engine. Reduces the loading time penalty and loss of engine power upon the damaging of the corresponding modules. Reduces the chance of engine fire. Prevents one fuel tank fire, ammo rack explosion, and engine destruction in a battle upon the destruction of the corresponding module. It is more effective when combined with the Repairs, Safe Stowage, and Preventative Maintenance perks. |
| weight | 50 |
| price | 200,000 cr. |
| removable | false |
| tags | improvedConfiguration, armor |
| incompatibleTags | improvedConfiguration |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 5 | 7 |

#### Factors (type=ImprovedConfiguration)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [engineReduceFineFactor](#enginereducefinefactor) | mul | 0.5 | 0.35 |
| [ammoBayReduceFineFactor](#ammobayreducefinefactor) | mul | 0.5 | 0.35 |
| [miscAttrs/repairSpeedFactor](#miscattrsrepairspeedfactor) | mul | 1.25 | 1.35 |
| [miscAttrs/ammoBayHealthFactor](#miscattrsammobayhealthfactor) | mul | 2.0 | 2.5 |
| [miscAttrs/fuelTankHealthFactor](#miscattrsfueltankhealthfactor) | mul | 2.0 | 2.5 |
| [miscAttrs/engineHealthFactor](#miscattrsenginehealthfactor) | mul | 2.0 | 2.5 |
| [miscAttrs/fireStartingChanceFactor](#miscattrsfirestartingchancefactor) | mul | 0.5 | 0.35 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleRepairSpeed | mul | 1.25 | 1.35 |
| vehicleAmmoBayEngineFuelStrength | mul | 2.0 | 2.5 |
| vehPenaltyForDamageEngineAndCombat | mul | 0.5 | 0.35 |
| vehicleFireChance | mul | 0.5 | 0.35 |

### improvedConfiguration_tier1

| Name | Description |
|:--- |:--- |
| id | 62 |
| userString_en | Modified Configuration Class 1 |
| userString_ja | 改修型モジュール クラス1 |
| shortDescriptionSpecial_ja | 修理速度を向上させると同時に、燃料タンク、弾薬庫、エンジンを保護する。 |
| longDescriptionSpecial_ja | 修理速度に加えて、燃料タンク、弾薬庫、およびエンジンの耐久性を向上させる。また、モジュール損傷時の装填時間やエンジン出力に対するペナルティーを軽減するほか、エンジンの火災発生率を低下させる。さらに、1戦につき1度、燃料タンクの発火、弾薬庫の誘爆、およびエンジンの大破を防ぐ。スキル「修理」、またはパーク「弾薬庫保護」や「こまめな手入れ」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Accelerates repairs, protects ammo rack, fuel tanks, and engine |
| longDescriptionSpecial_en | Accelerates repairs. Increases the durability of the ammo rack, fuel tanks, and engine. Reduces the loading time penalty and loss of engine power upon the damaging of the corresponding modules. Reduces the chance of engine fire. Prevents one fuel tank fire, ammo rack explosion, and engine destruction in a battle upon the destruction of the corresponding module. It is more effective when combined with the Repairs, Safe Stowage, and Preventative Maintenance perks. |
| weight | 50 |
| price | 600,000 cr. |
| removable | false |
| tags | improvedConfiguration, armor |
| incompatibleTags | improvedConfiguration |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 8 | 10 |

#### Factors (type=ImprovedConfiguration)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [engineReduceFineFactor](#enginereducefinefactor) | mul | 0.5 | 0.35 |
| [ammoBayReduceFineFactor](#ammobayreducefinefactor) | mul | 0.5 | 0.35 |
| [miscAttrs/repairSpeedFactor](#miscattrsrepairspeedfactor) | mul | 1.25 | 1.35 |
| [miscAttrs/ammoBayHealthFactor](#miscattrsammobayhealthfactor) | mul | 2.0 | 2.5 |
| [miscAttrs/fuelTankHealthFactor](#miscattrsfueltankhealthfactor) | mul | 2.0 | 2.5 |
| [miscAttrs/engineHealthFactor](#miscattrsenginehealthfactor) | mul | 2.0 | 2.5 |
| [miscAttrs/fireStartingChanceFactor](#miscattrsfirestartingchancefactor) | mul | 0.5 | 0.35 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleRepairSpeed | mul | 1.25 | 1.35 |
| vehicleAmmoBayEngineFuelStrength | mul | 2.0 | 2.5 |
| vehPenaltyForDamageEngineAndCombat | mul | 0.5 | 0.35 |
| vehicleFireChance | mul | 0.5 | 0.35 |

### antifragmentationLining_tier4

| Name | Description |
|:--- |:--- |
| id | 63 |
| userString_en | Light Spall Liner |
| userString_ja | 内張り装甲 (小) |
| shortDescriptionSpecial_ja | 車輌および搭乗員をHE弾、負傷、スタン、衝突から保護する。 |
| longDescriptionSpecial_ja | 衝突やHE弾の爆発によるダメージを軽減する。装甲厚に優れ、接近戦や体当たりを行うことが多い重戦車に搭載すると特に効果的。 |
| shortDescriptionSpecial_en | Protects the vehicle and crew from HE shells, injuries, stunning, and ramming |
| longDescriptionSpecial_en | Spall liners absorb damage; this means they reduce the damage from ramming and High-Explosive shells. Spall liners will be beneficial to heavy tanks with reliable armor and huge ramming potential that fight mainly in close combat. |
| weight | 250 |
| price | 50,000 cr. |
| removable | false |
| tags | antifragmentationLining, armor |
| incompatibleTags | antifragmentationLining |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | antifragmentationLining_light_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/antifragmentationLiningFactor](#miscattrsantifragmentationliningfactor) | mul | 1.5 | 1.6 |
| [miscAttrs/crewChanceToHitFactor](#miscattrscrewchancetohitfactor) | mul | 0.5 | 0.4 |
| [miscAttrs/stunResistanceDuration](#miscattrsstunresistanceduration) | add | 0.1 | 0.15 |
| [miscAttrs/repeatedStunDurationFactor](#miscattrsrepeatedstundurationfactor) | mul | 0.8 | 0.75 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleRamOrExplosionDamageResistance | mul | 0.5 | 0.4 |
| crewHitChance | mul | 1.5 | 1.6 |
| crewStunDuration | mul | 0.9 | 0.85 |
| crewRepeatedStunDuration | mul | 0.8 | 0.75 |

### antifragmentationLining_tier3

| Name | Description |
|:--- |:--- |
| id | 64 |
| userString_en | Medium Spall Liner |
| userString_ja | 内張り装甲 (中) |
| shortDescriptionSpecial_ja | 車輌および搭乗員をHE弾、負傷、スタン、衝突から保護する。 |
| longDescriptionSpecial_ja | 衝突やHE弾の爆発によるダメージを軽減する。装甲厚に優れ、接近戦や体当たりを行うことが多い重戦車に搭載すると特に効果的。 |
| shortDescriptionSpecial_en | Protects the vehicle and crew from HE shells, injuries, stunning, and ramming |
| longDescriptionSpecial_en | Spall liners absorb damage; this means they reduce the damage from ramming and High-Explosive shells. Spall liners will be beneficial to heavy tanks with reliable armor and huge ramming potential that fight mainly in close combat. |
| weight | 500 |
| price | 200,000 cr. |
| removable | false |
| tags | antifragmentationLining, armor |
| incompatibleTags | antifragmentationLining |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | antifragmentationLining_medium_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/antifragmentationLiningFactor](#miscattrsantifragmentationliningfactor) | mul | 1.5 | 1.6 |
| [miscAttrs/crewChanceToHitFactor](#miscattrscrewchancetohitfactor) | mul | 0.5 | 0.4 |
| [miscAttrs/stunResistanceDuration](#miscattrsstunresistanceduration) | add | 0.1 | 0.15 |
| [miscAttrs/repeatedStunDurationFactor](#miscattrsrepeatedstundurationfactor) | mul | 0.8 | 0.75 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleRamOrExplosionDamageResistance | mul | 0.5 | 0.4 |
| crewHitChance | mul | 1.5 | 1.6 |
| crewStunDuration | mul | 0.9 | 0.85 |
| crewRepeatedStunDuration | mul | 0.8 | 0.75 |

### antifragmentationLining_tier2

| Name | Description |
|:--- |:--- |
| id | 65 |
| userString_en | Heavy Spall Liner |
| userString_ja | 内張り装甲 (大) |
| shortDescriptionSpecial_ja | 車輌および搭乗員をHE弾、負傷、スタン、衝突から保護する。 |
| longDescriptionSpecial_ja | 衝突やHE弾の爆発によるダメージを軽減する。装甲厚に優れ、接近戦や体当たりを行うことが多い重戦車に搭載すると特に効果的。 |
| shortDescriptionSpecial_en | Protects the vehicle and crew from HE shells, injuries, stunning, and ramming |
| longDescriptionSpecial_en | Spall liners absorb damage; this means they reduce the damage from ramming and High-Explosive shells. Spall liners will be beneficial to heavy tanks with reliable armor and huge ramming potential that fight mainly in close combat. |
| weight | 1000 |
| price | 500,000 cr. |
| removable | false |
| tags | antifragmentationLining, armor |
| incompatibleTags | antifragmentationLining |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | antifragmentationLining_heavy_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/antifragmentationLiningFactor](#miscattrsantifragmentationliningfactor) | mul | 1.5 | 1.6 |
| [miscAttrs/crewChanceToHitFactor](#miscattrscrewchancetohitfactor) | mul | 0.5 | 0.4 |
| [miscAttrs/stunResistanceDuration](#miscattrsstunresistanceduration) | add | 0.1 | 0.15 |
| [miscAttrs/repeatedStunDurationFactor](#miscattrsrepeatedstundurationfactor) | mul | 0.8 | 0.75 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleRamOrExplosionDamageResistance | mul | 0.5 | 0.4 |
| crewHitChance | mul | 1.5 | 1.6 |
| crewStunDuration | mul | 0.9 | 0.85 |
| crewRepeatedStunDuration | mul | 0.8 | 0.75 |

### antifragmentationLining_tier1

| Name | Description |
|:--- |:--- |
| id | 66 |
| userString_en | Superheavy Spall Liner |
| userString_ja | 内張り装甲 (特大) |
| shortDescriptionSpecial_ja | 車輌および搭乗員をHE弾、負傷、スタン、衝突から保護する。 |
| longDescriptionSpecial_ja | 衝突やHE弾の爆発によるダメージを軽減する。装甲厚に優れ、接近戦や体当たりを行うことが多い重戦車に搭載すると特に効果的。 |
| shortDescriptionSpecial_en | Protects the vehicle and crew from HE shells, injuries, stunning, and ramming |
| longDescriptionSpecial_en | Spall liners absorb damage; this means they reduce the damage from ramming and High-Explosive shells. Spall liners will be beneficial to heavy tanks with reliable armor and huge ramming potential that fight mainly in close combat. |
| weight | 1500 |
| price | 750,000 cr. |
| removable | false |
| tags | antifragmentationLining, armor |
| incompatibleTags | antifragmentationLining |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | antifragmentationLining_superheavy_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/antifragmentationLiningFactor](#miscattrsantifragmentationliningfactor) | mul | 1.5 | 1.6 |
| [miscAttrs/crewChanceToHitFactor](#miscattrscrewchancetohitfactor) | mul | 0.5 | 0.4 |
| [miscAttrs/stunResistanceDuration](#miscattrsstunresistanceduration) | add | 0.1 | 0.15 |
| [miscAttrs/repeatedStunDurationFactor](#miscattrsrepeatedstundurationfactor) | mul | 0.8 | 0.75 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleRamOrExplosionDamageResistance | mul | 0.5 | 0.4 |
| crewHitChance | mul | 1.5 | 1.6 |
| crewStunDuration | mul | 0.9 | 0.85 |
| crewRepeatedStunDuration | mul | 0.8 | 0.75 |

### stereoscope_tier3

| Name | Description |
|:--- |:--- |
| id | 67 |
| userString_en | Binocular Telescope Class 3 |
| userString_ja | 双眼鏡 クラス3 |
| shortDescriptionSpecial_ja | stereoscope/short_special |
| longDescriptionSpecial_ja | 移動停止から3秒経過後に視認範囲を拡大する。視認範囲の広さは、戦闘を有利に進める上での重要な要素の1つ。特に、車高が低く、高精度の主砲を備えた駆逐戦車に搭載すると特に効果的。 |
| shortDescriptionSpecial_en | stereoscope/short_special |
| longDescriptionSpecial_en | A good view range provides a solid advantage to vehicles in battle. This type of equipment is beneficial for tank destroyers with low silhouettes and accurate guns. It activates 3 seconds after the vehicle stops moving. |
| weight | 50 |
| price | 50,000 cr. |
| removable | false |
| tags | stereoscope, reconnaissance |
| incompatibleTags | stereoscope |
| activateWhenStillSec | 3.0 |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include |  | 2 | 4 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=Stereoscope)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [circularVisionRadius](#circularvisionradius) | mul | 1.25 | 1.275 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStillCircularVisionRadius | mul | 1.25 | 1.275 |

### stereoscope_tier2

| Name | Description |
|:--- |:--- |
| id | 68 |
| userString_en | Binocular Telescope Class 2 |
| userString_ja | 双眼鏡 クラス2 |
| shortDescriptionSpecial_ja | stereoscope/short_special |
| longDescriptionSpecial_ja | 移動停止から3秒経過後に視認範囲を拡大する。視認範囲の広さは、戦闘を有利に進める上での重要な要素の1つ。特に、車高が低く、高精度の主砲を備えた駆逐戦車に搭載すると特に効果的。 |
| shortDescriptionSpecial_en | stereoscope/short_special |
| longDescriptionSpecial_en | A good view range provides a solid advantage to vehicles in battle. This type of equipment is beneficial for tank destroyers with low silhouettes and accurate guns. It activates 3 seconds after the vehicle stops moving. |
| weight | 50 |
| price | 200,000 cr. |
| removable | false |
| tags | stereoscope, reconnaissance |
| incompatibleTags | stereoscope |
| activateWhenStillSec | 3.0 |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include |  | 5 | 7 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=Stereoscope)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [circularVisionRadius](#circularvisionradius) | mul | 1.25 | 1.275 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStillCircularVisionRadius | mul | 1.25 | 1.275 |

### stereoscope_tier1

| Name | Description |
|:--- |:--- |
| id | 69 |
| userString_en | Binocular Telescope Class 1 |
| userString_ja | 双眼鏡 クラス1 |
| shortDescriptionSpecial_ja | stereoscope/short_special |
| longDescriptionSpecial_ja | 移動停止から3秒経過後に視認範囲を拡大する。視認範囲の広さは、戦闘を有利に進める上での重要な要素の1つ。特に、車高が低く、高精度の主砲を備えた駆逐戦車に搭載すると特に効果的。 |
| shortDescriptionSpecial_en | stereoscope/short_special |
| longDescriptionSpecial_en | A good view range provides a solid advantage to vehicles in battle. This type of equipment is beneficial for tank destroyers with low silhouettes and accurate guns. It activates 3 seconds after the vehicle stops moving. |
| weight | 50 |
| price | 600,000 cr. |
| removable | false |
| tags | stereoscope, reconnaissance |
| incompatibleTags | stereoscope |
| activateWhenStillSec | 3.0 |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include |  | 8 | 10 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=Stereoscope)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [circularVisionRadius](#circularvisionradius) | mul | 1.25 | 1.275 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStillCircularVisionRadius | mul | 1.25 | 1.275 |

### coatedOptics_tier3

| Name | Description |
|:--- |:--- |
| id | 70 |
| userString_en | Coated Optics Class 3 |
| userString_ja | レンズ皮膜 クラス3 |
| longDescriptionSpecial_ja | レンズ皮膜（タイプを問わない）の効力は、双眼鏡と比べるとやや見劣りするものの、移動中でも無効になることがありません。そのため、レンズ皮膜は積極的に動き回る軽戦車や中戦車に特に有用です。 |
| longDescriptionSpecial_en | Unlike Binocular Telescope, Coated Optics (of any type) provides a smaller yet permanent bonus that does not disappear with vehicle movement. As a result, Coated Optics is highly beneficial for vehicles with active gameplay—light and medium tanks. |
| price | 50,000 cr. |
| removable | false |
| tags | coatedOptics, reconnaissance |
| incompatibleTags | coatedOptics |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 2 | 4 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/circularVisionRadiusFactor](#miscattrscircularvisionradiusfactor) | mul | 1.1 | 1.115 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleCircularVisionRadius | mul | 1.1 | 1.115 |

### coatedOptics_tier2

| Name | Description |
|:--- |:--- |
| id | 71 |
| userString_en | Coated Optics Class 2 |
| userString_ja | レンズ皮膜 クラス2 |
| longDescriptionSpecial_ja | レンズ皮膜（タイプを問わない）の効力は、双眼鏡と比べるとやや見劣りするものの、移動中でも無効になることがありません。そのため、レンズ皮膜は積極的に動き回る軽戦車や中戦車に特に有用です。 |
| longDescriptionSpecial_en | Unlike Binocular Telescope, Coated Optics (of any type) provides a smaller yet permanent bonus that does not disappear with vehicle movement. As a result, Coated Optics is highly beneficial for vehicles with active gameplay—light and medium tanks. |
| price | 200,000 cr. |
| removable | false |
| tags | coatedOptics, reconnaissance |
| incompatibleTags | coatedOptics |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 5 | 7 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/circularVisionRadiusFactor](#miscattrscircularvisionradiusfactor) | mul | 1.1 | 1.115 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleCircularVisionRadius | mul | 1.1 | 1.115 |

### coatedOptics_tier1

| Name | Description |
|:--- |:--- |
| id | 72 |
| userString_en | Coated Optics Class 1 |
| userString_ja | レンズ皮膜 クラス1 |
| longDescriptionSpecial_ja | レンズ皮膜（タイプを問わない）の効力は、双眼鏡と比べるとやや見劣りするものの、移動中でも無効になることがありません。そのため、レンズ皮膜は積極的に動き回る軽戦車や中戦車に特に有用です。 |
| longDescriptionSpecial_en | Unlike Binocular Telescope, Coated Optics (of any type) provides a smaller yet permanent bonus that does not disappear with vehicle movement. As a result, Coated Optics is highly beneficial for vehicles with active gameplay—light and medium tanks. |
| price | 600,000 cr. |
| removable | false |
| tags | coatedOptics, reconnaissance |
| incompatibleTags | coatedOptics |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 8 | 10 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/circularVisionRadiusFactor](#miscattrscircularvisionradiusfactor) | mul | 1.1 | 1.115 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleCircularVisionRadius | mul | 1.1 | 1.115 |

### camouflageNet_tier3

| Name | Description |
|:--- |:--- |
| id | 73 |
| userString_en | Camouflage Net Class 3 |
| userString_ja | 迷彩ネット クラス3 |
| shortDescriptionSpecial_ja | 静止中の隠蔽率を向上させる。 |
| longDescriptionSpecial_ja | 静止中の隠蔽率を向上させる。効果は移動停止から3秒経過後に発揮され、具体的なボーナス値は車輌タイプにより異なる。低車高の自走砲や駆逐戦車など、隠蔽性の高い車輌に搭載すると特に有用。また、スキル「カモフラージュ」と併用すると、効果が累算される。 |
| shortDescriptionSpecial_en | Increases the concealment of a stationary vehicle |
| longDescriptionSpecial_en | The effectiveness depends on the vehicle type. Camouflage Net is mainly beneficial for stealthy vehicles: SPGs and tank destroyers with a low silhouette. It increases the effect of the Concealment skill. Combining them will provide a good bonus for your vehicle. Camouflage Net activates 3 seconds after the vehicle stops moving. |
| weight | 50 |
| price | 50,000 cr. |
| removable | false |
| tags | camouflageNet, camouflage |
| incompatibleTags | camouflageNet |
| activateWhenStillSec | 3.0 |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 2 | 4 |

#### Factors (type=CamouflageNet)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [invisibilityBonus](#invisibilitybonus) | override | 0.15 | 0.175 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStillCamouflage | mul | 1.15 | 1.175 |
| vehicleStillCamouflage | mul | 1.1 | 1.125 |
| vehicleStillCamouflage | mul | 1.05 | 1.075 |

### camouflageNet_tier2

| Name | Description |
|:--- |:--- |
| id | 74 |
| userString_en | Camouflage Net Class 2 |
| userString_ja | 迷彩ネット クラス2 |
| shortDescriptionSpecial_ja | 静止中の隠蔽率を向上させる。 |
| longDescriptionSpecial_ja | 静止中の隠蔽率を向上させる。効果は移動停止から3秒経過後に発揮され、具体的なボーナス値は車輌タイプにより異なる。低車高の自走砲や駆逐戦車など、隠蔽性の高い車輌に搭載すると特に有用。また、スキル「カモフラージュ」と併用すると、効果が累算される。 |
| shortDescriptionSpecial_en | Increases the concealment of a stationary vehicle |
| longDescriptionSpecial_en | The effectiveness depends on the vehicle type. Camouflage Net is mainly beneficial for stealthy vehicles: SPGs and tank destroyers with a low silhouette. It increases the effect of the Concealment skill. Combining them will provide a good bonus for your vehicle. Camouflage Net activates 3 seconds after the vehicle stops moving. |
| weight | 50 |
| price | 100,000 cr. |
| removable | false |
| tags | camouflageNet, camouflage |
| incompatibleTags | camouflageNet |
| activateWhenStillSec | 3.0 |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 5 | 10 |

#### Factors (type=CamouflageNet)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [invisibilityBonus](#invisibilitybonus) | override | 0.15 | 0.175 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStillCamouflage | mul | 1.15 | 1.175 |
| vehicleStillCamouflage | mul | 1.1 | 1.125 |
| vehicleStillCamouflage | mul | 1.05 | 1.075 |

### improvedVentilation_tier3

| Name | Description |
|:--- |:--- |
| id | 75 |
| userString_en | Improved Ventilation Class 3 |
| userString_ja | 改良型換気装置 クラス 3 |
| longDescriptionSpecial_ja | 視認範囲、射撃速度、照準速度、加速性能などの車輌性能を向上させる。ただし、はっきりとした効果が表れるのは、搭乗員がパーク「戦友」を100%まで訓練済みの場合のみ。 |
| longDescriptionSpecial_en | Improves a number of characteristics of your vehicle: view range, fire rate, aiming time, dynamic characteristics, and more. However, the provided bonus from this equipment will only be noticeable in combination with the Brothers in Arms perk trained to 100% by your crew. |
| weight | 100 |
| price | 50,000 cr. |
| removable | false |
| tags | ventilation, armor, firepower, camouflage, reconnaissance, mobility |
| incompatibleTags | ventilation |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | improvedVentilation_class3_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/crewLevelIncrease](#miscattrscrewlevelincrease) | add | 5.0 | 6.0 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| crewLevel | mul | 1.05 | 1.060 |

### improvedVentilation_tier2

| Name | Description |
|:--- |:--- |
| id | 76 |
| userString_en | Improved Ventilation Class 2 |
| userString_ja | 改良型換気装置 クラス 2 |
| longDescriptionSpecial_ja | 視認範囲、射撃速度、照準速度、加速性能などの車輌性能を向上させる。ただし、はっきりとした効果が表れるのは、搭乗員がパーク「戦友」を100%まで訓練済みの場合のみ。 |
| longDescriptionSpecial_en | Improves a number of characteristics of your vehicle: view range, fire rate, aiming time, dynamic characteristics, and more. However, the provided bonus from this equipment will only be noticeable in combination with the Brothers in Arms perk trained to 100% by your crew. |
| weight | 150 |
| price | 200,000 cr. |
| removable | false |
| tags | ventilation, armor, firepower, camouflage, reconnaissance, mobility |
| incompatibleTags | ventilation |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | improvedVentilation_class2_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/crewLevelIncrease](#miscattrscrewlevelincrease) | add | 5.0 | 6.0 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| crewLevel | mul | 1.05 | 1.06 |

### improvedVentilation_tier1

| Name | Description |
|:--- |:--- |
| id | 77 |
| userString_en | Improved Ventilation Class 1 |
| userString_ja | 改良型換気装置 クラス 1 |
| longDescriptionSpecial_ja | 視認範囲、射撃速度、照準速度、加速性能などの車輌性能を向上させる。ただし、はっきりとした効果が表れるのは、搭乗員がパーク「戦友」を100%まで訓練済みの場合のみ。 |
| longDescriptionSpecial_en | Improves a number of characteristics of your vehicle: view range, fire rate, aiming time, dynamic characteristics, and more. However, the provided bonus from this equipment will only be noticeable in combination with the Brothers in Arms perk trained to 100% by your crew. |
| weight | 200 |
| price | 600,000 cr. |
| removable | false |
| tags | ventilation, armor, firepower, camouflage, reconnaissance, mobility |
| incompatibleTags | ventilation |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | improvedVentilation_class1_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/crewLevelIncrease](#miscattrscrewlevelincrease) | add | 5.0 | 6.0 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| crewLevel | mul | 1.05 | 1.06 |

### grousers_tier3

| Name | Description |
|:--- |:--- |
| id | 78 |
| userString_en | Additional Grousers Class 3 |
| userString_ja | 追加グローサー クラス3 |
| shortDescriptionSpecial_ja | 車輌の旋回速度と加速力を向上させる。 |
| longDescriptionSpecial_ja | 車輌の旋回速度を向上させ、地形を問わず速度の維持を可能にする。軟地盤や通常地盤の走行時に極めて有用。スキル「クラッチの名手」や「オフロード走行」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Increases vehicle traverse speed and improves acceleration |
| longDescriptionSpecial_en | Increases vehicle traverse speed and maintains speed when crossing all types of terrains. It is considerably effective when driving on soft and moderately soft terrain. It is most effective when combined with the Clutch Braking and Off-Road Driving skills. |
| weight | 100 |
| price | 50,000 cr. |
| removable | false |
| tags | grousers, mobility |
| incompatibleTags | grousers |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include | LT, MT, HT, TD | 2 | 4 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=Grousers)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [rotationFactor](#rotationfactor) | mul | 0.869 | 0.833 |
| [rollingFrictionFactor](#rollingfrictionfactor) | mul | 0.909 | 0.869 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleAllGroundRotationSpeed | mul | 1.15 | 1.2 |
| vehicleSpeedGain | mul | 1.1 | 1.15 |

### grousers_tier2

| Name | Description |
|:--- |:--- |
| id | 79 |
| userString_en | Additional Grousers Class 2 |
| userString_ja | 追加グローサー クラス2 |
| shortDescriptionSpecial_ja | 車輌の旋回速度と加速力を向上させる。 |
| longDescriptionSpecial_ja | 車輌の旋回速度を向上させ、地形を問わず速度の維持を可能にする。軟地盤や通常地盤の走行時に極めて有用。スキル「クラッチの名手」や「オフロード走行」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Increases vehicle traverse speed and improves acceleration |
| longDescriptionSpecial_en | Increases vehicle traverse speed and maintains speed when crossing all types of terrains. It is considerably effective when driving on soft and moderately soft terrain. It is most effective when combined with the Clutch Braking and Off-Road Driving skills. |
| weight | 100 |
| price | 250,000 cr. |
| removable | false |
| tags | grousers, mobility |
| incompatibleTags | grousers |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include | LT, MT, HT, TD | 5 | 7 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=Grousers)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [rotationFactor](#rotationfactor) | mul | 0.869 | 0.833 |
| [rollingFrictionFactor](#rollingfrictionfactor) | mul | 0.909 | 0.869 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleAllGroundRotationSpeed | mul | 1.15 | 1.2 |
| vehicleSpeedGain | mul | 1.1 | 1.15 |

### grousers_tier1

| Name | Description |
|:--- |:--- |
| id | 80 |
| userString_en | Additional Grousers Class 1 |
| userString_ja | 追加グローサー クラス1 |
| shortDescriptionSpecial_ja | 車輌の旋回速度と加速力を向上させる。 |
| longDescriptionSpecial_ja | 車輌の旋回速度を向上させ、地形を問わず速度の維持を可能にする。軟地盤や通常地盤の走行時に極めて有用。スキル「クラッチの名手」や「オフロード走行」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Increases vehicle traverse speed and improves acceleration |
| longDescriptionSpecial_en | Increases vehicle traverse speed and maintains speed when crossing all types of terrains. It is considerably effective when driving on soft and moderately soft terrain. It is most effective when combined with the Clutch Braking and Off-Road Driving skills. |
| weight | 100 |
| price | 500,000 cr. |
| removable | false |
| tags | grousers, mobility |
| incompatibleTags | grousers |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include | LT, MT, HT, TD | 8 | 10 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=Grousers)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [rotationFactor](#rotationfactor) | mul | 0.869 | 0.833 |
| [rollingFrictionFactor](#rollingfrictionfactor) | mul | 0.909 | 0.869 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleAllGroundRotationSpeed | mul | 1.15 | 1.2 |
| vehicleSpeedGain | mul | 1.1 | 1.15 |

### tankRammer_tier2

| Name | Description |
|:--- |:--- |
| id | 81 |
| userString_en | Gun Rammer Class 2 |
| userString_ja | 装填棒 クラス2 |
| longDescriptionSpecial_ja | 射撃速度を向上させる。射撃速度は最も重要な特性値の1つであるため、優先的に搭載を検討すべき拡張パーツ。弾倉式自動装填システムを備えた車輌には搭載できない。 |
| longDescriptionSpecial_en | Rate of fire is one of the most important characteristics of your vehicle. This equipment is quite beneficial in a battle and should be mounted on your vehicles. However, rammers cannot be mounted on vehicles with a magazine loading system. |
| weight | 200 |
| price | 300,000 cr. |
| removable | false |
| tags | rammer, firepower |
| incompatibleTags | rammer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | tankRammer_class2_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/gunReloadTimeFactor](#miscattrsgunreloadtimefactor) | mul | 0.9 | 0.885 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleGunReloadTime | mul | 0.9 | 0.885 |

### tankRammer_tier1

| Name | Description |
|:--- |:--- |
| id | 82 |
| userString_en | Gun Rammer Class 1 |
| userString_ja | 装填棒 クラス1 |
| longDescriptionSpecial_ja | 射撃速度を向上させる。射撃速度は最も重要な特性値の1つであるため、優先的に搭載を検討すべき拡張パーツ。弾倉式自動装填システムを備えた車輌には搭載できない。 |
| longDescriptionSpecial_en | Rate of fire is one of the most important characteristics of your vehicle. This equipment is quite beneficial in a battle and should be mounted on your vehicles. However, rammers cannot be mounted on vehicles with a magazine loading system. |
| weight | 400 |
| price | 600,000 cr. |
| removable | false |
| tags | rammer, firepower |
| incompatibleTags | rammer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | tankRammer_class1_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/gunReloadTimeFactor](#miscattrsgunreloadtimefactor) | mul | 0.9 | 0.885 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleGunReloadTime | mul | 0.9 | 0.885 |

### enhancedAimDrives_tier3

| Name | Description |
|:--- |:--- |
| id | 83 |
| userString_en | Enhanced Gun Laying Drive Class 3 |
| userString_ja | 改良型射撃装置 クラス3 |
| longDescriptionSpecial_ja | 照準時間を短縮し、射撃精度を向上させる。射撃速度と射撃精度の上昇により、照準の完全収束を待たずとも攻撃を命中させることが可能となる。 |
| longDescriptionSpecial_en | Improved aiming reduces the aiming time. A vehicle that is equipped with Enhanced Gun Laying Drive fires more accurately. In addition, it improves the rate of fire, since reloading is sometimes faster than fully aiming. |
| weight | 100 |
| price | 50,000 cr. |
| removable | false |
| tags | enhancedAimDrives, firepower |
| incompatibleTags | enhancedAimDrives |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 2 | 4 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/gunAimingTimeFactor](#miscattrsgunaimingtimefactor) | mul | 0.909 | 0.897 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleGunAimSpeed | mul | 1.1 | 1.115 |

### enhancedAimDrives_tier2

| Name | Description |
|:--- |:--- |
| id | 84 |
| userString_en | Enhanced Gun Laying Drive Class 2 |
| userString_ja | 改良型射撃装置 クラス2 |
| longDescriptionSpecial_ja | 照準時間を短縮し、射撃精度を向上させる。射撃速度と射撃精度の上昇により、照準の完全収束を待たずとも攻撃を命中させることが可能となる。 |
| longDescriptionSpecial_en | Improved aiming reduces the aiming time. A vehicle that is equipped with Enhanced Gun Laying Drive fires more accurately. In addition, it improves the rate of fire, since reloading is sometimes faster than fully aiming. |
| weight | 100 |
| price | 200,000 cr. |
| removable | false |
| tags | enhancedAimDrives, firepower |
| incompatibleTags | enhancedAimDrives |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 5 | 7 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/gunAimingTimeFactor](#miscattrsgunaimingtimefactor) | mul | 0.909 | 0.897 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleGunAimSpeed | mul | 1.1 | 1.115 |

### enhancedAimDrives_tier1

| Name | Description |
|:--- |:--- |
| id | 85 |
| userString_en | Enhanced Gun Laying Drive Class 1 |
| userString_ja | 改良型射撃装置 クラス1 |
| longDescriptionSpecial_ja | 照準時間を短縮し、射撃精度を向上させる。射撃速度と射撃精度の上昇により、照準の完全収束を待たずとも攻撃を命中させることが可能となる。 |
| longDescriptionSpecial_en | Improved aiming reduces the aiming time. A vehicle that is equipped with Enhanced Gun Laying Drive fires more accurately. In addition, it improves the rate of fire, since reloading is sometimes faster than fully aiming. |
| weight | 100 |
| price | 600,000 cr. |
| removable | false |
| tags | enhancedAimDrives, firepower |
| incompatibleTags | enhancedAimDrives |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 8 | 10 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/gunAimingTimeFactor](#miscattrsgunaimingtimefactor) | mul | 0.909 | 0.897 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleGunAimSpeed | mul | 1.1 | 1.115 |

### aimingStabilizer_tier2

| Name | Description |
|:--- |:--- |
| id | 86 |
| userString_en | Vertical Stabilizer Class 2 |
| userString_ja | 砲垂直安定装置 クラス2 |
| longDescriptionSpecial_ja | 移動および旋回に伴う照準拡散を軽減し、着弾分布を縮小する。着弾分布が小さいほど照準サークルも小さくなり、照準の完全収束に必要な時間が短縮されるほか、移動中や照準の完全収束前の射撃精度も向上する。 |
| longDescriptionSpecial_en | Reducing your gun dispersion is critical: the smaller the initial dispersion, the smaller the aiming circle and the quicker you can fully aim your gun. In addition, an unprepared shot (that is fired without fully aiming or on the move) will have a better chance of hitting the target. |
| weight | 100 |
| price | 200,000 cr. |
| removable | false |
| tags | aimingStabilizer, firepower |
| incompatibleTags | aimingStabilizer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | aimingStabilizer_class2_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/additiveShotDispersionFactor](#miscattrsadditiveshotdispersionfactor) | mul | 0.8 | 0.77 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunShotDispersion | mul | 0.8 |

### aimingStabilizer_tier1

| Name | Description |
|:--- |:--- |
| id | 87 |
| userString_en | Vertical Stabilizer Class 1 |
| userString_ja | 砲垂直安定装置 クラス1 |
| longDescriptionSpecial_ja | 移動および旋回に伴う照準拡散を軽減し、着弾分布を縮小する。着弾分布が小さいほど照準サークルも小さくなり、照準の完全収束に必要な時間が短縮されるほか、移動中や照準の完全収束前の射撃精度も向上する。 |
| longDescriptionSpecial_en | Reducing your gun dispersion is critical: the smaller the initial dispersion, the smaller the aiming circle and the quicker you can fully aim your gun. In addition, an unprepared shot (that is fired without fully aiming or on the move) will have a better chance of hitting the target. |
| weight | 200 |
| price | 600,000 cr. |
| removable | false |
| tags | aimingStabilizer, firepower |
| incompatibleTags | aimingStabilizer |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | aimingStabilizer_class1_user |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/additiveShotDispersionFactor](#miscattrsadditiveshotdispersionfactor) | mul | 0.8 | 0.77 |

#### KPI

| Attribute | Type | Regular |
|:--- |:---:|:---:|
| vehicleGunShotDispersion | mul | 0.8 |

### additionalInvisibilityDevice_tier3

| Name | Description |
|:--- |:--- |
| id | 88 |
| userString_en | Low Noise Exhaust System Class 3 |
| userString_ja | 消音排気システム クラス3 |
| shortDescriptionSpecial_ja | 移動中および静止中の車輌の隠蔽率を向上させる。 |
| longDescriptionSpecial_ja | 移動中および静止中の車輌の隠蔽率を向上させる。能動的偵察だけでなく、危険地帯での受動的偵察を行う際にも有用。ボーナス値は車輌タイプにより異なる。 |
| shortDescriptionSpecial_en | Increases the concealment of a stationary or moving vehicle. |
| longDescriptionSpecial_en | The effectiveness depends on the vehicle type. Increases the concealment of a stationary or moving vehicle. Useful for active spotting, as well as for passive spotting from risky positions. |
| weight | 100 |
| price | 50,000 cr. |
| tags | additInvisibilityDevice |
| incompatibleTags | additInvisibilityDevice |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 2 | 4 |

#### Factors (type=LowNoiseTracks)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [invisibilityBonus](#invisibilitybonus) | override | 0.06 | 0.08 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleCamouflage | mul | 1.06 | 1.08 |
| vehicleCamouflage | mul | 1.05 | 1.06 |
| vehicleCamouflage | mul | 1.03 | 1.04 |

### additionalInvisibilityDevice_tier2

| Name | Description |
|:--- |:--- |
| id | 89 |
| userString_en | Low Noise Exhaust System Class 2 |
| userString_ja | 消音排気システム クラス2 |
| shortDescriptionSpecial_ja | 移動中および静止中の車輌の隠蔽率を向上させる。 |
| longDescriptionSpecial_ja | 移動中および静止中の車輌の隠蔽率を向上させる。能動的偵察だけでなく、危険地帯での受動的偵察を行う際にも有用。ボーナス値は車輌タイプにより異なる。 |
| shortDescriptionSpecial_en | Increases the concealment of a stationary or moving vehicle. |
| longDescriptionSpecial_en | The effectiveness depends on the vehicle type. Increases the concealment of a stationary or moving vehicle. Useful for active spotting, as well as for passive spotting from risky positions. |
| weight | 100 |
| price | 200,000 cr. |
| tags | additInvisibilityDevice |
| incompatibleTags | additInvisibilityDevice |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 5 | 7 |

#### Factors (type=LowNoiseTracks)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [invisibilityBonus](#invisibilitybonus) | override | 0.06 | 0.08 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleCamouflage | mul | 1.06 | 1.08 |
| vehicleCamouflage | mul | 1.05 | 1.06 |
| vehicleCamouflage | mul | 1.03 | 1.04 |

### additionalInvisibilityDevice_tier1

| Name | Description |
|:--- |:--- |
| id | 90 |
| userString_en | Low Noise Exhaust System Class 1 |
| userString_ja | 消音排気システム クラス1 |
| shortDescriptionSpecial_ja | 移動中および静止中の車輌の隠蔽率を向上させる。 |
| longDescriptionSpecial_ja | 移動中および静止中の車輌の隠蔽率を向上させる。能動的偵察だけでなく、危険地帯での受動的偵察を行う際にも有用。ボーナス値は車輌タイプにより異なる。 |
| shortDescriptionSpecial_en | Increases the concealment of a stationary or moving vehicle. |
| longDescriptionSpecial_en | The effectiveness depends on the vehicle type. Increases the concealment of a stationary or moving vehicle. Useful for active spotting, as well as for passive spotting from risky positions. |
| weight | 100 |
| price | 600,000 cr. |
| tags | additInvisibilityDevice |
| incompatibleTags | additInvisibilityDevice |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 8 | 10 |

#### Factors (type=LowNoiseTracks)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [invisibilityBonus](#invisibilitybonus) | override | 0.06 | 0.08 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleCamouflage | mul | 1.06 | 1.08 |
| vehicleCamouflage | mul | 1.05 | 1.06 |
| vehicleCamouflage | mul | 1.03 | 1.04 |

### extraHealthReserve_tier3

| Name | Description |
|:--- |:--- |
| id | 91 |
| userString_en | Improved Hardening Class 3 |
| userString_ja | 改良型装甲材 クラス3 |
| shortDescriptionSpecial_ja | 車輌HPと最大積載量を向上させ、サスペンションを保護する。 |
| longDescriptionSpecial_ja | 車輌HP、最大積載量、修理速度を向上させる。また、衝撃によるサスペンションの損傷時に車体が受けるダメージを軽減するほか、被弾時の履帯切断率を低下させる。さらに、修理時にはサスペンションの耐久性を最大値まで回復させる。 |
| shortDescriptionSpecial_en | Increases vehicle durability and load capacity, protects suspension |
| longDescriptionSpecial_en | Increases vehicle durability, load capacity, and repair speed. Reduces hull damage caused by suspension damage during impact. Fully restores suspension hit points after repairs. Reduces the chance of destruction of a track upon receiving damage, allowing your vehicle to remain on the move as long as possible. |
| price | 40,000 cr. |
| tags | healthReserve |
| incompatibleTags | healthReserve |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | extraHealthReserve_class3_user |

#### Factors (type=ExtraHealthReserve)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [chassisMaxLoadFactor](#chassismaxloadfactor) | mul | 1.1 | 1.1 |
| [miscAttrs/healthFactor](#miscattrshealthfactor) | mul | 1.08 | 1.1 |
| [miscAttrs/chassisHealthFactor](#miscattrschassishealthfactor) | mul | 1.5 | 1.65 |
| [miscAttrs/vehicleByChassisDamageFactor](#miscattrsvehiclebychassisdamagefactor) | mul | 0.5 | 0.35 |
| [miscAttrs/chassisRepairSpeedFactor](#miscattrschassisrepairspeedfactor) | mul | 1.15 | 1.2 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStrength | mul | 1.08 | 1.1 |
| vehicleChassisStrength | mul | 1.5 | 1.65 |
| vehicleChassisRepairSpeed | mul | 1.15 | 1.2 |
| vehicleChassisLoad | mul | 1.1 | 1.1 |
| vehicleChassisFallDamage | mul | 0.5 | 0.35 |

### extraHealthReserve_tier2

| Name | Description |
|:--- |:--- |
| id | 92 |
| userString_en | Improved Hardening Class 2 |
| userString_ja | 改良型装甲材 クラス2 |
| shortDescriptionSpecial_ja | 車輌HPと最大積載量を向上させ、サスペンションを保護する。 |
| longDescriptionSpecial_ja | 車輌HP、最大積載量、修理速度を向上させる。また、衝撃によるサスペンションの損傷時に車体が受けるダメージを軽減するほか、被弾時の履帯切断率を低下させる。さらに、修理時にはサスペンションの耐久性を最大値まで回復させる。 |
| shortDescriptionSpecial_en | Increases vehicle durability and load capacity, protects suspension |
| longDescriptionSpecial_en | Increases vehicle durability, load capacity, and repair speed. Reduces hull damage caused by suspension damage during impact. Fully restores suspension hit points after repairs. Reduces the chance of destruction of a track upon receiving damage, allowing your vehicle to remain on the move as long as possible. |
| price | 200,000 cr. |
| tags | healthReserve |
| incompatibleTags | healthReserve |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | extraHealthReserve_class2_user |

#### Factors (type=ExtraHealthReserve)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [chassisMaxLoadFactor](#chassismaxloadfactor) | mul | 1.1 | 1.1 |
| [miscAttrs/healthFactor](#miscattrshealthfactor) | mul | 1.08 | 1.1 |
| [miscAttrs/chassisHealthFactor](#miscattrschassishealthfactor) | mul | 1.5 | 1.65 |
| [miscAttrs/vehicleByChassisDamageFactor](#miscattrsvehiclebychassisdamagefactor) | mul | 0.5 | 0.35 |
| [miscAttrs/chassisRepairSpeedFactor](#miscattrschassisrepairspeedfactor) | mul | 1.15 | 1.2 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStrength | mul | 1.08 | 1.1 |
| vehicleChassisStrength | mul | 1.5 | 1.65 |
| vehicleChassisRepairSpeed | mul | 1.15 | 1.2 |
| vehicleChassisLoad | mul | 1.1 | 1.1 |
| vehicleChassisFallDamage | mul | 0.5 | 0.35 |

### extraHealthReserve_tier1

| Name | Description |
|:--- |:--- |
| id | 93 |
| userString_en | Improved Hardening Class 1 |
| userString_ja | 改良型装甲材 クラス1 |
| shortDescriptionSpecial_ja | 車輌HPと最大積載量を向上させ、サスペンションを保護する。 |
| longDescriptionSpecial_ja | 車輌HP、最大積載量、修理速度を向上させる。また、衝撃によるサスペンションの損傷時に車体が受けるダメージを軽減するほか、被弾時の履帯切断率を低下させる。さらに、修理時にはサスペンションの耐久性を最大値まで回復させる。 |
| shortDescriptionSpecial_en | Increases vehicle durability and load capacity, protects suspension |
| longDescriptionSpecial_en | Increases vehicle durability, load capacity, and repair speed. Reduces hull damage caused by suspension damage during impact. Fully restores suspension hit points after repairs. Reduces the chance of destruction of a track upon receiving damage, allowing your vehicle to remain on the move as long as possible. |
| price | 600,000 cr. |
| tags | healthReserve |
| incompatibleTags | healthReserve |

#### Vehicle Filter

| Type | Tags |
|:---:|:---:|
| include | extraHealthReserve_class1_user |

#### Factors (type=ExtraHealthReserve)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [chassisMaxLoadFactor](#chassismaxloadfactor) | mul | 1.1 | 1.1 |
| [miscAttrs/healthFactor](#miscattrshealthfactor) | mul | 1.08 | 1.1 |
| [miscAttrs/chassisHealthFactor](#miscattrschassishealthfactor) | mul | 1.5 | 1.65 |
| [miscAttrs/vehicleByChassisDamageFactor](#miscattrsvehiclebychassisdamagefactor) | mul | 0.5 | 0.35 |
| [miscAttrs/chassisRepairSpeedFactor](#miscattrschassisrepairspeedfactor) | mul | 1.15 | 1.2 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleStrength | mul | 1.08 | 1.1 |
| vehicleChassisStrength | mul | 1.5 | 1.65 |
| vehicleChassisRepairSpeed | mul | 1.15 | 1.2 |
| vehicleChassisLoad | mul | 1.1 | 1.1 |
| vehicleChassisFallDamage | mul | 0.5 | 0.35 |

### improvedRadioCommunication

| Name | Description |
|:--- |:--- |
| id | 94 |
| userString_en | Improved Radio Set |
| userString_ja | 改良型無線機 |
| shortDescriptionSpecial_ja | 敵車輌の発見状態持続時間を延長する一方、自車輌の被発見状態持続時間を短縮する。 |
| longDescriptionSpecial_ja | 自車輌の視認範囲外から逃れた敵車輌の発見状態持続時間を延長する。また、敵に発見された際に、自車輌の被発見状態持続時間を短縮する。パーク「指定された目標」や「報復要請」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Increases the time an enemy remains spotted and reduces the time you remain spotted |
| longDescriptionSpecial_en | Increases the time an enemy remains spotted after leaving your vehicle's view range. Reduces the time you remain spotted for enemies. It is most effective when combined with the Designated Target and Call for Vengeance perks. |
| weight | 100 |
| price | 600,000 cr. |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include | LT, MT | 8 | 10 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/increaseEnemySpottingTime](#miscattrsincreaseenemyspottingtime) | add | 1.5 | 2.0 |
| [miscAttrs/decreaseOwnSpottingTime](#miscattrsdecreaseownspottingtime) | add | 1.5 | 2.0 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleEnemySpottingTime | add | 1.5 | 2.0 |
| vehicleOwnSpottingTime | add | -1.5 | -2.0 |

### improvedRotationMechanism_tier2

| Name | Description |
|:--- |:--- |
| id | 95 |
| userString_en | Improved Rotation Mechanism Class 2 |
| userString_ja | 改良型旋回機構 クラス2 |
| shortDescriptionSpecial_ja | 旋回速度を向上させると同時に、移動や旋回に伴う照準拡散を軽減する。 |
| longDescriptionSpecial_ja | 砲塔を搭載した車輌の場合は車体と砲塔の旋回速度を、戦闘室が固定式の車輌の場合は車体と主砲の旋回速度を向上させる。また、移動や、車体または砲塔の旋回に伴う照準拡散を軽減する。スキル「クラッチの名手」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Increases traverse speed and reduces gun dispersion during movement and rotation |
| longDescriptionSpecial_en | Increases hull and turret traverse speed for vehicles with turrets. Increases hull and gun traverse speed for vehicles with stationary cabins. Reduces gun dispersion during movement and during hull and turret traverse. It is more effective when combined with the Clutch Braking skill. |
| weight | 200 |
| price | 200,000 cr. |
| tags | rotationMechanism |
| incompatibleTags | rotationMechanism |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 5 | 7 |

#### Factors (type=RotationMechanisms)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [trackRotateSpeedFactor](#trackrotatespeedfactor) | mul | 1.1 | 1.125 |
| [wheelRotateSpeedFactor](#wheelrotatespeedfactor) | mul | 1.1 | 1.125 |
| [wheelCenterRotationFwdSpeed](#wheelcenterrotationfwdspeed) | mul | 1.1 | 1.125 |
| [trackMoveSpeedFactor](#trackmovespeedfactor) | mul | 1.1 | 1.125 |
| [wheelMoveSpeedFactor](#wheelmovespeedfactor) | mul | 1.1 | 1.125 |
| [miscAttrs/turretRotationSpeed](#miscattrsturretrotationspeed) | mul | 1.1 | 1.125 |
| [miscAttrs/additiveShotDispersionFactor](#miscattrsadditiveshotdispersionfactor) | mul | 0.9 | 0.875 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleTurretOrCuttingRotationSpeed | mul | 1.1 | 1.125 |
| vehicleAllGroundRotationSpeed | mul | 1.1 | 1.125 |
| vehicleGunShotDispersion | mul | 0.9 | 0.875 |

### improvedRotationMechanism_tier1

| Name | Description |
|:--- |:--- |
| id | 96 |
| userString_en | Improved Rotation Mechanism Class 1 |
| userString_ja | 改良型旋回機構 クラス1 |
| shortDescriptionSpecial_ja | 旋回速度を向上させると同時に、移動や旋回に伴う照準拡散を軽減する。 |
| longDescriptionSpecial_ja | 砲塔を搭載した車輌の場合は車体と砲塔の旋回速度を、戦闘室が固定式の車輌の場合は車体と主砲の旋回速度を向上させる。また、移動や、車体または砲塔の旋回に伴う照準拡散を軽減する。スキル「クラッチの名手」と併用するとより効果的。 |
| shortDescriptionSpecial_en | Increases traverse speed and reduces gun dispersion during movement and rotation |
| longDescriptionSpecial_en | Increases hull and turret traverse speed for vehicles with turrets. Increases hull and gun traverse speed for vehicles with stationary cabins. Reduces gun dispersion during movement and during hull and turret traverse. It is more effective when combined with the Clutch Braking skill. |
| weight | 200 |
| price | 600,000 cr. |
| tags | rotationMechanism |
| incompatibleTags | rotationMechanism |

#### Vehicle Filter

| Type | MinLevel | MaxLevel |
|:---:|:---:|:---:|
| include | 8 | 10 |

#### Factors (type=RotationMechanisms)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [trackRotateSpeedFactor](#trackrotatespeedfactor) | mul | 1.1 | 1.125 |
| [wheelRotateSpeedFactor](#wheelrotatespeedfactor) | mul | 1.1 | 1.125 |
| [wheelCenterRotationFwdSpeed](#wheelcenterrotationfwdspeed) | mul | 1.1 | 1.125 |
| [trackMoveSpeedFactor](#trackmovespeedfactor) | mul | 1.1 | 1.125 |
| [wheelMoveSpeedFactor](#wheelmovespeedfactor) | mul | 1.1 | 1.125 |
| [miscAttrs/turretRotationSpeed](#miscattrsturretrotationspeed) | mul | 1.1 | 1.125 |
| [miscAttrs/additiveShotDispersionFactor](#miscattrsadditiveshotdispersionfactor) | mul | 0.9 | 0.875 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleTurretOrCuttingRotationSpeed | mul | 1.1 | 1.125 |
| vehicleAllGroundRotationSpeed | mul | 1.1 | 1.125 |
| vehicleGunShotDispersion | mul | 0.9 | 0.875 |

### turbocharger_tier3

| Name | Description |
|:--- |:--- |
| id | 97 |
| userString_en | Turbocharger Class 3 |
| userString_ja | ターボチャージャー クラス3 |
| shortDescriptionSpecial_ja | エンジン出力の上昇により最大速度を向上させる。 |
| longDescriptionSpecial_ja | エンジン出力を上昇させる。最大速度、後退速度、加速力、俊敏性、登坂性能がすべて向上するため、要所の素早い確保や、敵の反撃の回避が可能となる。 |
| shortDescriptionSpecial_en | Increases engine power and top speed |
| longDescriptionSpecial_en | Increases engine power, providing better acceleration, maneuvering, and ascending of terrain slopes. Increases the vehicle's maximum forward and reverse speed, allowing you to take positions and escape enemy return fire quicker. |
| weight | 100 |
| price | 50,000 cr. |
| tags | turbocharger |
| incompatibleTags | turbocharger |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include |  | 2 | 4 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/enginePowerFactor](#miscattrsenginepowerfactor) | mul | 1.075 | 1.1 |
| [miscAttrs/forwardMaxSpeedKMHTerm](#miscattrsforwardmaxspeedkmhterm) | add | 4 | 5 |
| [miscAttrs/backwardMaxSpeedKMHTerm](#miscattrsbackwardmaxspeedkmhterm) | add | 2 | 3 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleEnginePower | mul | 1.075 | 1.1 |
| vehicleForwardMaxSpeed | add | 4 | 5 |
| vehicleBackwardMaxSpeed | add | 2 | 3 |

### turbocharger_tier2

| Name | Description |
|:--- |:--- |
| id | 98 |
| userString_en | Turbocharger Class 2 |
| userString_ja | ターボチャージャー クラス2 |
| shortDescriptionSpecial_ja | エンジン出力の上昇により最大速度を向上させる。 |
| longDescriptionSpecial_ja | エンジン出力を上昇させる。最大速度、後退速度、加速力、俊敏性、登坂性能がすべて向上するため、要所の素早い確保や、敵の反撃の回避が可能となる。 |
| shortDescriptionSpecial_en | Increases engine power and top speed |
| longDescriptionSpecial_en | Increases engine power, providing better acceleration, maneuvering, and ascending of terrain slopes. Increases the vehicle's maximum forward and reverse speed, allowing you to take positions and escape enemy return fire quicker. |
| weight | 100 |
| price | 200,000 cr. |
| tags | turbocharger |
| incompatibleTags | turbocharger |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include |  | 5 | 7 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/enginePowerFactor](#miscattrsenginepowerfactor) | mul | 1.075 | 1.1 |
| [miscAttrs/forwardMaxSpeedKMHTerm](#miscattrsforwardmaxspeedkmhterm) | add | 4 | 5 |
| [miscAttrs/backwardMaxSpeedKMHTerm](#miscattrsbackwardmaxspeedkmhterm) | add | 2 | 3 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleEnginePower | mul | 1.075 | 1.1 |
| vehicleForwardMaxSpeed | add | 4 | 5 |
| vehicleBackwardMaxSpeed | add | 2 | 3 |

### turbocharger_tier1

| Name | Description |
|:--- |:--- |
| id | 99 |
| userString_en | Turbocharger Class 1 |
| userString_ja | ターボチャージャー クラス1 |
| shortDescriptionSpecial_ja | エンジン出力の上昇により最大速度を向上させる。 |
| longDescriptionSpecial_ja | エンジン出力を上昇させる。最大速度、後退速度、加速力、俊敏性、登坂性能がすべて向上するため、要所の素早い確保や、敵の反撃の回避が可能となる。 |
| shortDescriptionSpecial_en | Increases engine power and top speed |
| longDescriptionSpecial_en | Increases engine power, providing better acceleration, maneuvering, and ascending of terrain slopes. Increases the vehicle's maximum forward and reverse speed, allowing you to take positions and escape enemy return fire quicker. |
| weight | 100 |
| price | 600,000 cr. |
| tags | turbocharger |
| incompatibleTags | turbocharger |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include |  | 8 | 10 |
| exclude | wheeledVehicle |  |  |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/enginePowerFactor](#miscattrsenginepowerfactor) | mul | 1.075 | 1.1 |
| [miscAttrs/forwardMaxSpeedKMHTerm](#miscattrsforwardmaxspeedkmhterm) | add | 4 | 5 |
| [miscAttrs/backwardMaxSpeedKMHTerm](#miscattrsbackwardmaxspeedkmhterm) | add | 2 | 3 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleEnginePower | mul | 1.075 | 1.1 |
| vehicleForwardMaxSpeed | add | 4 | 5 |
| vehicleBackwardMaxSpeed | add | 2 | 3 |

### commandersView

| Name | Description |
|:--- |:--- |
| id | 100 |
| userString_en | Commander's Vision System |
| userString_ja | 車長用視認性向上装置 |
| shortDescriptionSpecial_ja | 移動中または茂みや樹木の陰に隠れた敵車輌の隠蔽率を低下させる。 |
| longDescriptionSpecial_ja | 移動中または茂みや樹木の陰に隠れた敵車輌の隠蔽率を低下させる。移動中の敵を通常より遠くから視認したり、潜伏中の敵をより効果的に発見できるようになる。 |
| shortDescriptionSpecial_en | Reduces the concealment of enemy vehicles, either moving or behind foliage |
| longDescriptionSpecial_en | Reduces the concealment of moving enemies and the foliage concealment coefficients of enemies during spotting. This allows you to spot moving enemies from longer distances and more effectively spot enemies that are concealed by bushes or fallen trees. |
| weight | 100 |
| price | 600,000 cr. |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include | LT, MT | 8 | 10 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/demaskFoliageFactor](#miscattrsdemaskfoliagefactor) | mul | 0.85 | 0.8 |
| [miscAttrs/demaskMovingFactor](#miscattrsdemaskmovingfactor) | mul | 0.9 | 0.875 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| demaskFoliageFactor | mul | 0.85 | 0.80 |
| demaskMovingFactor | mul | 0.9 | 0.875 |

### improvedSights_tier2

| Name | Description |
|:--- |:--- |
| id | 101 |
| userString_en | Improved Aiming Class 2 |
| userString_ja | 改良型照準器 クラス2 |
| shortDescriptionSpecial_ja | 照準サークルを縮小する。 |
| longDescriptionSpecial_ja | 照準サークルのサイズを縮小することで、収束中、または完全に収束済みの主砲の射撃精度を向上させる。 |
| shortDescriptionSpecial_en | Reduces gun dispersion |
| longDescriptionSpecial_en | Reduces the size of the aiming circle to increase the accuracy of an aiming or fully aimed gun. As a result, your gun has a better chance of hitting the target. |
| weight | 100 |
| price | 200,000 cr. |
| tags | improvedSights |
| incompatibleTags | improvedSights |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include | LT, MT, HT, TD | 5 | 7 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/multShotDispersionFactor](#miscattrsmultshotdispersionfactor) | mul | 0.95 | 0.93 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleGunShotFullDispersion | mul | 0.95 | 0.93 |

### improvedSights_tier1

| Name | Description |
|:--- |:--- |
| id | 102 |
| userString_en | Improved Aiming Class 1 |
| userString_ja | 改良型照準器 クラス1 |
| shortDescriptionSpecial_ja | 照準サークルを縮小する。 |
| longDescriptionSpecial_ja | 照準サークルのサイズを縮小することで、収束中、または完全に収束済みの主砲の射撃精度を向上させる。 |
| shortDescriptionSpecial_en | Reduces gun dispersion |
| longDescriptionSpecial_en | Reduces the size of the aiming circle to increase the accuracy of an aiming or fully aimed gun. As a result, your gun has a better chance of hitting the target. |
| weight | 100 |
| price | 600,000 cr. |
| tags | improvedSights |
| incompatibleTags | improvedSights |

#### Vehicle Filter

| Type | Tags | MinLevel | MaxLevel |
|:---:|:---:|:---:|:---:|
| include | LT, MT, HT, TD | 8 | 10 |

#### Factors (type=StaticOptionalDevice)

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| [miscAttrs/multShotDispersionFactor](#miscattrsmultshotdispersionfactor) | mul | 0.95 | 0.93 |

#### KPI

| Attribute | Type | Regular | Imploved |
|:--- |:---:|:---:|:---:|
| vehicleGunShotFullDispersion | mul | 0.95 | 0.93 |


## Attribute Factors

### engineReduceFineFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 43 | [deluxImprovedConfiguration](#deluximprovedconfiguration) | 0.35 | 0.35 |
| 105 | [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | 0.5 | 0.5 |
| 106 | [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | 0.3 | 0.3 |
| 61 | [improvedConfiguration_tier2](#improvedconfiguration_tier2) | 0.5 | 0.35 |
| 62 | [improvedConfiguration_tier1](#improvedconfiguration_tier1) | 0.5 | 0.35 |

### ammoBayReduceFineFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 43 | [deluxImprovedConfiguration](#deluximprovedconfiguration) | 0.35 | 0.35 |
| 105 | [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | 0.5 | 0.5 |
| 106 | [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | 0.3 | 0.3 |
| 61 | [improvedConfiguration_tier2](#improvedconfiguration_tier2) | 0.5 | 0.35 |
| 62 | [improvedConfiguration_tier1](#improvedconfiguration_tier1) | 0.5 | 0.35 |

### miscAttrs/repairSpeedFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 43 | [deluxImprovedConfiguration](#deluximprovedconfiguration) | mul | 1.45 | 1.45 |
| 105 | [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | mul | 1.25 | 1.25 |
| 106 | [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | mul | 1.4 | 1.4 |
| 61 | [improvedConfiguration_tier2](#improvedconfiguration_tier2) | mul | 1.25 | 1.35 |
| 62 | [improvedConfiguration_tier1](#improvedconfiguration_tier1) | mul | 1.25 | 1.35 |

### miscAttrs/ammoBayHealthFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 43 | [deluxImprovedConfiguration](#deluximprovedconfiguration) | mul | 2.5 | 2.5 |
| 105 | [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | mul | 2.0 | 2.0 |
| 106 | [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | mul | 2.5 | 2.5 |
| 61 | [improvedConfiguration_tier2](#improvedconfiguration_tier2) | mul | 2.0 | 2.5 |
| 62 | [improvedConfiguration_tier1](#improvedconfiguration_tier1) | mul | 2.0 | 2.5 |

### miscAttrs/fuelTankHealthFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 43 | [deluxImprovedConfiguration](#deluximprovedconfiguration) | mul | 2.5 | 2.5 |
| 105 | [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | mul | 2.0 | 2.0 |
| 106 | [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | mul | 2.5 | 2.5 |
| 61 | [improvedConfiguration_tier2](#improvedconfiguration_tier2) | mul | 2.0 | 2.5 |
| 62 | [improvedConfiguration_tier1](#improvedconfiguration_tier1) | mul | 2.0 | 2.5 |

### miscAttrs/engineHealthFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 43 | [deluxImprovedConfiguration](#deluximprovedconfiguration) | mul | 2.5 | 2.5 |
| 105 | [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | mul | 2.0 | 2.0 |
| 106 | [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | mul | 2.5 | 2.5 |
| 61 | [improvedConfiguration_tier2](#improvedconfiguration_tier2) | mul | 2.0 | 2.5 |
| 62 | [improvedConfiguration_tier1](#improvedconfiguration_tier1) | mul | 2.0 | 2.5 |

### miscAttrs/fireStartingChanceFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 43 | [deluxImprovedConfiguration](#deluximprovedconfiguration) | mul | 0.35 | 0.35 |
| 105 | [trophyBasicImprovedConfiguration](#trophybasicimprovedconfiguration) | mul | 0.5 | 0.5 |
| 106 | [trophyUpgradedImprovedConfiguration](#trophyupgradedimprovedconfiguration) | mul | 0.3 | 0.3 |
| 61 | [improvedConfiguration_tier2](#improvedconfiguration_tier2) | mul | 0.5 | 0.35 |
| 62 | [improvedConfiguration_tier1](#improvedconfiguration_tier1) | mul | 0.5 | 0.35 |

### miscAttrs/crewLevelIncrease

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 44 | [deluxImprovedVentilation](#deluximprovedventilation) | add | 8.5 | 8.5 |
| 54 | [trophyBasicImprovedVentilation](#trophybasicimprovedventilation) | add | 5.0 |  |
| 55 | [trophyUpgradedImprovedVentilation](#trophyupgradedimprovedventilation) | add | 7.5 |  |
| 75 | [improvedVentilation_tier3](#improvedventilation_tier3) | add | 5.0 | 6.0 |
| 76 | [improvedVentilation_tier2](#improvedventilation_tier2) | add | 5.0 | 6.0 |
| 77 | [improvedVentilation_tier1](#improvedventilation_tier1) | add | 5.0 | 6.0 |

### miscAttrs/gunReloadTimeFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 45 | [deluxRammer](#deluxrammer) | mul | 0.865 | 0.865 |
| 52 | [trophyBasicTankRammer](#trophybasictankrammer) | mul | 0.9 |  |
| 53 | [trophyUpgradedTankRammer](#trophyupgradedtankrammer) | mul | 0.875 |  |
| 81 | [tankRammer_tier2](#tankrammer_tier2) | mul | 0.9 | 0.885 |
| 82 | [tankRammer_tier1](#tankrammer_tier1) | mul | 0.9 | 0.885 |

### miscAttrs/circularVisionRadiusFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 46 | [deluxCoatedOptics](#deluxcoatedoptics) | mul | 1.135 | 1.135 |
| 58 | [trophyBasicCoatedOptics](#trophybasiccoatedoptics) | mul | 1.1 |  |
| 59 | [trophyUpgradedCoatedOptics](#trophyupgradedcoatedoptics) | mul | 1.125 |  |
| 70 | [coatedOptics_tier3](#coatedoptics_tier3) | mul | 1.1 | 1.115 |
| 71 | [coatedOptics_tier2](#coatedoptics_tier2) | mul | 1.1 | 1.115 |
| 72 | [coatedOptics_tier1](#coatedoptics_tier1) | mul | 1.1 | 1.115 |

### miscAttrs/additiveShotDispersionFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 47 | [deluxAimingStabilizer](#deluxaimingstabilizer) | mul | 0.725 | 0.725 |
| 56 | [trophyBasicAimingStabilizer](#trophybasicaimingstabilizer) | mul | 0.8 |  |
| 57 | [trophyUpgradedAimingStabilizer](#trophyupgradedaimingstabilizer) | mul | 0.75 |  |
| 86 | [aimingStabilizer_tier2](#aimingstabilizer_tier2) | mul | 0.8 | 0.77 |
| 87 | [aimingStabilizer_tier1](#aimingstabilizer_tier1) | mul | 0.8 | 0.77 |
| 95 | [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | mul | 0.9 | 0.875 |
| 96 | [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | mul | 0.9 | 0.875 |

### miscAttrs/gunAimingTimeFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 48 | [deluxEnhancedAimDrives](#deluxenhancedaimdrives) | mul | 0.881 | 0.881 |
| 50 | [trophyBasicAimDrives](#trophybasicaimdrives) | mul | 0.91 |  |
| 51 | [trophyUpgradedAimDrives](#trophyupgradedaimdrives) | mul | 0.89 |  |
| 83 | [enhancedAimDrives_tier3](#enhancedaimdrives_tier3) | mul | 0.909 | 0.897 |
| 84 | [enhancedAimDrives_tier2](#enhancedaimdrives_tier2) | mul | 0.909 | 0.897 |
| 85 | [enhancedAimDrives_tier1](#enhancedaimdrives_tier1) | mul | 0.909 | 0.897 |

### miscAttrs/antifragmentationLiningFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 63 | [antifragmentationLining_tier4](#antifragmentationlining_tier4) | mul | 1.5 | 1.6 |
| 64 | [antifragmentationLining_tier3](#antifragmentationlining_tier3) | mul | 1.5 | 1.6 |
| 65 | [antifragmentationLining_tier2](#antifragmentationlining_tier2) | mul | 1.5 | 1.6 |
| 66 | [antifragmentationLining_tier1](#antifragmentationlining_tier1) | mul | 1.5 | 1.6 |

### miscAttrs/crewChanceToHitFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 63 | [antifragmentationLining_tier4](#antifragmentationlining_tier4) | mul | 0.5 | 0.4 |
| 64 | [antifragmentationLining_tier3](#antifragmentationlining_tier3) | mul | 0.5 | 0.4 |
| 65 | [antifragmentationLining_tier2](#antifragmentationlining_tier2) | mul | 0.5 | 0.4 |
| 66 | [antifragmentationLining_tier1](#antifragmentationlining_tier1) | mul | 0.5 | 0.4 |

### miscAttrs/stunResistanceDuration

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 63 | [antifragmentationLining_tier4](#antifragmentationlining_tier4) | add | 0.1 | 0.15 |
| 64 | [antifragmentationLining_tier3](#antifragmentationlining_tier3) | add | 0.1 | 0.15 |
| 65 | [antifragmentationLining_tier2](#antifragmentationlining_tier2) | add | 0.1 | 0.15 |
| 66 | [antifragmentationLining_tier1](#antifragmentationlining_tier1) | add | 0.1 | 0.15 |

### miscAttrs/repeatedStunDurationFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 63 | [antifragmentationLining_tier4](#antifragmentationlining_tier4) | mul | 0.8 | 0.75 |
| 64 | [antifragmentationLining_tier3](#antifragmentationlining_tier3) | mul | 0.8 | 0.75 |
| 65 | [antifragmentationLining_tier2](#antifragmentationlining_tier2) | mul | 0.8 | 0.75 |
| 66 | [antifragmentationLining_tier1](#antifragmentationlining_tier1) | mul | 0.8 | 0.75 |

### circularVisionRadius

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 67 | [stereoscope_tier3](#stereoscope_tier3) | 1.25 | 1.275 |
| 68 | [stereoscope_tier2](#stereoscope_tier2) | 1.25 | 1.275 |
| 69 | [stereoscope_tier1](#stereoscope_tier1) | 1.25 | 1.275 |

### rotationFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 78 | [grousers_tier3](#grousers_tier3) | 0.869 | 0.833 |
| 79 | [grousers_tier2](#grousers_tier2) | 0.869 | 0.833 |
| 80 | [grousers_tier1](#grousers_tier1) | 0.869 | 0.833 |

### rollingFrictionFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 78 | [grousers_tier3](#grousers_tier3) | 0.909 | 0.869 |
| 79 | [grousers_tier2](#grousers_tier2) | 0.909 | 0.869 |
| 80 | [grousers_tier1](#grousers_tier1) | 0.909 | 0.869 |

### chassisMaxLoadFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 91 | [extraHealthReserve_tier3](#extrahealthreserve_tier3) | 1.1 | 1.1 |
| 92 | [extraHealthReserve_tier2](#extrahealthreserve_tier2) | 1.1 | 1.1 |
| 93 | [extraHealthReserve_tier1](#extrahealthreserve_tier1) | 1.1 | 1.1 |

### miscAttrs/healthFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 91 | [extraHealthReserve_tier3](#extrahealthreserve_tier3) | mul | 1.08 | 1.1 |
| 92 | [extraHealthReserve_tier2](#extrahealthreserve_tier2) | mul | 1.08 | 1.1 |
| 93 | [extraHealthReserve_tier1](#extrahealthreserve_tier1) | mul | 1.08 | 1.1 |

### miscAttrs/chassisHealthFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 91 | [extraHealthReserve_tier3](#extrahealthreserve_tier3) | mul | 1.5 | 1.65 |
| 92 | [extraHealthReserve_tier2](#extrahealthreserve_tier2) | mul | 1.5 | 1.65 |
| 93 | [extraHealthReserve_tier1](#extrahealthreserve_tier1) | mul | 1.5 | 1.65 |

### miscAttrs/vehicleByChassisDamageFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 91 | [extraHealthReserve_tier3](#extrahealthreserve_tier3) | mul | 0.5 | 0.35 |
| 92 | [extraHealthReserve_tier2](#extrahealthreserve_tier2) | mul | 0.5 | 0.35 |
| 93 | [extraHealthReserve_tier1](#extrahealthreserve_tier1) | mul | 0.5 | 0.35 |

### miscAttrs/chassisRepairSpeedFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 91 | [extraHealthReserve_tier3](#extrahealthreserve_tier3) | mul | 1.15 | 1.2 |
| 92 | [extraHealthReserve_tier2](#extrahealthreserve_tier2) | mul | 1.15 | 1.2 |
| 93 | [extraHealthReserve_tier1](#extrahealthreserve_tier1) | mul | 1.15 | 1.2 |

### miscAttrs/increaseEnemySpottingTime

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 94 | [improvedRadioCommunication](#improvedradiocommunication) | add | 1.5 | 2.0 |

### miscAttrs/decreaseOwnSpottingTime

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 94 | [improvedRadioCommunication](#improvedradiocommunication) | add | 1.5 | 2.0 |

### trackRotateSpeedFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 95 | [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | 1.1 | 1.125 |
| 96 | [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | 1.1 | 1.125 |

### wheelRotateSpeedFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 95 | [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | 1.1 | 1.125 |
| 96 | [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | 1.1 | 1.125 |

### wheelCenterRotationFwdSpeed

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 95 | [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | 1.1 | 1.125 |
| 96 | [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | 1.1 | 1.125 |

### trackMoveSpeedFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 95 | [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | 1.1 | 1.125 |
| 96 | [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | 1.1 | 1.125 |

### wheelMoveSpeedFactor

| ID | DeviceName | Regular | Imploved |
|:---:|:--- |:---:|:---:|
| 95 | [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | 1.1 | 1.125 |
| 96 | [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | 1.1 | 1.125 |

### miscAttrs/turretRotationSpeed

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 95 | [improvedRotationMechanism_tier2](#improvedrotationmechanism_tier2) | mul | 1.1 | 1.125 |
| 96 | [improvedRotationMechanism_tier1](#improvedrotationmechanism_tier1) | mul | 1.1 | 1.125 |

### miscAttrs/enginePowerFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 97 | [turbocharger_tier3](#turbocharger_tier3) | mul | 1.075 | 1.1 |
| 98 | [turbocharger_tier2](#turbocharger_tier2) | mul | 1.075 | 1.1 |
| 99 | [turbocharger_tier1](#turbocharger_tier1) | mul | 1.075 | 1.1 |

### miscAttrs/forwardMaxSpeedKMHTerm

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 97 | [turbocharger_tier3](#turbocharger_tier3) | add | 4 | 5 |
| 98 | [turbocharger_tier2](#turbocharger_tier2) | add | 4 | 5 |
| 99 | [turbocharger_tier1](#turbocharger_tier1) | add | 4 | 5 |

### miscAttrs/backwardMaxSpeedKMHTerm

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 97 | [turbocharger_tier3](#turbocharger_tier3) | add | 2 | 3 |
| 98 | [turbocharger_tier2](#turbocharger_tier2) | add | 2 | 3 |
| 99 | [turbocharger_tier1](#turbocharger_tier1) | add | 2 | 3 |

### miscAttrs/demaskFoliageFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 100 | [commandersView](#commandersview) | mul | 0.85 | 0.8 |

### miscAttrs/demaskMovingFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 100 | [commandersView](#commandersview) | mul | 0.9 | 0.875 |

### miscAttrs/multShotDispersionFactor

| ID | DeviceName | Type | Regular | Imploved |
|:---:|:--- |:---:|:---:|:---:|
| 101 | [improvedSights_tier2](#improvedsights_tier2) | mul | 0.95 | 0.93 |
| 102 | [improvedSights_tier1](#improvedsights_tier1) | mul | 0.95 | 0.93 |

