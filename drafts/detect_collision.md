---
layout: page
title: 衝突判定
sitemap: false
mathjax: true
---
**(作成中)**

衝突判定は
クラス VehicleGunRotator の
メソッド __getGunMarkerPosition で行われます。
VehicleGunRotator は `scripts/client/VehicleGunRotator.py` で定義されています。

__getGunMarkerPosition の返り値は、
着弾点、着弾点での砲弾の方向ベクトル、着弾散布界の直径(1)、着弾散布界の直径(2)、衝突データのタプルで、
衝突データは衝突までの距離と、車輌衝突情報のタプル、
さらに衝突車輌情報は衝突したエンティティ、衝突角度、衝突部の装甲厚の名前付きタプルです。

```python
return (endPos,
 dir,
 markerDiameter,
 idealMarkerDiameter,
 collData)
```

## 関数

+ computeProjectileTrajectory()：
指定された時間の範囲で砲弾の軌道を計算し、衝突判定を行う座標列を取得します。
時間の範囲は `SERVER_TICK_LENGTH` である 0.1 秒が指定されています。
+ collideVehiclesAndStaticScene():
砲弾の軌道 (線分) が指定のエンティティ群と交差するかどうかを検査します。
砲弾の軌道は2点で指定します。
交差する場合は、交点および交差したエンティティの情報を返します。
+ collideWithSpaceBB():
砲弾の軌道 (線分) が地形境界と交わるかどうかを検査します。
砲弾の軌道が地形境界と交わる場合はその交点を返します。


## computeProjectileTrajectory

砲弾の軌道は `SERVER_TICK_LENGTH` である 0.1 秒刻みで計算されますが、
衝突の判定の際にはさらに細部を分割して計算されます。

砲弾の軌道の詳細計算は common/projectile_trajectory.py 内の関数 computeProjectileTrajectory で行われます。
computeProjectileTrajectory の引数は以下のようになります。

```python
def computeProjectileTrajectory(beginPoint, velocity, gravity, time, epsilon):
```

`beginPoint` は計算の始点、
`velocity` は弾速、
`gravity` は重力加速度、
`time` は計算対象の時間、
`epsilon` は刻みの長さを表します。

呼び出し元の __getGunMarkerPosition では以下のようなコードになっています。

```python
checkPoints = computeProjectileTrajectory(prevPos, prevVelocity, gravity, SERVER_TICK_LENGTH, SHELL_TRAJECTORY_EPSILON_CLIENT)
```

`SHELL_TRAJECTORY_EPSILON_CLIENT` は 0.03 が設定されています
(ちなみにサーバ側で用いられる `SHELL_TRAJECTORY_EPSILON_SERVER` は 0.1 です)。

実際には BigWorld.wg_computeProjectileTrajectory が使われ、内部は不明。

```python
try:
    computeProjectileTrajectory = BigWorld.wg_computeProjectileTrajectory
except AttributeError:
    pass
```


### 計算手順

最初に始点から終点までの平面上の距離の2乗を求めます。

鉛直方向 (y方向) の速度が 0 となる時間 $t_e$ を求めます。

$$
t_e = - \frac{v_y}{g_y}
$$

極大点 $p_e$ の相対位置を求めます。

$$
p_e = v \cdot t_e + \frac{1}{2} g \cdot {t_e}^2
$$


## collideVehiclesAndStaticScene


```python
for curCheckPoint in checkPoints:
    testRes = collideVehiclesAndStaticScene(prevCheckPoint, curCheckPoint, testEntities)
```

### collideVehiclesAndStaticScene

関数 collideVehiclesAndStaticScene は
`scripts/client/ProjectileMover.py` で定義されており、
`startPoint` から `endPoint` に砲弾が移動する場合に、
車輌または静止オブジェクトに衝突するかどうかを調べます。

実際の衝突判定は、
車輌については関数 collideEntities で、
静止オブジェクトについては関数 BigWorld.wg_collideSegment で行われます。

衝突する場合は、`startPoint` に最も近いものが選ばれます。

返り値は、衝突までの距離と、衝突車輌の情報 (衝突したエンティティ、衝突角度、衝突部の装甲厚) のタプルです。
静止オブジェクトの場合、タプルの2番目の値は None となります。

```python
def collideVehiclesAndStaticScene(startPoint, endPoint, vehicles, collisionFlags = 128, skipGun = False):
    testResStatic = BigWorld.wg_collideSegment(BigWorld.player().spaceID, startPoint, endPoint, collisionFlags)
    testResDynamic = collideEntities(startPoint, endPoint if testResStatic is None else testResStatic[0], vehicles, skipGun)
    if testResStatic is None and testResDynamic is None:
        return
    else:
        distDynamic = 1000000.0
        if testResDynamic is not None:
            distDynamic = testResDynamic[0]
        distStatic = 1000000.0
        if testResStatic is not None:
            distStatic = (testResStatic[0] - startPoint).length
        if distDynamic <= distStatic:
            dir = endPoint - startPoint
            dir.normalise()
            return (startPoint + distDynamic * dir, testResDynamic[1])
        return (testResStatic[0], None)
        return
```

### collideEntities

関数 collideEntities は
`scripts/client/ProjectileMover.py`
で定義されており、
`startPoint` から `endPoint` に砲弾が移動する場合に、
`entities` で指定した対象物群に衝突するものがあるどうかを調べます。
衝突する場合は `startPoint` に最も近いものが選ばれます。
返り値は `dist` と EntityCollisionData のタプルで、
`dist` は `startPoint` からの衝突する対象物の距離、
EntityCollisionData は namedtuple の派生クラスで、
衝突したエンティティ、衝突角度、衝突部の装甲厚からなります。

```python
def collideEntities(startPoint, endPoint, entities, skipGun = False):
    res = None
    dir = endPoint - startPoint
    endDist = dir.length
    dir.normalise()
    for entity in entities:
        collisionResult = entity.collideSegment(startPoint, endPoint, skipGun)
        if collisionResult is None:
            continue
        dist = collisionResult[0]
        if dist < endDist:
            endPoint = startPoint + dir * dist
            endDist = dist
            res = (dist, EntityCollisionData(entity, collisionResult.hitAngleCos, collisionResult.armor))

    return res
```

```python
class EntityCollisionData(namedtuple('collisionData', ('entity', 'hitAngleCos', 'armor'))):
```

entity が車輌の場合の collideSegment は scripts/client/Vehicle.py の
クラス Vehicle　(BigWorld.Entity の派生クラス) で定義されている。

グローバル座標系から車輌モジュール局所座標系への変換マトリクスを作成する。
変換マトリクスは履帯 (車台)、車体、砲塔、主砲それぞれに作成され、
各局所座標系で衝突判定が行われる。

```python
tempCol = chassis['hitTester'].localHitTest(compMatrix.applyPoint(startPoint), compMatrix.applyPoint(endPoint))
if tempCol is not None:
    for dist, _, hitAngleCos, matKind in tempCol:
        if res is None or res[0] >= dist:
            matInfo = chassis['materials'].get(matKind)
            res = SegmentCollisionResult(dist, hitAngleCos, matInfo.armor if matInfo is not None else 0)
```

モジュールデータである chassis などは、クラス VehicleDescr (scripts/common/items/vehicles.py) の属性として取得できる。


scripts/common/ModelHitTester.py

```python
SegmentCollisionResult = namedtuple('SegmentCollisionResult', ('dist', 'hitAngleCos', 'armor'))
```

```python
def localHitTest(self, start, stop, value = 0):
    return self.__getBspModel(value).collideSegment(start, stop)
```

