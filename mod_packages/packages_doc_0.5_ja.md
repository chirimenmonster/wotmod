---
layout: post
title: "World of Tanks: Mod Packages ver. 0.5"
excerpt: "公式ドキュメント World of Tanks: Mod Packages ver. 0.5 の翻訳"
date: 2017-12-09 08:00 +0900
---
原文:
[packages_doc_0.5_ru.pdf](https://koreanrandom.com/forum/applications/core/interface/file/attachment.php?id=119661),
[packages_doc_0.5_en.pdf](https://koreanrandom.com/forum/applications/core/interface/file/attachment.php?id=119781)

### version 0.5, 2017-10-12

### World of Tanks 0.9.20.1

|Author||Contact information|
|:---|:---|:---|
|Anton Bobrov|Wargaming.net||
|Mikhail Paulyshka|XVM team|mixail@modxvm.com|
|Andrey Andruschyshyn|Individual||
|The Koreanrandom.com community|||

ライセンス: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

## 1. 一般情報

パッケージ形式とは、mod のファイル群を整理してまとめる仕組みです。
この形式によって、特定の mod のすべての内容が1つのファイルにまとめられます。

古い配布形式では、 mod はフォルダ `<WoT_game_folder>/res_mods/<WoT_version>/` にインストールされます。
この形式では、異なる mod のファイルが同じフォルダに入ることになるので、特定の mod ファイルを見つけるのが難しくなります。

パッケージ形式では、mod ファイルの整理がずっと単純になります。
mod をインストールするには、フォルダ `<WoT_game_folder>/mods/<WoT_version>/` にパッケージをコピーするだけですみます。
また、mod をアンインストールするには、同じファイルを削除するだけです。

## 2. パッケージの構成

パッケージは以下の仕様の **zip** ファイルです。

+ 非圧縮
+ 拡張子: `.wotmod`
+ 最大アーカイブサイズ: 2 Gb - 1 バイト (2,147,483,647 バイト) 

**注:** 圧縮されたアーカイブは、現在のバージョンの World of Tanks ではサポートされていません。
したがって、アーカイブを作成する際に圧縮レベルを「圧縮なし」に設定してください。

**注:** 2 Gb 以上のアーカイブは現在の World of Tanks ではサポートされていません。
大きなサイズのファイルは小さなサイズに分割し、各ファイルサイズが 2 Gb - 1 バイトを超えないようにする必要があります。

パッケージには以下の内容が含まれます:

+ 必須: フォルダ `/res/`。mod の内容がこのフォルダに置かれます。
フォルダ `<WoT_game_folder>/res_mods/<WoT_version>` にインストールされていたファイルが該当します。
+ 省略可: ユーティリティファイル `meta.xml` ([**5節**](#5-メタデータファイル-metaxml) を参照)
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

パッケージ識別子 (下記の `package_id`) は次の形式が推奨されます:

    package_id = author_id.mod_id

ここで:

+ `author_id`: 開発者の識別子。開発者のwebサイト (`com.example`) か開発者のニックネーム (`noname`) が使用できます。
+ `mod_id`: mod 識別子。開発者が任意に選択します。

パッケージ識別子は `meta.xml` ファイルの `<id>` フィールドで使用 ([**5節**](#5-メタデータファイル-metaxml) 参照) されるほか、
パッケージファイル名の一部としても使用されます。

パッケージ識別子の例:

+ `com.example.coolmod`
+ `noname.supermod`

パッケージの名前は以下のような形式となります:

    <author_id>.<mod_id>_<mod_version>.wotmod

ここで:

+ `mod_version`: mod のバージョン。
mod の作者によって `meta.xml` ファイルの `<version>` フィールドに設定される ([**5節**](#5-メタデータファイル-metaxml) 参照)。

ファイル名の例:

+ `com.example.coolmod_0.1.wotmod`
+ `noname.supermod_0.2.8.wotmod`

## 5. メタデータファイル `meta.xml`

省略可のファイル `meta.xml` には mod に関する情報が特定のフィールドとして含まれます。

例:

```xml
<root>
    <!-- Package identifier -->
    <id>noname.crosshair</id>

    <!-- Package version -->
    <version>0.2.8</version>

    <!-- Package name clear for players -->
    <name>Crosshair</name>

    <!-- Package description -->
    <description>New cool Crosshair with feature1.....N</description>
</root>
```
フィールド `<id>` および `<version>` の値はパッケージを組み込む順序を決定するために使用されます。
フィールド `<name>` および `<description>` の値は、将来的に mod 管理システムで使用されることになります。

## 6. パッケージの読み込み

### 6.1 読み込み順序

フォルダ `<WoT_game_folder>/mods/<WoT_version>/` にあるすべてのパッケージは、
ファイル `meta.xml` で指定されたノード `<id>` の値によってソートされ、この順序に従って読み込まれます。
ファイル `meta.xml` がない場合はパッケージ識別子としてファイル名が使用されます。

ファイル `load_order.xml` は、読み込み順序を変更するために使用できます。
前述のフォルダに配置する必要があります。

すべてのパッケージがファイル `load_order.xml` で指定されている場合、
パッケージはファイル内で設定された順序に従って読み込まれます。

いくつかのパッケージがファイル `load_order.xml` で指定されていない場合、
`load_order.xml` で指定されたパッケージが最初に読み込まれます。
残りのパッケージはアルファベット順に読み込まれます。


### 6.2 フォルダ `res_mods` とパッケージの併用

ゲームクライアント側から見た場合、仮想システムの起点は次のように構成されています:

+ `/res_mods/<WoT_version>`
+ `/mods/<WoT_version>/<package_name>.wotmod/res/`
+ `/res/packages/*.pkg/`
+ `/res/`
+ ファイル `<WoT_game_folder>/paths.xml` で指定されたその他の場所

これらのパスは、優先度の高い順にリストされます。
つまり、フォルダ `/res_mods/<WoT_version>/` にあるファイルは、ファイル `load_order.xml` に関係なく、より高い優先度を持ちます。

### 6.3 読み込み時に発生する競合の解決

通常、パッケージシステムは、異なるパッケージのフォルダ `res/` に同名のファイルが存在する状況を許可していません。
このような状況は競合とみなされます。

競合が検出された場合、競合するパッケージは完全に処理から除外されます。
そのようにして、対応する通知がユーザに表示されます。

例えば、`a.wotmod` と `b.wotmod` の両方のパッケージにファイル `res/scripts/entities.xml` が含まれている場合、
`a.wotmod` パッケージは正常に読み込まれますが、 `b.wotmod` は競合を引き起こし、読み込まれません。

競合の制御は次のように行われます:

#### 1. ファイル `load_order.xml`

ファイル `load_order.xml` はフォルダ `<WoT_game_folder>/mods/<WoT_version>/` に置かれなければなりません。
以下のような形式になります:

```xml
<root>
    <Collection>
        <pkg>package_name_1.wotmod</pkg>
        <pkg>package_name_2.wotmod</pkg>
        <!-- ... -->
        <pkg>package_name_N.wotmod</pkg>
    </Collection>
</root>
```

このファイルに列挙されたパッケージは競合とみなされず、ファイル名の衝突解析をせずに読み込まれます。
優先順位は最後に指定されたパッケージからになります。

#### 2. `meta.xml` のノード `<id>` と `<version>` の値

ノード `<id>` がファイル　`meta.xml` で指定されている場合、
パッケージのファイル名は読み込み順序の決定に考慮されません。
同一の `<id>` を持つパッケージは、同一 mod の異なるバージョン、またはその一部と見なされます。
これらの要素間の競合は考慮されません。
これらのパッケージはバージョン順に読み込まれます (バージョンはノード `<version>` で指定)。

パッケージのバージョンは ASCII コード表に従って文字単位で比較されます。
この挙動は以下に示すように [strcmp()](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/strcmp-wcscmp-mbscmp) と同様の挙動となります:

+ バージョン `9.0.0` はバージョン `10.0.0` より優先度が高くなります。
+ バージョン `b` はバージョン `B` より優先度が高くなります。
+ バージョン `c<任意の文字列>` はバージョン `c` より優先度が高くなります。
+ バージョンが一致する場合は、パッケージはアルファベット順で読み込まれます。

異なるパッケージに同名のファイルが含まれていて、それらの競合が `load_order.xml` または `meta.xml` ファイルで解決された場合、
最近追加されたパッケージか `<version>` の値が大きいパッケージに含まれるファイルの方が優先度が高くなります。

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

### 9.1 `.py` ファイルの実行

#### 問題の説明

現在、パッケージにある `.py` ファイルは実行できません。

#### 一時的な解決策

コンパイルされたバイトコード `.pyc` ファイルを `.py` ファイルに加えてパッケージに追加します。

### 9.2 ZIP フォーマットの部分的なサポート

#### 問題の説明

現在、`.wotmod` はアーカイブ内のすべてのフォルダに `ZIPDIRENTRY` と `ZIPFILERECORD` を持つ必要があります。 

#### 一時的な解決策

アーカイブの作成に互換性のあるツールを使用します。
例えば、
+ 7-Zip [http://7-zip.org/](http://7-zip.org/)
+ Info-ZIP [http://info-zip.org/](http://info-zip.org/)


## 付録 A. 変更リスト


### v 0.5 (2017-10-12)

+ 9.2 節および 9.3 節を削除 (World of Tanks 9.20.1 にて修正)
+ ZIP フォーマットの部分的なサポートに関する記述を追加

### V 0.4 (2017-05-04)

+ ファイル `load_order.xml` によるパッケージ間の競合解決の説明 

### v 0.3 (2017-05-03)

+ ファイル形式 .wotmod の制限に関する情報を追加
+ IDが同じパッケージでの競合解決アルゴリズムの説明を追加

### v 0.2 (2017-04-10)

+ 構成の変更: 新規レイアウト、記事の分離
+ パッケージの命名方法に関する説明を改訂
+ パッケージの読み込み順序に関する説明を改訂
+ ログと設定ファイルの置き場に関する推奨事項を追加
+ パッケージ内のファイルを操作するソースコードの例を追加
+ 現在知られている問題の説明を追加

### v 0.1 (2017-01-13)

+ 最初のバージョン
