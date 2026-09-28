# Extended Kalman Filter (EKF) Sensor Fusion for Dead Reckoning

## State Space Formulation

When GPS telemetry is lost or degraded in indoor tunnels, urban canyons, or denied environments, vehicle position and velocity must be estimated via sensor fusion of:
1. 6-DOF Inertial Measurement Unit (Triaxial Accelerometer + Gyroscope)
2. Wheel Speed Encoders (Odometry)
3. Magnetometer (Heading reference)

### State Vector

The state vector at time step `k` is defined as:

```
x_k = [ p_x,  p_y,  p_z,  v_x,  v_y,  v_z,  q_0,  q_1,  q_2,  q_3,  b_ax, b_ay, b_az, b_wx, b_wy, b_wz ]^T
```

Where:
- `p`: 3D position in local NED (North-East-Down) navigation frame.
- `v`: 3D velocity in local NED frame.
- `q`: Unit quaternion representing orientation.
- `b_a`: Dynamic accelerometer bias drift vector.
- `b_w`: Dynamic gyroscope bias drift vector.

---

## Predict and Update Cycle

### 1. Propagation Step (High Frequency ~200 Hz)

High-rate IMU delta-velocities and delta-angles drive non-linear state propagation:

```
x_hat_{k|k-1} = f(x_hat_{k-1|k-1}, u_k)
P_{k|k-1} = F_k * P_{k-1|k-1} * F_k^T + Q_k
```

Where `F_k` is the Jacobian matrix of partial derivatives of `f` with respect to `x`, and `Q_k` is discrete process noise covariance.

### 2. Correction Step (Lower Frequency ~20-50 Hz)

When wheel tick odometry or sparse GNSS fixes become available, the Kalman gain `K_k` weights measurement residuals:

```
y_k = z_k - h(x_hat_{k|k-1})
S_k = H_k * P_{k|k-1} * H_k^T + R_k
K_k = P_{k|k-1} * H_k^T * S_k^{-1}
x_hat_{k|k} = x_hat_{k|k-1} + K_k * y_k
P_{k|k} = (I - K_k * H_k) * P_{k|k-1}
```

Zero Velocity Updates (ZUPT) and non-holonomic vehicle constraints (`v_y = 0`, `v_z = 0` in body frame) prevent exponential dead reckoning drift.
