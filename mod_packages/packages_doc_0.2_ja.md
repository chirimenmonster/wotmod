---
layout: page
title: "World of Tanks: Mod Packages"
---
原文: 
[packages_doc_0.2.pdf](../resources/packages_doc_0.2.pdf),
[packages_doc_0.2_en.pdf](../resources/packages_doc_0.2_en.pdf)

### version 0.2, 2017-04-10

+ Anton Bobrov, Wargaming.net
+ Mikhail Paulyshka, XVM team
+ Andrey Andruschyshyn, Independent
+ Koreanrandom.com community

ライセンス: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

## 1. 一般情報

パッケージ形式とは、mod ファイルを整理する方法です。
この形式により特定の mod が1つのファイルにまとめられます。

古い配布形式では、 mod はフォルダ `<WoT_game_folder>/res_mods/<WoT_version>/` にインストールされます。
この形式だと異なる mod のファイルが同じフォルダにあるので、特定の mod ファイルを見つけるのが難しくなります。

パッケージ形式では、mod ファイルの配置がずっと単純になります。
mod をインストールするには、フォルダ `<WoT_game_folder>/mods/<WoT_version>/` にパッケージをコピーするだけですみます。
また、mod をアンインストールするには、同じファイルを削除します。

## 2. パッケージの構成

パッケージは拡張子が `.wotmod` の非圧縮 **zip** ファイルです。

**注:** 圧縮されたアーカイブは、現在のバージョンの World of Tanks ではサポートされていません。
したがって、アーカイブを作成する際に圧縮レベルを「圧縮なし」に設定してください。

パッケージには以下の内容が含まれます:

+ 必須: フォルダ `/res/`。mod の中身がこのフォルダに置かれます。
フォルダ `<WoT_game_folder>/res_mods/<WoT_version>` にインストールされるファイルが該当します。
+ 省略可: ファイル `meta.xml` ([**5節**](#5-メタデータファイル-metaxml) を参照)
+ 省略可: ファイル `LICENSE`。ライセンス条件です。
+ 省略可: mod 開発者が必要と判断したその他のファイル。webページへのリンクや説明書、変更記録など。

パッケージの構成例:

    /package.wotmod
        /meta.xml
        /README.md
        /LICENSE
        /res
            /scripts
                /client
                    /gui
                        /mods
                            /mod_example.pyc

## 3. パッケージのインストール

パッケージはフォルダ `<WoT_game_folder>/mods/<WoT_version>` にインストールされます。
手動でコピーするか、もしくは mod または mod パックのインストーラによりインストールすることが可能です。

必要に応じて、パッケージをサブフォルダに分割することができます。
これにより、開発者はグループ別にファイルを整理できます:

    mods/
        0.9.17.1/
            MultiHitLog_2.8.wotmod
            DamagePanel/
                Some_common_library_3.14.5.wotmod
                DamagePanel_2.6.wotmod
                DamagePanel_2.8.wotmod
                DamagePanel_2.8_patch1.wotmod

## 4. パッケージの命名に関する推奨事項

パッケージの名称 (以下 `package_id`) は次の形式が推奨されます:

    package_id = author_id.mod_id

ここで:

+ `author_id`: 開発者の識別子。開発者のwebサイト (`com.example`) や開発者のニックネーム (`noname`) が可能です。
+ `mod_id`: modの識別子。開発者が任意に選択します。
この名称は `meta.xml` ファイル ([**5節**](#5-メタデータファイル-metaxml) 参照) の内部や、パッケージのファイル名の一部として使用されます。

パッケージ名称の例:

    com.example.coolmod
    noname.supermod

ファイル名は以下のような形式となります:

    <author_id>.<mod_id>_<mod_version>.wotmod

ここで:

+ `mod_version`: mod のバージョン。`meta.xml` 内で開発者によって設定される ([**5節**](#5-メタデータファイル-metaxml) 参照)。

例:

    com.example.coolmod_0.1.wotmod
    noname.supermod_0.2.8.wotmod

## 5. メタデータファイル `meta.xml`

省略可のファイル `meta.xml` には mod に関する情報が特定のフィールドとして含まれます。

例:

```xml
<root>
    <!‐‐ Techical MOD ID ‐‐>
    <id>noname.crosshair</id>

    <!‐‐ Package version ‐‐>
    <version>0.2.8</version>

    <!‐‐ Human readable name ‐‐>
    <name>Crosshair</name>

    <!‐‐ Human readable description ‐‐>
    <description>New cool Crosshair with feature1.....N</description>
</root>
```

これらのフィールドに指定された値は、mod 管理システムで使用されることになります。

## 6. パッケージの読み込み

### 6.1 読み込み順序

フォルダ `<WoT_game_folder>/mods/<WoT_version>/` にあるすべてのパッケージは、
ファイル `meta.xml` で指定されたノード `<id>` の値によってソートされ、この順序に従ってロードされます。
ファイル `meta.xml` がない場合はパッケージIDとしてファイル名が使用されます。

ファイル `load_order.xml` は、読み込み順序を変更するために使用できます。
前述のフォルダに配置する必要があります。

すべてのパッケージがファイル `load_order.xml` で指定されている場合、それらはファイル内で設定された順序に従って読み込まれます。

いくつかのパッケージがファイル `load_order.xml` で指定されていない場合、
`load_order.xml` で指定されたパッケージが最初に読み込まれます。
残りのパッケージはアルファベット順に読み込まれます。

**注:** 現在のところ、ファイル `load_orders.xml` の利用はかなり難しいものとなります ([**9.4節**](#94-パッケージの読み込み順序の変更) 参照)。

### 6.2 フォルダ `res_mods` とパッケージの併用

ゲームクライアント側から見た場合、仮想システムルートは次のように構成されています:

+ `/res_mods/<WoT_version>`
+ `/mods/<WoT_version>/<package_name>.wotmod/res/`
+ `/res/packages/*.pkg/`
+ `/res/`
+ ファイル `<WoT_game_folder>/paths.xml` で指定されたその他の場所

これらのパスは、優先度の高い順にリストされます。
つまり、フォルダ `/res_mods/<WoT_version>/` にあるファイルは、ファイル `load_order.xml` に関係なく、より高い優先度を持ちます。

### 6.3 読み込み時に発生する競合の解決

通常、パッケージ形式では、異なるパッケージのフォルダ `res/` に同名のファイルが存在する状況がありえます。
このような状況は競合とみなされます。

競合が検出された場合、競合するパッケージは完全に処理から除外されます。
そのようにして、対応する通知がユーザに表示されます。

言い換えると、`a.wotmod` と `b.wotmod` の両方のパッケージにファイル `res/scripts/entities.xml` が含まれている場合、
`a.wotmod` パッケージは正常に読み込まれますが、 `b.wotmod` は競合を引き起こし、読み込まれません。

競合の制御は次のように行います:

1. ファイル `load_order.xml`。このファイルで指定されたパッケージは競合とみなされません。同名ファイルをチェックせずに読み込まれます。
2. `meta.xml` のノード `<id>`  の値。同一の `<id>` を持つパッケージは、1つの mod の異なるバージョンまたは一部と見なされます。
これらの要素間の競合は考慮されません。

異なるパッケージに同名のファイルが含まれていて、それらの競合が `load_order.xml` または `meta.xml` ファイルで解決された場合、
最近追加されたパッケージに含まれるファイルの方が優先度が高くなります。

### 6.4 Python コードの実行

すべてのパッケージを追加して競合を解決した後、フォルダ `/scripts/client/gui/mods/` 内の
ファイル名の先頭が `mod_` であるすべての `.pyc` ファイルがアルファベット順に実行されます。

パッケージ内では、このファイルは次の場所にあります:

    <author_id>.<mod_id>_<version>.wotmod/res/scripts/client/gui/mods/mod_<anything>.pyc

## 7. mod ファイルの推奨パス

### 7.1 設定ファイル

mod の設定ファイルは次の場所が推奨されます:

    <WoT_game_foler>/mods/configs/<author_id>.<mod_id>/

ここで:

+ `author_id` と `mod_id` - 本文書の [**4節**](#4-パッケージの命名に関する推奨事項) で説明した識別子

### 7.2 ログファイル

標準ファイルの `python.log` とは別に、以下のパスの仕様が推奨されます:

    <WoT_game_folder>/mods/logs/<author_id>.<mod_id>/

ここで:

+ `author_id` と `mod_id` - 本文書の [**4節**](#4-パッケージの命名に関する推奨事項) で説明した識別子

### 7.3 一時ファイル

一時ファイルの置き場は以下が推奨されます:

    <temp>/world_of_tanks/<author_id>.<mod_id>/

ここで:

+ `temp` - OS の現在のユーザに対応する一時ファイル用フォルダのパス
+ `author_id` と `mod_id` - 本文書の [**4節**](#4-パッケージの命名に関する推奨事項) で説明した識別子

### 7.4 その他の mod ファイル

ゲームクライアントでアクセス可能なコンテンツを格納するには、次のパスを使用します:

    <package_name>.wotmod/res/mods/<author_id>.<mod_id>/

ここで:

+ `author_id` と `mod_id` - 本文書の [**4節**](#4-パッケージの命名に関する推奨事項) で説明した識別子

## 8. パッケージ内のファイルの操作

パッケージ内のファイルを操作するにはモジュール `ResMgr` を使います。

### 8.1 標準的な操作

#### 8.1.1 パッケージからのファイルの読み込み

```python
#import
import ResMgr

#function
def read_file(vfs_path, read_as_binary=True):
    vfs_file = ResMgr.openSection(vfs_path)
    if vfs_file is not None and ResMgr.isFile(vfs_path):
        if read_as_binary:
            return str(vfs_file.asBinary)
        else:
            return str(vfs_file.asString)
    return None

#example
myscript = read_file('scripts/client/gui/mods/mod_mycoolmod.pyc')
```

#### 8.1.2 フォルダ内の要素のリストを取得

```python
#import
import ResMgr

#function
def list_directory(vfs_directory):
    result = []
    folder = ResMgr.openSection(vfs_directory)

    if folder is not None and ResMgr.isDir(vfs_directory):
        for name in folder.keys():
            if name not in result:
                result.append(name)

    return sorted(result)

#example
content = list_directory('scripts/client/gui/mods/')
```

#### 8.1.3 パッケージからフォルダへのファイルのコピー

```python
#import
import os
import ResMgr

#function
def file_copy(vfs_from, realfs_to)
    realfs_directory = os.path.dirname(realfs_to)
    if not os.path.exists(realfs_directory):
        os.makedirs(realfs_directory)

    vfs_data = file_read(vfs_from) #view 8.1.1
    if vfs_data:
        with open(realfs_to, 'wb') as realfs_file:
            realfs_file.write(vfs_data)

#example
file_copy('scripts/client/gui/mods/mod_my.pyc','res_mods/0.9.17.1/scripts/client/gui/mods/mod_my.pyc')
```

## 9. 既知の問題点

### 9.1 大文字と小文字が区別されるファイル名

#### 問題の説明

現在、仮想ファイルシステムにファイルを追加する場合：

+ パッケージからのファイルは小文字で追加されます
+ フォルダ `<WoT_game_folder>/res_mods/` のファイルはそのまま追加されます

その結果、ファイルがパッケージと `res_mods`フォルダの両方にあり、ファイル名に少なくとも1つの大文字が含まれていると、
ファイルが2回読み込まれることがあります。

#### 一時的な解決策

`<WoT_game_folder>/res_mods` フォルダのファイルとフォルダの名前には小文字のみを使用してください。

### 9.2 GNU Gettext ファイルの操作

#### 問題の説明

現在、 `<WoT_game_folder>/res/text/LC_MESSAGES/` フォルダにある `.mo` ファイルの代わりに
`.mo` ファイルをパッケージに割り当てることはできません。

#### 一時的な解決策

一時的な解決策としてnet.openwg.vfsgettextを使用します:
http://openwg.net/download/vfsgettext/net.openwg.vfsgettext_1.0.0.wotmod

### 9.3 `.py` ファイルの実行

#### 問題の説明

現在、パッケージにある `.py` ファイルは実行できません。

#### 一時的な解決策

コンパイルされたバイトコード `.pyc` ファイルを `.py` ファイルに加えてパッケージに追加します。

### 9.4 パッケージの読み込み順序の変更

#### 問題の説明

現在、ファイル `load_order.xml` を使用してパッケージの読み込み順序を変更することはできません。

#### 一時的な解決策

この問題に対する一時的な解決策はありません。 問題はまもなく解決される予定です。

## 付録 A. 変更リスト

### v 0.2 (2017-04-10)

+ reworked design: new layout, separation into articles
+ reworked description of the package naming scheme
+ reworked description of the package loading order
+ added recommendations concerning the locations of logs and configuration files
+ added examples of the source code for working with files within packages
+ added description of currently known issues

### v 0.1 (2017-01-13)

+ First versio
