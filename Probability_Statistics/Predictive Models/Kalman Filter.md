
Excellent Video Progressions to Explain Kalman Filter
https://youtu.be/HCd-leV8OkU
https://youtu.be/qCZ2UTgLM_g
https://youtu.be/DbE4PMgqp3s

# Kalman Filters

## Quick Reference Table

| Concept | Intuition | Formula |
|----------|----------|----------|
| Batch Average | Average all data at once | $\bar{x}_k = \frac{1}{k}\sum_{i=1}^{k}x_i$ |
| Recursive Average | Update average using previous average | $\bar{x}_k = \alpha\bar{x}_{k-1} + (1-\alpha)x_k$ |
| Moving Average | Average recent observations only | Window of size $N$ |
| Low-Pass Filter | Smooth noisy data while emphasizing recent data | $\hat{x}_k = \alpha\hat{x}_{k-1} + (1-\alpha)z_k$ |
| Kalman Filter | Adaptive low-pass filter | Gain changes automatically |
| Prediction Step | Predict next state using physics/model | $\hat{x}_k^{-}$ |
| Update Step | Correct prediction using measurement | $\hat{x}_k$ |
| Kalman Gain | Determines trust in measurement vs model | $K_k$ |
| Process Noise | Uncertainty in model | $Q$ |
| Measurement Noise | Uncertainty in sensor | $R$ |
| Error Covariance | Uncertainty in estimate | $P$ |

---

# Big Picture

The Kalman Filter is fundamentally:

```text
Prediction
+
Measurement
+
Adaptive Weighting
```

It is often described as:

```text
An optimal recursive estimator
```

for linear systems with Gaussian noise.

---

# Why Kalman Filters Exist

Suppose we are measuring altitude.

True altitude:

$$
100 \text{ meters}
$$

Sensor readings:

```text
98
104
97
102
101
96
```

The measurements are noisy.

Question:

```text
How do we estimate
the true altitude?
```

---

# Step 1: Batch Average

Given:

$$
x_1,x_2,\ldots,x_k
$$

Average:

$$
\bar{x}_k
=
\frac1k
\sum_{i=1}^{k}
x_i
$$

---

## Example

Measurements:

```text
10
20
30
```

Average:

$$
\bar{x}
=
\frac{10+20+30}{3}
=
20
$$

---

## Problem

When a new measurement arrives:

```text
10
20
30
40
```

we must recompute:

$$
\frac{10+20+30+40}{4}
$$

This becomes expensive for large datasets.

---

# Step 2: Recursive Average

Instead:

$$
\bar{x}_k
=
\frac{k-1}{k}
\bar{x}_{k-1}
+
\frac1k x_k
$$

Define:

$$
\alpha
=
\frac{k-1}{k}
$$

Then:

$$
\bar{x}_k
=
\alpha \bar{x}_{k-1}
+
(1-\alpha)x_k
$$

---

## Intuition

Previous estimate:

$$
\bar{x}_{k-1}
$$

contains all past information.

New measurement:

$$
x_k
$$

contains only new information.

Combine them.

---

# Example

Measurements:

```text
10
20
30
```

---

## First Estimate

$$
\bar{x}_1
=
10
$$

---

## Second Estimate

$$
\bar{x}_2
=
\frac12(10)
+
\frac12(20)
=
15
$$

---

## Third Estimate

$$
\bar{x}_3
=
\frac23(15)
+
\frac13(30)
=
20
$$

Same answer.

Much less computation.

---

# Problem With Averages

Suppose altitude changes:

```text
100
101
102
103
104
```

A full average becomes:

$$
102
$$

which is not the current altitude.

It is lagging behind reality.

---

# Step 3: Moving Average

Average only recent observations.

Window size:

$$
N
=
10
$$

Example:

```text
Last 10 measurements only
```

---

## Benefit

Tracks changing signals.

---

## Problem

Every measurement receives equal weight.

Example:

```text
Newest Reading = 104

Oldest Reading = 95
```

Both receive:

$$
\frac1N
$$

weight.

This causes delay.

---

# Step 4: Low-Pass Filter

Idea:

Recent measurements should matter more.

Use:

$$
\hat{x}_k
=
\alpha \hat{x}_{k-1}
+
(1-\alpha)z_k
$$

where:

- $\hat{x}_k$ = estimate
- $z_k$ = measurement
- $\alpha$ = smoothing factor

---

# Interpretation

If:

$$
\alpha = 0.95
$$

then:

```text
Trust previous estimate heavily
```

---

If:

$$
\alpha = 0.1
$$

then:

```text
Trust measurement heavily
```

---

# Why This Is Better

Expanding recursively:

$$
\hat{x}_k
=
(1-\alpha)z_k
+
\alpha(1-\alpha)z_{k-1}
+
\alpha^2(1-\alpha)z_{k-2}
+\cdots
$$

Recent data receives larger weights.

Older data receives exponentially smaller weights.

---

# The Key Limitation

Low-pass filter requires:

$$
\alpha
$$

to be chosen manually.

Bad choice:

```text
Too noisy
```

or

```text
Too sluggish
```

---

# Kalman Filter Idea

The Kalman Filter automatically chooses:

$$
\alpha
$$

at every time step.

Instead of:

$$
\alpha
$$

we use:

$$
K_k
$$

called the Kalman Gain.

---

# Intuition Behind Kalman Gain

Suppose:

Model prediction:

$$
100
$$

Measurement:

$$
102
$$

Question:

```text
How much should we trust
the measurement?
```

Answer depends on:

- Sensor quality
- Model quality

---

# Case 1: Noisy Sensor

Prediction:

$$
100
$$

Measurement:

$$
102
$$

Sensor known to be noisy.

Result:

```text
Trust prediction
```

Estimate:

$$
100.2
$$

---

# Case 2: Accurate Sensor

Prediction:

$$
100
$$

Measurement:

$$
102
$$

Sensor highly accurate.

Result:

```text
Trust measurement
```

Estimate:

$$
101.9
$$

---

# Kalman Filter Structure

Two steps:

```text
Prediction
Update
Prediction
Update
Prediction
Update
...
```

---

# Step 1: Prediction

Predict future state.

Notation:

$$
\hat{x}_k^{-}
$$

Read as:

```text
Predicted state
before seeing measurement
```

---

## Formula

$$
\hat{x}_k^{-}
=
A\hat{x}_{k-1}
$$

---

# Intuition

Use physics/model.

Example:

Current position:

$$
100
$$

Current velocity:

$$
10
$$

Time step:

$$
1
$$

Prediction:

$$
110
$$

---

# Step 2: Predict Uncertainty

Prediction uncertainty:

$$
P_k^{-}
=
AP_{k-1}A^T
+
Q
$$

---

## Meaning

Prediction uncertainty grows.

Why?

Because models are imperfect.

---

# Process Noise Matrix

$$
Q
$$

Represents uncertainty in the model.

---

## Example

Perfectly constant voltage:

$$
Q = 0
$$

---

Rocket dynamics:

$$
Q > 0
$$

because disturbances exist.

---

# Step 3: Compute Kalman Gain

Formula:

$$
K_k
=
P_k^{-}
H^T
\left(
H P_k^{-} H^T
+
R
\right)^{-1}
$$

---

# Intuition

Large:

$$
R
$$

means:

```text
Measurement noisy
```

Trust model more.

---

Large:

$$
P_k^{-}
$$

means:

```text
Prediction uncertain
```

Trust measurement more.

---

# Step 4: Update Estimate

Core Kalman equation:

$$
\hat{x}_k
=
\hat{x}_k^{-}
+
K_k
\left(
z_k
-
H\hat{x}_k^{-}
\right)
$$

---

# Interpretation

Term:

$$
z_k
-
H\hat{x}_k^{-}
$$

is called:

```text
Innovation
```

or

```text
Residual
```

It measures:

```text
How wrong prediction was
```

---

# Compare to Low-Pass Filter

Low-pass filter:

$$
\hat{x}_k
=
\alpha \hat{x}_{k-1}
+
(1-\alpha)z_k
$$

Kalman filter:

$$
\hat{x}_k
=
(1-K_k)
\hat{x}_k^{-}
+
K_k z_k
$$

---

## Important Insight

Kalman Filter is:

```text
Adaptive Low-Pass Filter
```

where:

$$
K_k
$$

changes automatically.

---

# Step 5: Update Uncertainty

$$
P_k
=
(I-K_kH)P_k^{-}
$$

---

# Interpretation

After seeing a measurement:

```text
We know more
```

so uncertainty decreases.

---

# Simple Voltage Example

True voltage:

$$
14.4V
$$

Measurements:

```text
13.8
16.2
11.5
14.8
15.1
```

---

## Model

Voltage assumed constant.

State:

$$
x_k
=
V_k
$$

---

## State Model

Voltage remains same:

$$
x_{k+1}
=
x_k
$$

Thus:

$$
A=1
$$

---

## Measurement Model

Sensor measures voltage directly.

$$
z_k
=
x_k
+
v_k
$$

Thus:

$$
H=1
$$

---

## Matrices

$$
A=1
$$

$$
H=1
$$

$$
Q=0
$$

$$
R=4
$$

---

## Initial Guess

$$
\hat{x}_0
=
14
$$

---

Result:

```text
Kalman filter rapidly converges
toward 14.4 volts
```

while filtering noise.

---

# Position and Velocity Example

Suppose we measure only position.

Want:

```text
Position
Velocity
```

---

# State Vector

$$
x_k
=
\begin{bmatrix}
p_k \\
v_k
\end{bmatrix}
$$

---

# Physics Model

Position update:

$$
p_{k+1}
=
p_k
+
\Delta t\,v_k
$$

Velocity update:

$$
v_{k+1}
=
v_k
$$

---

## State Transition Matrix

$$
A
=
\begin{bmatrix}
1 & \Delta t \\
0 & 1
\end{bmatrix}
$$

---

# Measurement Model

Only position measured.

$$
z_k
=
p_k
$$

Therefore:

$$
H
=
\begin{bmatrix}
1 & 0
\end{bmatrix}
$$

---

# Important Result

Even though we never directly measure velocity:

$$
v_k
$$

is estimated automatically.

---

# Why This Is Powerful

Naive velocity estimate:

$$
v_k
=
\frac{
p_{k+1}
-
p_k
}{
\Delta t
}
$$

---

Problem:

Differentiation amplifies noise.

Result:

```text
Extremely noisy velocity estimate
```

---

Kalman Filter:

```text
Uses model
+
measurement
+
uncertainty
```

Produces much smoother velocity estimates.

---

# Sensor Fusion

One of the most important applications.

---

# Idea

Combine multiple sensors.

Example:

```text
GPS
+
Accelerometer
+
Gyroscope
```

---

Each sensor is imperfect.

Together:

```text
Much better estimate
```

---

# Example: Aircraft Attitude

Gyroscope:

```text
Measures angular velocity
```

---

Accelerometer:

```text
Measures gravity direction
```

---

Kalman Filter combines:

```text
Short-term gyro accuracy
+
Long-term gravity reference
```

---

Result:

```text
Accurate orientation estimate
```

---

# Meaning of the Main Matrices

## A (State Transition Matrix)

Physics model.

Example:

$$
x_{k+1}
=
Ax_k
$$

---

## H (Measurement Matrix)

Maps state to measurement.

Example:

$$
z_k
=
Hx_k
$$

---

## Q (Process Noise)

Uncertainty in model.

Higher:

$$
Q
$$

means:

```text
Trust model less
```

---

## R (Measurement Noise)

Uncertainty in sensor.

Higher:

$$
R
$$

means:

```text
Trust sensor less
```

---

# Tuning Q and R

Most practical Kalman filter work involves tuning:

$$
Q
$$

and

$$
R
$$

---

## Large Q

```text
Model uncertain
```

Filter follows measurements more closely.

---

## Large R

```text
Sensor uncertain
```

Filter follows model more closely.

---

# Python Example (FilterPy)

```python
from filterpy.kalman import KalmanFilter
import numpy as np

kf = KalmanFilter(
    dim_x=2,
    dim_z=1
)

kf.x = np.array([0., 0.])

kf.F = np.array([
    [1, 1],
    [0, 1]
])

kf.H = np.array([
    [1, 0]
])

kf.R *= 5

kf.Q *= 0.1

for z in measurements:

    kf.predict()

    kf.update(z)

    print(kf.x)
```

---

# Relationship to Machine Learning

Kalman Filters are not typically considered ML.

They belong to:

```text
Estimation Theory
Control Theory
Signal Processing
```

However they are widely used with ML systems.

Examples:

```text
Neural Network
+
Kalman Filter

Computer Vision
+
Kalman Filter

Object Tracking
+
Kalman Filter
```

---

# Common Applications

## Aerospace

- Aircraft navigation
- Spacecraft attitude estimation
- GPS fusion

---

## Robotics

- SLAM
- Localization
- Path planning

---

## Autonomous Vehicles

- Sensor fusion
- Tracking vehicles
- GPS correction

---

## Finance

- State-space models
- Trend estimation
- Noise filtering

---

## Computer Vision

- Object tracking
- Motion estimation

---

# Common Mistakes

## Choosing Q and R Arbitrarily

Most common issue.

---

## Incorrect State Model

Bad:

$$
A
$$

gives bad estimates.

---

## Ignoring Units

Position:

```text
meters
```

Velocity:

```text
meters/sec
```

Noise matrices must be consistent.

---

## Expecting Perfect Estimates

Kalman filters estimate uncertainty.

They do not eliminate uncertainty.

---

# Extended Kalman Filter (EKF)

Standard Kalman Filter assumes:

$$
x_{k+1}
=
Ax_k
$$

Linear systems.

---

For nonlinear systems:

$$
x_{k+1}
=
f(x_k)
$$

use:

```text
Extended Kalman Filter (EKF)
```

---

# Unscented Kalman Filter (UKF)

More accurate nonlinear alternative.

Often used for:

- Drones
- Robotics
- Spacecraft

---

# Key Takeaways

- Kalman Filters evolved naturally from recursive averages and low-pass filters.
- A Kalman Filter is essentially an adaptive low-pass filter that automatically chooses how much to trust measurements.
- Every iteration consists of prediction and update.
- The Kalman Gain determines how much the estimate moves toward the measurement.
- The matrices $A$, $H$, $Q$, and $R$ define the system model.
- $Q$ controls trust in the model; $R$ controls trust in the sensor.
- Kalman Filters can estimate unmeasured quantities such as velocity from noisy position measurements.
- Sensor fusion is one of the most powerful uses of Kalman Filters.
- The filter maintains both an estimate and a measure of uncertainty.
- Modern robotics, aerospace systems, navigation systems, and tracking systems rely heavily on Kalman Filters.