---
layout: page
title: 優等
sitemap: false
mathjax: true
---
**(作成中)**

## 算定式

$$\mathrm{EMA}_i = \mathrm{EMA}_{i-1} + K\; (\mathrm{DAMAGE}_i\; – \;\mathrm{EMA}_{i-1})$$

$$\mathrm{DAMAGE}_i = (\text{dealt by player in battle}) + (\text{maximum damage by tracking or by spotting}) - (\text{team-damage caused to allies in battle})$$

$$K = 2 / (N +1)$$

$$N = 100$$


```python
damageRating = g_currentVehicle.getDossier().getRecordValue(ACHIEVEMENT_BLOCK.TOTAL, 'damageRating') / 100.0
movingAvgDamage = g_currentVehicle.getDossier().getRecordValue(ACHIEVEMENT_BLOCK.TOTAL, 'movingAvgDamage')
```

+ `damageRating`: 優等% (内部ではさらに100倍した値)
+ `movingAvgDamage`: ダメージの指数平滑移動平均　(EMA)

```python
from CurrentVehicle import g_currentVehicle
from dossiers2.ui.achievements import ACHIEVEMENT_BLOCK

dossier = g_currentVehicle.getDossier()	# <gui.shared.gui_items.dossier.VehicleDossier>
randomStats = dossier.getRandomStats()	# <gui.shared.gui_items.dossier.stats.RandomStatsBlock>

damageRating = dossier.getRecordValue(ACHIEVEMENT_BLOCK.TOTAL, 'damageRating') / 100.0
movingAvgDamage = dossier.getRecordValue(ACHIEVEMENT_BLOCK.TOTAL, 'movingAvgDamage')
avgDamage = randomStats.getAvgDamage()
track = randomStats._getAvgValue(randomStats.getBattlesCountVer2, randomStats.getDamageAssistedTrack)
radio = randomStats._getAvgValue(randomStats.getBattlesCountVer2, randomStats.getDamageAssistedRadio)
stun = randomStats.getAvgDamageAssistedStun()
```


## 参考

+ [WotLabs Forum: Marks of Excellence](http://forum.wotlabs.net/index.php?/topic/10422-marks-of-excellence/&do=findComment&comment=269826)
+ [WoT EU Forum: How MoE is calculated](http://forum.worldoftanks.eu/index.php?/topic/608742-how-moe-is-calculated/)
+ [WoTで強くなりたい人のブログ: 優等マークについて考える(優等マーク計算方法等)](http://taku-wot.hatenablog.com/entry/2017/03/31/021426)
+ [GitHub: spoter/spoter-mods/mod_marksOnGunExtended](https://github.com/spoter/spoter-mods/tree/master/mod_marksOnGunExtended)
