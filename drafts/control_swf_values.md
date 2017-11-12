---
layout: page
title: Python スクリプトからの SWF 制御 (2) 値の受け渡し
sitemap: false
---
**(作成中)**

## 概要

* public にした関数、変数は python 側からアクセス可能
* AS3 で定義した関数から return で返した値を python 側で取得できる
* AS3 から python 側の関数を呼び出すには登録作業が必要
* g_entitiesFactories.addSettings() で python側の関数を AS3 に紐づけできる (未確認)


## SWF にパブリック変数を定義する

python 側から参照可能にするには変数をパブリックで宣言します。

```actionscript
public var as_testvar:int;
```

## SWF から値を返す

SWF の関数からは return で値を返すことができます。
関数の型宣言は返したい値の型に合わせておいてください。

```actionscript
public function as_addValue(param1:int):int
{
    return as_testvar + param1;
}
```

## Python 側からの値の参照と取得

```python
f.movie.root.as_var = 5
result = f.movie.root.as_addValue(3)    # result <= 8
```
