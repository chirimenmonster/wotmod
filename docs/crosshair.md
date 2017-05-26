---
layout: page
title: WoT の照準データ
---
** 作成中 **

WoT の照準は Flash (ActionScript3) で作成されている。

ファイルは `res/packages/gui.pkg` 内の
`gui/flash` にある `crosshairPanelContainer.swf`,
`crosshairControls.swf`,
`arcadeCrosshair.swf`, `sniperCrosshair.swf`, `strategicCrosshair.swf`, `postmortemCrosshair.swf`

## crosshairControls.swf

sprites の DefineSprite 66 が crosshairControls_fla.A_3 で、

|:----:|----|
| frame 1 | 55 | normal |
| frame 2 | 57 | red    |
| frame 3 | 59 | orange |
| frame 4 | 61 | green  |
| frame 5 | 63 | purple |
| frame 6 | 65 | yellow |

shapes

| ID | Needed | Dependent |
|----|----|----|
| 55 | 54 | 66, 67, 68, 122, 132, 123 |
| 57 | 56 | 66, 122, 132, 123 |
| 59 | 58 | 66, 122, 132, 123 |
| 61 | 60 | 66, 67, 122, 132, 123 |
| 63 | 62 | 66, 122, 132, 123 |
| 65 | 64 | 66, 122, 132, 123 |


