---
layout: page
title: WoT での ActionScript 利用
---
**作成中**

Flash 操作のためのクラス `gui.Scaleform.Flash.Flash`

```python
class Flash(object):

    def __init__(self, swf, className = 'Flash', args = None, path = SCALEFORM_SWF_PATH):
        if args is None:
            args = []
        super(Flash, self).__init__()
        self.__fsCbs = defaultdict(set)
        self.__exCbs = defaultdict(set)
        movieDefinition = _Scaleform.MovieDef(''.join((path, '/', swf)))
        movie = movieDefinition.createInstance()
        self.component = getattr(GUI, className)(movie, *args)
```

引数 `swf` に指定された SWF ファイルを読み込む。

以下は照準パネルでの使用例、と思ったけど `crosshairPanel.swf` なんて `gui.pkg` にないな。
（`crosshairPanelContainer.swf` というのはある）

```python
class CrosshairPanel(Flash.Flash, CrosshairPanelMeta):
    """Class is UI component of crosshair panel. It provides access to Action Script."""

    def __init__(self):
        super(CrosshairPanel, self).__init__('crosshairPanel.swf', className=_CROSSHAIR_PANEL_COMPONENT, path=SCALEFORM_SWF_PATH_V3)
        self.__plugins = PluginsCollection(self)
```

`crosshairPanelContainer.swf` というのがあって
`crosshairControls.swf`,
`postmortemCrosshair.swf`,
`sniperCrosshair.swf`,
`strategicCrosshair.swf`,
`arcadeCrosshair.swf`
をインポートしているのだけど根っこの `crosshairPanelContainer.swf` がどこで読まれるのかわからない。
