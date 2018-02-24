---
layout: page
title: エンジントルク
mathjax: true
sitemap: false
---




## トルクと加速度

トルク (Nm) $T$,
回転速度 (rpm) $N$,
出力 (W) $P$
の関係

$$
P = \frac{2\pi N}{60}\cdot T
$$

車輌速度 (km/h) $v$,
駆動輪半径 (m) $r$,
回転速度 (rpm) $N$,
変速比 transmission ratio $R_t$
減速比 drive ratio $R_d$
の関係

$$
v = \frac{2\pi r \cdot N / 60}{R_t \cdot R_d} \cdot \frac{3600}{1000}
$$

車輌質量 (kg) $m$,
加速度 (m/s<sup><small>2</small></sup>) $a$

$$
m (a + \mu'g) = F = \frac{T \cdot R_t \cdot R_d}{r}
$$


### 速度とエンジン回転数の関係

WoT では速度とエンジン回転数の内部表現の比は一定です [要出典]。

$$
v = r \cdot v_\mathrm{limit}
$$

ここで $r$ の値域は [0, 1] です。


### 演出用の RPM

表示、演出用の RPM　は次のように計算されます。

制限速度を 1/3 した値をギア毎の速度幅とします。
速度を速度幅で除して丸めて1を足した値を現在のギアとします。
値は 1-4　で、4は超過を表します。
それぞれのギアの段数に

$$
v_\mathrm{gear} = v_\mathrm{limit} / 3
$$

$$
n = \lceil v / v_\mathrm{gear} \rceil
$$

$$
r = v - (n-1) \cdot v_\mathrm{gear}
$$

$$
r' =
\begin{cases}
0.3 + 0.7 \cdot r & \text{($r < 1.0$)} \\
r & \text{($r \geq 1.0$)}
\end{cases}
$$




### 計算

AMX40 のエンジン回転数 2000 - 2500 rpm のとき、
出力 190 hp で一定

2000 rpm のとき

$$
T = 190 \times 735.5 \times \frac{60}{2 \times 3.14 \times 2000} = 667.6 ~\mathrm{(Nm)}
$$

2500 rpm のとき

$$
T = 190 \times 735.5 \times \frac{60}{2 \times 3.14 \times 2500} = 534.1 ~\mathrm{(Nm)}
$$


## 現在値の取得

### 車輌 (class Vehicle.Vehicle)

```python
import BigWorld
vehicle = BigWorld.player().vehicle
```

### 速度 (m/s)

speedInfo は4次元のベクトル量。
速度、ロール、ピッチ、ヨー （ロール、ピッチ、ヨーの順序は未確認）

```python
vehicle.speedInfo[0]
```

### エンジン回転数 (rpm)

表示用の値なので物理モデルとの対応関係は不明

```python
vehicle.appearance.rpm
vehicle.appearance._CompoundAppearance__detailedEngineState.rpm
vehicle.appearance._CompoundAppearance__detailedEngineState.relativeRPM
```

scripts/client/gui/battle_control/controllers/vehicle_state_ctrl.py

```python
    def __simRpm(self, vehicle):
        """
        Updates current rpm based on vehicle's speed and the current gear. Used algorithm from
        engine_states.py (see refresh method)
        
        :param vehicle: A reference to Vehicle(BW entity) object.
        """
        try:
            vehicleSpeed = vehicle.speedInfo.value[0]
            speedRangePerGear = (self.__speedLimits[0] + self.__speedLimits[1]) / 3.0
        except (AttributeError, IndexError, ValueError):
            LOG_CURRENT_EXCEPTION()
            return

        gearNum = math.ceil(math.floor(math.fabs(vehicleSpeed) * 50.0) / 50.0 / speedRangePerGear)
        if gearNum:
            self.__rpm = math.fabs(1.0 + (vehicleSpeed - gearNum * speedRangePerGear) / speedRangePerGear)
        else:
            self.__rpm = 0.0

    def __scaleRmp(self, rpm, vehicle):
        """
        Calculates a new value for shifted rpm scale (In moving, rpm scale is shifted from
        0.0-1.2 to 0.3-1.2).
        
        :param rpm: True rpm value to be converted.
        :param vehicle: A reference to Vehicle(BW entity) object.
        """
        if vehicle.appearance.gear == _STOPPED_ENGINE_GEAR:
            rpm = 0
        elif vehicle.appearance.gear > 0 and rpm < _RPM_MAX_VALUE:
            rpm = _RPM_SHIFT + rpm * _RPM_SHIFT_FACTOR
        return rpm
```

### 出力

車輌のマスクデータとして smplEnginePower というパラメータも定義されているのですが、
詳細は不明です。
client/gui/shared/items_parameters/param.py での使用箇所を見ると
限界速度 (制限速度ではなく速度の上限値) の計算に用いられているようです。

```python
    def __getRealSpeedLimit(self):
        enginePower = self.__getEnginePhysics()['smplEnginePower']
        rollingFriction = self.__getChassisPhysics()['grounds']['ground']['rollingFriction']
        return enginePower / self.vehicleWeight.current * METERS_PER_SECOND_TO_KILOMETERS_PER_HOUR * self.__factors['engine/power'] / 12.25 / rollingFriction
```

この中の 12.25 という値もよくわかりませんね。
本来であれば、ここには重力加速度として 9.81 が来るはずです。

この限界速度の値は relativeMobility の計算の一部に使用されます。

### 転がり摩擦 (rolling friction)

転がり摩擦 (rolling friction) は履帯 (chassis) のパラメータとして定義されています。
対象物には ground, sand, wood, snow, stone, metal がありますが、
現在 (0.9.22.0.1) では対象物によらず同じ値が設定されているようです。

値は履帯によって異なりますが、おおむね 0.1 前後です。




## scripts/common/items/vehicles.py

```python
            defResistance = chassis.terrainResistance[0]
            rotationEnergy = type.engines[0].power * (weight / defWeight) / (chassis.rotationSpeed * defResistance)
            rotationSpeedLimit = physics['enginePower'] / (rotationEnergy * physics['terrainResistance'][0])
            if not chassis.rotationIsAroundCenter:
                rotationEnergy -= trackCenterOffset * weight * chassis.specificFriction / defResistance
                if rotationEnergy <= 0.0:
                    raise Exception('wrong parameters of rotation of ' + type.name)
            if chassis.rotationSpeedLimit is not None:
                rotationSpeedLimit = min(rotationSpeedLimit, chassis.rotationSpeedLimit)
            physics['rotationSpeedLimit'] = rotationSpeedLimit
            physics['rotationEnergy'] = rotationEnergy
            physics['massRotationFactor'] = defWeight / weight
```