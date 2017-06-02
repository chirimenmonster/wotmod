---
layout: page
title: 照準車輌の特定
sitemap: false
mathjax: true
---
**(作成中)**

砲の向きなどから現在どの車輌を照準しているか調べるのは VehicleGunRotator.VehicleGunRotator クラスの __getGunMarkerPosition() で行われている。

```python
def __getGunMarkerPosition(self, shotPos, shotVec, dispersionAngles):
```

`shotPos`: 主砲の付け根の位置,
`shotVec`: 主砲の方向ベクトル,
`dispersionAngles`: 散布界


このうち、`shotPos`, `shotVec` は砲塔回転角 (turret yaw)、主砲仰俯角 (gun pitch) から計算される。
計算は __getShotPosition() で行われる。

```python
turretWorldMatrix = Math.Matrix()
turretWorldMatrix.setRotateY(turretYaw)
turretWorldMatrix.translation = turretOffs
turretWorldMatrix.postMultiply(Math.Matrix(self.__avatar.getOwnVehicleStabilisedMatrix()))
position = turretWorldMatrix.applyPoint(gunOffs)
```

turretWorldMatrix は変換行列である。
gunOffs は砲塔に対する主砲の (付け根の) 位置、
turretOffs は車体に対する砲塔の位置と車台 (履帯) に対する車体位置の和、つまり、車輌の基準点から砲塔位置のオフセットを表す。

```python
gunWorldMatrix = Math.Matrix()
gunWorldMatrix.setRotateX(gunPitch)
gunWorldMatrix.postMultiply(turretWorldMatrix)
vector = gunWorldMatrix.applyVector(Math.Vector3(0, 0, shotSpeed))
```

`Math.Vector3(0, 0, shotSpeed)` は奥行方向のベクトルを表す。
変換行列を適用することで、主砲の向きが世界座標でのベクトルに変換される。

### 探索

主砲位置から主砲の方向ベクトルの 10000 倍を探索の終端位置とする。
方向ベクトルの大きさは弾速となっている。

ProjectileMover.getCollidableEntities() により、
探索範囲の衝突要素のリストを得る。

`SERVER_TICK_LENGTH = 0.1`
0.1秒間の砲弾の軌道を計算する。
砲弾は重力によって下降し、鉛直方向に加速する。


### 砲弾の軌道から計算されるチェックポイントの集合

```python
checkPoints = computeProjectileTrajectory(prevPos, prevVelocity, gravity, SERVER_TICK_LENGTH, SHELL_TRAJECTORY_EPSILON_CLIENT)
```

### エンティティが砲弾の軌道に干渉するかどうかの検査

エンティティは車輌、オブジェクトが含まれる

```python
testRes = collideVehiclesAndStaticScene(prevCheckPoint, curCheckPoint, testEntities)
```

### 地面と砲弾の軌道の干渉チェック

```python
pos = collideWithSpaceBB(prevCheckPoint, curCheckPoint)
```

### 探索の終了

砲弾の軌道が車輌・オブジェクトに干渉するか、地面と干渉したら探索はそこで終了。
