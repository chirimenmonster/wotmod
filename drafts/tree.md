---
layout: page
title: クラス・インスタンス
sitemap: false
---

class: PlayerAvatar
  attr: inputHandler = AvatarInputHandler.AvatarInputHandler()

class: AvatarInputHandler(
  attr: ctrls['arcade'] ?

class: AvatarInputHandler(
  attr: __ctrls['arcade']

var: _CTRLS_DESC_MAP = {_CTRL_MODE.ARCADE: ('ArcadeControlMode', 'arcadeMode', _CTRL_TYPE.USUAL),... (client/AvatarInputHandler/__init__.py)

class: ArcadeControlMode <- _GunControlMode　(client/AvatarInputHandler/control_modes.py)

class: _GunControlMode (client/AvatarInputHandler/control_modes.py)
  attr: _gunMarker = gun_marker_ctrl.createGunMarker

gun_marker_ctrl (client/AvatarInputHandler/gun_marker_ctrl.py)


> BigWorld.player().inputHandler.ctrls['arcade']
<AvatarInputHandler.control_modes.ArcadeControlMode object at 0x4B099D70>



scripts/client/gui/Scaleform/daapi/view/battle/shared/crosshair/container.py
class CrosshairPanelContainer:
  __init__:
    self.__plugins.addPlugins(plugins.createPlugins())

scripts/client/gui/Scaleform/daapi/view/battle/shared/crosshair/plugins.py
def createPlugins():