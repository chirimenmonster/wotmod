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
