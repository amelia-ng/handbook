# M3.2.2. Introduction to SLAM

<blockquote> 
If the LaTex Math notation are not displayed correctly, please reload the site.
</blockquote>

## 1. Overview

**Simultaneous Localization and Mapping (SLAM)** is a computational problem where a robot (or any agent) attempts to construct a mmap of an unknown environment while simultaneously tracking its own position within that map. SLAM is an essential concept in robotics, autonomous vehicles, augmented reality (AR), and virtual reality (VR) systems.

The problem lies at the intersection of localization (knowing where the agent is) and mapping (understanding the environment). These tasks are interdependent: the agent must know where it is to build a map, and it must build a map to know where it is. This circular dependency makes SLAM a challenging but crucial problem for autonomous systems.

## 2. Why SLAM is Important

SLAM enables autonomous systems to navigate environments without pre-existing maps, which is crucial for:

1. **Autonomous vehicles**: Self-driving cars must navigate unknown roads
2. **Robotics**: Drones, robots, and industrial machines need to operate in dynamic and unknown environments.
3. **Augmented and Virtual Reality**: AR/VR systems need real-time understanding of their surroundings for proper user interaction.
4. **Exploration**: SLAM is used in planetary exploration, underwater navigation, and other applications where GPS is unavailable.

## 3. The Core SLAM Problem

The goal of SLAM is to estimate the following:

1. **The Robot's Pose**: The robot's position $x_t$, $y_t$ and orientation $\theta _ t$ in the environment at time $t$.
2. **The Map**: A representation, $m$, of the environment, typically consisting of landmarks, obstacles, or other relevant features.

SLAM can be mathematically described as:

1. **Input**: Sensor measurements and control inputs
2. **Output**: A map of the environment and an estimate of the robot's position within that map.

The two main components of the SLAM problem are

1. **Localization**: Estimating the robot's position and orientation.
2. **Mapping**: Creating or updating a map of the environment based on sensor data.

## 4. Challenges in SLAM

- **Uncertainty**: Measurements from sensors (e.g., cameras, lasers, radar) are noisy and prone to error, and the robot's motion is uncertain.
- **Data Association**: Identifying which measurements correspond to ưhich features in the map map(e.g, recognizing the same landmark multiple times).
- **Non-linearity**: The robot's motion and sensor models are often non-linear.
- **Loop Closure**: Detecting when the robot returns to a previously visited place (which is essential for correcting drift in localization).
- **Real-Time Constraints**: SLAM must run efficiently in real-time on limited computational resources, particularly in robotics and autonomous vehicles.

## 5. Types of SLAM

There are several approaches to SLAM, depending on the sensors used, the type of map representation, and the algorithmsd used for solving the problem.

### 5.1. Based on Sensors

- **Visual SLAM (vSLAM)**: Uses cameras as the promary sensor to track visual features and build a map. It can be monocular (single camera), stereo (two cameras), or RGB-D (camera with depth information).
- **LiDAR SLAM**: Uses LiDAR (Light DEtection and Ranging) sensors, which provide accurate 3D distance meeasurements to the surrounding environment.
- **RGB-D SLAM**: Uses an RGB camera and a depth sensor to capture both visual and depth information simultaneously.
- **Multi-Sensor SLAM**: Combines multiple sensors (e.g., camera, LiDAR, IMU, GPS) for more robust mapping and localization.

### 5.2. Based on Map Representation

- **Feature-based SLAM**: The environment is represented by a sparse set of landmarks (features), such as points of corners. These features are tracked over time to build the map.
- **Grid-based SLAM**: The environment is represented as an occupancy gridwhere each cell holds the probability of being occupied. Grid-based maps are typically used with LiDAR sensor.
- **Dense SLAM**: Creates a dense, continuous map of the environment, capturing all surfaces and textures. This is typically used in applications such as visual SLAM, AR/VR, and high-fideliy mapping.

### 5.3. Based on Algorithmic Approaches

- **Extended Kalman Filter (EKF) SLAM**: One of the first and most widely used approaches. It uses a Gaussian distribution to represent uncertainty and estimates both the robot's state and map features using a Kalman Filter.
- **Particle Filter (FastSLAM)**: Uses a set of particles to represent different hypotheses of the robot's pose and map, and is better suited for handling non-linearities and multi-modal distributions.
- **Graph-based SLAM**: Models SLAM as a graph optimization problem, where nodes represent robot poses and landmarks, and edges represent measurements or constraints between them.
- **Pose Graph Optimization (PGO)**: Focuses on optimizing the trajectory of the robot by adjusting poses in the graph while keeping landmarks fixed or minimal.

## 6. Mathematical Framework of SLAM

### 6.1. State Representation

At time $t$, the state $x_t$ of the SLAM system includes

**The robot's pose**: $\mathbf{x}_t = [x_t, y_t, \theta _t ]$ where $x_t$, $y_t$ are the robot's position coordinates, and $\theta _t$ is its orientation.

**The map** $m$: The map can include features ${f_1, f_2, ..., f_n}$ or grid cells representing obstacles or free space.

The full SLAM state combines both the robot's pose and the map $m$:

$$
\mathbf{s}_t = [\mathbf{x}_t, m]
$$

### 6.2. Bayesian SLAM Framework

SLAM is often framed as a probabilitistic inference problem

**Goal**: Compute the posterior distribution over the robot's trajectory and the map given the sensor measurements and control inputs.

$$
p(\mathbf{x}_{0:t}, m | z_{1:t}, u_{1:t})
$$

| Component | Description |
| --------- | ---------------|
| $\mathbf{x}_{0:t}$ | The robot's trajectory up to time $t$ |
| $m$ | The map of the environment | 
| $z_{1:t}$ | All sensor measurements up to time $t$ |
|$u_{1:t}$ | All control inputs (e.g., odometry) up to time $t$ |

### 6.3. The SLAM Problem Breakdown

The SLAM problem can be broken down into two key recursive steps

**Prediction (Motion Model)**: Given the prev9ious state $\mathbf{x}_{t-1}$ and control input $u_t$, predict the new state $\mathbf{x}_t$:

$$
\mathbf{x}_t = f(\mathbf_{t-1}, u_t) + \mathbf{w}_t
$$

where $f$ is the motion model, and $\mathbf{w}_t$ represents the process noise.

**Update (Measurement Model)**: Update the belief about the robot'sstate and map based on the sensor measurement $z_t$:

$$
z_t - h(\mathbf{x}_t, m) + v_t
$$

where $h$ is the observation model, and $v_t$ represents the measurement noise.

These steps are iterated recursively as the robot moves through the environment.

### 6.4. Map Representation

In SLAM, the map can be representede in several ways depending on the type of algorithm and application:

1. **Landmark-Based Representation**: Each feature or landmark is represented by its coordinates (e.g., point features such as corners or walls). The robot observes these feaures and refines the position estimate based on their relative locations.
2. **Occupancy Grid Map**: The environment is divided into grid cells, and each cell holds the probability of being occupied or free. This approach is commonly used in LiDAR SLAM systems.

