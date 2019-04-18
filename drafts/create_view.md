---
layout: page
title: View の作成
sitemap: false
---
WoT での GUI の基本要素である View の作成についてのメモです。


### View の作成

View 設定の登録

```python
from gui.Scaleform.framework import ViewSettings, ViewTypes, ScopeTemplates, g_entitiesFactories

MY_ALIAS = 'myView'
MY_SWF_PATH = 'myView.swf'

settings = ViewSettings(MY_ALIAS, MyView, MY_SWF_PATH, Viewtypes.WINDOW, None,
                        ScopeTemplates.DEFAULT_SCOPE)
g_entitiesFactories.addSettings(settings)
```

View の読み込みは下記のようになります。
ここでは getLobbyApp でロビーアプリケーションを取得し、
ロビーアプリケーションに MY_ALIAS に対応する View を表示しています。

```python
def showWindow():
    app = g_appLoader.getLobbyApp()
    app.loadView(SFViewLoadParams(MY_ALIAS))
```

```python
from gui.Scaleform.framework.entities.View import View

class MyView(View):
    pass
```

### swf ファイル

下のコードは枠付きのウィンドウを表示する AS3 のコードです。

```actionscript
package
{
    import net.wg.infrastructure.base.AbstractWindowView;
    
    public class Main extends AbstractWindowView
    {
        public function Main() : void
        {
            width = 380;
            height = 240;
            window.title = "Test View";
        }
    }
}
```

WoT では枠付きのウインドウは AbstractWindowView というクラスで表現されています。
枠なし背景なしのウィンドウは AbstractView です。

ここではウィンドウの幅と高さ、ウィンドウのタイトルを設定してます。

AbstractWindowView を import する必要があるので、
あらかじめ WoT の swc ライブラリを gui.pkg から抽出してライブラリに登録しておきます。


```python
from gui.shared import g_eventBus, events
from gui.app_loader import g_appLoader
from gui.app_loader.settings import APP_NAME_SPACE
from gui.Scaleform.framework import ViewSettings, ViewTypes, ScopeTemplates, g_entitiesFactories
from gui.Scaleform.framework.entities.View import View, ViewKey
from gui.Scaleform.framework.managers.loaders import SFViewLoadParams

MY_ALIAS = 'myView'
MY_SWF_PATH = 'test/MyView.swf'

pyView = None

def init():
    print 'init'
    settings = ViewSettings(MY_ALIAS, MyView, MY_SWF_PATH, ViewTypes.WINDOW, None, ScopeTemplates.DEFAULT_SCOPE)
    g_entitiesFactories.addSettings(settings)
    g_eventBus.addListener(events.AppLifeCycleEvent.INITIALIZED, onAppInitialized)

def onAppInitialized(event):
    print 'onAppInitialized'
    if event.ns != APP_NAME_SPACE.SF_LOBBY:
        return
    showWindow()

def showWindow():
    print 'showWindow'
    app = g_appLoader.getDefLobbyApp()
    app.loadView(SFViewLoadParams(MY_ALIAS))
    global pyView
    pyView = app.containerManager.getViewByKey(ViewKey(MY_ALIAS))

class MyView(View):
    def _populate(self):
        super(MyView, self)._populate()
        self.flashObject.root.window.title = 'TEST WINDOW'
        self.flashObject.root.width = 380
        self.flashObject.root.height = 240
```

```actionscript
package
{
    import net.wg.infrastructure.base.AbstractWindowView;
    
    public class Main extends AbstractWindowView
    {
    }
}
```
