# Kalman Filters

## 1. Overview

- The **Kalman Filter** is a powerful mathematical tool used to estimate the state of adynamic system from a series of incomplete and noisy measurement.
- It provides an efficient computational (recursive)  means to estimate the state of a process in a way that minimizes the mean of the squared error.
- The filter is named after Rudolf E. Kálmán, one of the primar developers of its theory.

## 2. Core Concepts

### 2.1. State-Space Representation

The Kalman Filter estimates the internal state o a process described by a **linear stochastic difference equation**. The system is modeled in discrete time with the following equations

- **State Equation (Process Model):**

$$
\mathbf{x}_k = \mathbf{F}_k \mathbf{x}_{k-1} + \mathbf{B}_k \mathbf{u}_k + \mathbf{w}_{k-1}
$$

- **Measurement Equation (Observation Model):**

$$
\mathbf{z}_k
=
\mathbf{H}_k \mathbf{x}_k
+
\mathbf{v}_k
$$

| Component | Description |
|-----------|-------------|
| $\mathbf{x}_k$ | State vector at time step $k$ (e.g., position and velocity). |
| $\mathbf{F}_k$ | State transition matrix applying the effect of each state parameter at time $k-1$ on the state at time $k$. |
| $\mathbf{B}_k$ | Control input matrix applying the effect of each control input parameter. |
| $\mathbf{u}_k$ | Control input vector (e.g., acceleration commands). |
| $\mathbf{w}_{k-1}$ | Process noise vector (assumed to be Gaussian with zero mean and covariance $\mathbf{Q}_{k-1}$). |
| $\mathbf{z}_k$ | Measurement vector at time $k$. |
| $\mathbf{H}_k$ | Observation matrix mapping the true state space into the observed space. |
| $\mathbf{v}_k$ | Measurement noise vector (assumed to be Gaussian with zero mean and covariance $\mathbf{R}_k$). |

**Assumptions**

- **Linearity**: The system dynamics and measurement equations are linear
- **Gaussian Noise**: Both process noise $\mathbf{w}_{k-1}$ and measurement noise $\mathbf{v}_k$ are white (uncorrelated), Gaussian, and have zero mean.
- **Initial Conditions**: Initial state $\mathbf{x}_0$ and covariance $\mathbf{P}_0$ are known.

### 2.2. The Kalman Filter Algorithm
The algorithm operates recursively in 2 steps:
- Prediction Step (Time Update)
- Update Step (Measurement Update)

#### a. Prediction Step
- **Predicted State Estimate**:

$$
\hat{\mathbf{x}}_{k \mid k-1}
=
\mathbf{F}_k
\hat{\mathbf{x}}_{k-1 \mid k-1}
+
\mathbf{B}_k
\mathbf{u}_k
$$

The predicted state estimate $\hat{\mathbf{x}}_{k-1 \mid k-1}$ is calculated using the state transition matrix $\mathbf{F}_k$ and the control input $\mathbf{u}_k$

- **Predicted Estimate Covariance**

$$
\mathbf{P}_{k \mid k-1}
=
\mathbf{F}_k
\mathbf{P}_{k-1 \mid k-1}
\mathbf{F}_k^{\top}
+
\mathbf{Q}_{k-1}
$$

The predicted estimate covariance $\mathbf{P}_{k-1 \mid k-1}$ incorporates the uncertainty from both the previous estimate covariance and the process noise covariance $\mathbf{Q}_{k-1}$.

| Component | Description |
|-----------|-------------|
| $\hat{\mathbf{x}}_{k \mid k-1}$ | Predicted state estimate at time $k$ given observations up to time $k-1$. |
| $\hat{\mathbf{x}}_{k-1 \mid k-1}$ | Updated state estimate at time $k-1$. |
| $\mathbf{P}_{k \mid k-1}$ | Predicted estimate covariance at time $k$. |
| $\mathbf{P}_{k-1 \mid k-1}$ | Updated estimate covariance at time $k-1$. |
| $\mathbf{Q}_{k-1}$ | Process noise covariance matrix. |

#### b. Update Step (Correction Step)

**Innovation (Residual) Calculation**

$$
\mathbf{y}_k
=
\mathbf{z}_k
-
\mathbf{H}_k
\hat{\mathbf{x}}_{k \mid k-1}
$$

The innovation $\mathbf{y}_k$ represents the discrepancy between the actual measurement $\mathbf{Z}_k$; and the predicted measurement $\hat{\mathbf{x}}_{k \mid k-1}$

**Innovation Covariance**

$$
\mathbf{S}_k
=
\mathbf{H}_k
\mathbf{P}_{k \mid k-1}
\mathbf{H}_k^{\top}
+
\mathbf{R}_k
$$

The innovation coveriance $\mathbf{S}_k$ reflects the expected uncertainty in the innovation, combining both the predicted estimate covariance and the measurement noise covariance.

**Kalman Gain**

$$
\mathbf{K}_k
=
\mathbf{P}_{k \mid k-1}
\mathbf{H}_k^{\top}
\mathbf{S}_k^{-1}
$$

The Kalman Gain $\mathbf{K}k$ determines the weighting of the innovation in updating the state estimate. It balances the uncertainty between the predicted estimate and the measurement.

**Updated State Estimate**

$$
\hat{\mathbf{x}}_{k \mid k}
=
\hat{\mathbf{x}}_{k \mid k-1}
+
\mathbf{K}_k
\mathbf{y}_k
$$

The updated state estimate $\hat{\mathbf{x}}_{k \mid k-1}$ is obtained by adjusting the predicted state estimate with the weighted innovation.

**Updated Estimate Covariance**

$$
\mathbf{P}_{k \mid k}
=
\left(
\mathbf{I}
-
\mathbf{K}_k
\mathbf{H}_k
\right)
\mathbf{P}_{k \mid k-1} 
$$

The updated estimate covariance $\mathbf{P}_{k \mid k}$ refects the reduced uncertainty after incorporating the measurement.

| Component | Description |
|-----------|-------------|
| $\mathbf{y}_k$ | Innovation (measurement residual) at time $k$. |
| $\mathbf{S}_k$ | Innovation covariance matrix. |
| $\mathbf{K}_k$ | Kalman gain matrix. |
| $\hat{\mathbf{x}}_{k \mid k}$ | Updated state estimate at time $k$. |
| $\mathbf{P}_{k \mid k}$ | Updated estimate covariance matrix at time $k$. |
| $\mathbf{I}$ | Identity matrix. |
| $\mathbf{R}_k$ | Measurement noise covariance matrix. |

## 3. Mathematical Derivation of the Kalman Gain

The Kalman Gain $\mathbf{K}_k$ is derived by minimizing the posterior estimate covariance $\mathbf{P}_{k \mid k}$, leading to the optimal gain that minimizes the mean squared error.

**Objective:** Minimize $\mathbf{P}_{k \mid k}$ with respect to $\mathbf{K}_k$:

$$
\mathbf{P}_{k \mid k}
=
(\mathbf{I}-\mathbf{K}_k\mathbf{H}_k)
\mathbf{P}_{k \mid k-1}
(\mathbf{I}-\mathbf{K}_k\mathbf{H}_k)^{\top}
+
\mathbf{K}_k
\mathbf{R}_k
\mathbf{K}_k^{\top}
$$

**Solution:** Setting the derivative of $\mathbf{P}_{k \mid k}$ with respect to $\mathbf{K}_k$ to zero yields

$$
\mathbf{K}_k
=
\mathbf{P}_{k \mid k-1}
\mathbf{H}_k^{\top}
\left(
\mathbf{H}_k
\mathbf{P}_{k \mid k-1}
\mathbf{H}_k^{\top}
+
\mathbf{R}_k
\right)^{-1}
$$

## 4. Applications of the Kalman Filters

- **Navigation and Tracking**: GPS systems, aircraft, and spacecraft navigation.
- **Control Systems**: State estimation in control loops.
- **Signal Processing**: Filtering noise from sensor data.
- **Economics**: Estimating economic indicators.
- **Robotics**: Localization and mapping (SLAM).