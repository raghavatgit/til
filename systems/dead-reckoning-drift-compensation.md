# Inertial Dead Reckoning and Quadratic Drift Compensation

*Date: 2026-09-08*  
*Category: Navigation Systems / Sensor Fusion*

## Overview

In GPS-denied environments (tunnels, underground facilities, urban canyons), navigation systems rely on dead reckoning using Inertial Measurement Units (IMUs). Accelerometer double-integration calculates distance, but sensor bias causes position error to grow quadratically over time: E(t) proportional to t^2.

## The Drift Problem

Given measured acceleration a_meas(t) = a_true(t) + b(t) + n(t), where b is sensor bias and n is white noise:

1. Velocity integration: v(t) = integral(a(t) dt) -> Linear error growth: E_v(t) ~ b * t.
2. Position integration: p(t) = integral(v(t) dt) -> Quadratic error growth: E_p(t) ~ 0.5 * b * t^2.

Even a tiny accelerometer bias of 0.05 m/s^2 leads to a 250-meter position error in just 100 seconds without correction.

## Compensation Techniques

1. **Zero Velocity Updates (ZUPT):** Whenever static stationary conditions are detected (accelerometer variance falls below threshold), reset integrated velocity to zero. This halts velocity drift propagation.
2. **Complementary Filtering:** High-pass filter accelerometer data (which handles rapid dynamic maneuvers well) combined with low-pass filtered periodic GPS / GNSS position fixes when satellite reception briefly resumes.
3. **Attitude Quaternion Normalization:** Continually re-normalize attitude quaternions to prevent non-orthonormal coordinate transformation matrices during vehicle pitch and roll.
