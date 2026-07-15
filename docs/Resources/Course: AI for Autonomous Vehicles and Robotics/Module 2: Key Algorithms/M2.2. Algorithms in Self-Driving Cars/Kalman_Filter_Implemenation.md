# Kalman Filters in State Estimation Implementation

## Problem Setup

**State:** We want to estimate the object's position and velocity.

$$
\mathbf{x}_k =
\begin{bmatrix}
\text{position}_k \\
\text{velocity}_k
\end{bmatrix}
$$

**Measurement:** We only measure the object's noisy position.

**State Transition Model:** Assuming the object moves with constant velocity, the state transition model is:

$$
\mathbf{x}_k = \mathbf{F}_k \mathbf{x}_{k-1} + \mathbf{w}_{k-1}
$$

where $\mathbf{F}_k$ is the state transition matrix and $\mathbf{w}_{k-1}$ is the process noise.

**Measurement Model:** We only observe the position, so the observation matrix $\mathbf{H}_k$ is:

$$
\mathbf{z}_k = \mathbf{H}_k \mathbf{x}_k + \mathbf{v}_k
$$

where $\mathbf{v}_k$ is the measurement noise.



```python
import sys
!{sys.executable} -m pip install numpy matplotlib
```

```python
import numpy as np
import matplotlib.pyplot as plt
```


```python
# Simulate motion with constant velocity and noisy position measurements

# Time step
dt = 1.0

# State transition matrix (constant velocity model)
F = np.array([[1, dt],
              [0, 1]])

# Observation matrix (we only measure the position)
H = np.array([[1, 0]])

# Process noise covariance matrix (uncertainty in model prediction)
Q = np.array([[1, 0],
              [0, 1]])

# Measurement noise covariance matrix (uncertainty in measurement)
R = np.array([[10]])

# Initial state estimate (position and velocity)
x = np.array([[0],  # Initial position
              [1]])  # Initial velocity

# Initial estimate covariance matrix
P = np.array([[500, 0], 
              [0, 500]])

# Number of iterations (time steps)
n = 50

# Simulated true positions and noisy measurements
true_positions = []
measurements = []
velocities = []
position = 0
velocity = 1
for i in range(n):
    # True position and velocity (without noise)
    position += velocity * dt
    true_positions.append(position)
    
    # Noisy measurement (position)
    noisy_measurement = position + np.random.normal(0, np.sqrt(R[0, 0]))
    measurements.append(noisy_measurement)

# Kalman filter process
estimates = []  # Store position estimates
for i in range(n):
    # Prediction Step
    x = np.dot(F, x)  # State prediction
    P = np.dot(np.dot(F, P), F.T) + Q  # Covariance prediction
    
    # Measurement update step (if we have a measurement)
    z = np.array([[measurements[i]]])  # Current measurement (noisy position)
    y = z - np.dot(H, x)  # Innovation (residual)
    S = np.dot(H, np.dot(P, H.T)) + R  # Innovation covariance
    K = np.dot(np.dot(P, H.T), np.linalg.inv(S))  # Kalman gain
    
    x = x + np.dot(K, y)  # Updated state estimate
    P = np.dot(np.eye(2) - np.dot(K, H), P)  # Updated covariance estimate
    
    # Store the estimate
    estimates.append(x[0, 0])

# Plot results
plt.figure(figsize=(10, 6))
plt.plot(true_positions, label='True Position')
plt.plot(measurements, 'ro', label='Noisy Measurements')
plt.plot(estimates, 'b-', label='Kalman Filter Estimate')
plt.legend()
plt.xlabel('Time Step')
plt.ylabel('Position')
plt.title('Kalman Filter for Position Estimation')
plt.show()
```


    
![png](Kalman_Filter_Implemenation_files/Kalman_Filter_Implemenation_3_0.png)
    

