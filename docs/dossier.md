---
layout: post
title: WoT Dossier ファイル メモ
date: 2018-05-27 08:00 +0900
last_modified_at: 2018-05-29 17:00 +0900
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
ログインサーバ、アカウント名、アカウントクラスを ';' (セミコロン) で連結したものになっています。

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

cPickle で復元されたオブジェクトは、
バージョンと中身で構成されていて、
さらに中身は Dossier データの辞書のリストになっています。
Dossier データの辞書は、
キーが dpssierType の種別と ownerID のタプル、
値が changeTime と dossierCompDescr のタプルになっています。

```python
{
    version,
    [
        { ( dossierType, ownerID ): ( changeTime, dossierCompDescr ) },
        ...
    ]
}
```

### dossierType

dossier の種別を表します。
scripts/common/constants.py で定義されている
クラス DOSSIER_TYPE の属性のいずれかの値をとります。
車輌戦績であれば DOSSIER_TYPE.VEHICLE (=2) です。


### ownerID

全アイテム間で固有の値となるように決められたIDです。
アイテム種別 (itemTypeID) と国 (nationID)、アイテム識別番号 (itemID) をエンコードしたものです。
itemTypeID や nationID が異なる場合には itemID は重複する場合があります。

ownerID は compactDescr として scripts/common/items/\__init__.py の
makeIntCompactDescrByID によってエンコード、
parseIntCompactDescr によってデコードされます。

```python
def makeIntCompactDescrByID(itemTypeName, nationID, itemID):
    itemTypeID = ITEM_TYPES[itemTypeName]
    if itemTypeID <= 15:
        header = itemTypeID + (nationID << 4)
        return (itemID << 8) + header
    if itemTypeID <= 255:
        header = 0 + (nationID << 4)
        return (itemTypeID << 24) + (itemID << 8) + header
```

```python
def parseIntCompactDescr(compactDescr):
    itemTypeID = compactDescr & 15
    if itemTypeID == 0:
        itemTypeID = compactDescr >> 24 & 255
    return (itemTypeID, compactDescr >> 4 & 15, compactDescr >> 8 & 65535)
```

ownerID は32bit長の整数で、itemTypeID の大きさに応じて以下のように2種類のフォーマットになっています。

itemTypeID が 15 以下の場合:
```
 0           4           8           12          16          20          24          28       31
|--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--|
|itemTypeID | nationID  |                  itemID(16)                   |           0           |
```

itemTypeID が 16 以上の場合:
```
 0           4           8           12          16          20          24          28       31
|--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--|
|     0     | nationID  |                  itemID(16)                   |      itemTypeID       |
```

#### itemTypeID

+ 値域 [1, 15] (4bit長) または [16, 255] (8bit長)
+ 15 以下と 16以上で ownerID のフォーマットが異なる
+ 名称との対応表は scripts/common/items/\__init__.py の ITEM_TYPE_NAMES から生成される
+ 'vehicle' の場合は 1

#### nationID
+ 値域 [0, 15] (4bit長)
+ 名称との対応表は scripts/common/nations.py の NAMES から生成される
+ 'ussr' は 0, 'germany' は 1, 'italy' は 10

#### itemID
+ 値域 [0, 65535]  (16bit長)
+ itemTypeID と nationID の組み合わせに対して固有の値となるように ID が定義されている
+ 例えば itemTypeID=1 (vehicle) かつ nationID=0 (ussr) であれば
ソ連車輌の ID と解釈される
+ ソ連車輌の ID は scripts/item_defs/vehicles/ussr/list.xml で定義される


itemTypeID + nationID * 16 + itemID * 256 (itemTypsID <= 15 の場合)

itemTypeID * 2^24 + nationID * 16 + itemID * 256 (itemTypsID <= 15 の場合)


### changeTime

dossier データの更新時刻です。
POSIX タイムスタンプの形式なので、
Python の datetime.fromtimestamp で datetime オブジェクトに変換できます。

```python
from datetime import datetime
print datetime.fromtimestamp(changeTime)
```


### dossierCompDescr

パックされたバイナリデータです。
リトルエンディアンです。




## Dossier 関連コード

WoT 内の Dossier の処理は scripts/common/dossiers2 で行われています。

