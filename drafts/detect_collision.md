---
layout: page
title: 衝突判定
sitemap: false
mathjax: true
---
**(作成中)**

衝突判定。

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


## 計算手順

最初に始点から終点までの平面上の距離の2乗を求めます。

鉛直方向 (y方向) の速度が 0 となる時間 $t_e$ を求めます。

$$
t_e = - \frac{v_y}{g_y}
$$

極大点 $p_e$ の相対位置を求めます。

$$
p_e = v \cdot t_e + \frac{1}{2} g \cdot {t_e}^2
$$