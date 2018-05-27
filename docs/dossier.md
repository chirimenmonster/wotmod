---
layout: post
title: WoT Dossier ファイル メモ
date: 2018-05-27 08:00 +0900
---

WoT の戦績データ Dossier ファイルに関するメモです。

## ファイルの場所

%AppData%\Wargaming.net\WorldOfTanks\dossier_cache
に作成されます。


## ファイル名

ファイル名は以下のように、英数字列に "=" を付加したものに拡張子 ".dat" が付いた形式となっています。

```
O5XXIYLTNFQTCLJSFZWG6Z3JNYXHOYLSM5QW22LOM4XG4ZLUHIZDAMBRGY5UG2DJOJUW2ZLOHNIGYYLZMVZECY3DN52W45A=.dat
```

拡張子を除いた部分は、Base32 でエンコードした文字列で、
上記の例をデコードすると下記のようになります。

```
wotasia1-2.login.wargaming.net:20016;Chirimen;PlayerAccount
```

ファイル名は scripts/client/account_helpers/DossierCache.py で作成されています。

```python
class DossierCache(object):

    def __init__(self, accountName, accountClassName):
        self.__account = None
        self.__syncController = None
        p = os.path
        prefsFilePath = unicode(BigWorld.wg_getPreferencesFilePath(), 'utf-8', errors='ignore')
        self.__cacheDir = p.join(p.dirname(prefsFilePath), 'dossier_cache')
        self.__cacheFileName = p.join(self.__cacheDir, '%s.dat' % base64.b32encode('%s;%s;%s' % (str(BigWorld.server()), accountName, accountClassName)))
```


## データ

Dossier ファイル内のデータは Python のオブジェクトを cPickle でシリアライズしたものです。
ファイルの入出力やシリアライズ・復元は scripts/client/account_helpers/DossierCache.py で行っています。

cPickle でシリアライズされたオブジェクトを復元できます (pickle でも可)

```python
import os
import cPickle
from pprint import pprint

dir = '(AppDataの場所)/Wargaming.net/WorldOfTanks/dossier_cache'
file = '(Dossierファイル).dat'

with open(os.path.join(dir, file), 'r') as f:
    data = cPickle.load(f)
    pprint(data)
```




## Dossier 関連コード

WoT 内の Dossier の処理は scripts/common/dossiers2 で行われています。

