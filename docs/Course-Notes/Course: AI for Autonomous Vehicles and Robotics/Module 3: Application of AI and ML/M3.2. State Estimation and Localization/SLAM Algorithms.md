# M3.2.3 SLAM Algorithms


<blockquote> 
If the LaTex Math notation are not displayed correctly, please reload the site.
</blockquote>

## 1.  Extended Kalman Filter (EKF) SLAM

EKF SLAM is a traditional approach where the Kalman Filter is used to estimate both the robot's state and the map. The EKF assumes Gaussian noise and linearizes the motion and measurement models using the first-order Taylor expansion (Jacobian).

**EKF SLAM Steps**:

**State Prediction**: Predict the new state $\mathbf{x}_t$ using the motion model

$$
\mathbf{x}_t = \mathbf{F}_t \mathbf{x}_{t-1} + \mathbf{B}_t u_t + \mathbf{w}_t
$$

**Covariance Prediction**: Update the state covariance matrix to reflect the uncertainty in the prediction

$$
\mathbf{P}_t = \mathbf{F}_t \mathbf{P}_{t-1} \mathbf{F}^{\top}_t + \mathbf{Q}_t
$$

**Measurement Update**: When a measurement $z_t$ is received, the state estimate is updated using the Kalman Gain

$$
\mathbf{K}_t = \mathbf{P}_t \mathbf{H}^{\top}_t (\mathbf{H}_t \mathbf{P}_t \mathbf{H}_t^{\top} + \mathbf{R}_t)^{-1}
$$

Update the state estimate:

$$
\mathbf{x}_t = \mathbf{x}_t + \mathbf{K}_t (z_t -\mathbf{H}_t \mathbf{x}_t)
$$

Update the covariance matrix

$$
\mathbf{P}_t = (\mathbf{I} - \mathbf{K}_t \mathbf{H}_t) \mathbf{P}_t
$$

| Component | Description |
| ---------- | --------------- |
| $x_t$ | The predicted state vector at time $t$, representing the robot's position and orientation, as well as map feature locations. In the case of a @D SLAM problem, the state might include the robot's position $(x_t, y_t, \theta _t)$ and the map features (landmarks) $(f_1, f_2,..., f_n)$. |
| $\mathbf{F}_t$ | The state transition matrix, which models show the robots state evolves over time absed on its previous state and control inputs. It typically includes identity matrices for static parts of the state (e.g., landmarks) and changes in position/orientation based on control inputs. |
| $\mathbf{B}_t$ | The conrol input matrix, which relates the control inputs to the changes in state. This matrix maps the control inputs (e.g. velocity and steering angle) to their efefcts on the robot's state.|
| $u_t$ | The control input vector, which contains the control inputs applied to the rbot, such as velocity $v_t$ adn steering angle $\omega _t$. |
| $\mathbf{w}_t$ | The process noise, which accounts for uncertainties in the robot's motion model (e.g., due to wheel slippage or imperfect control execution). It is usually modeled as zero-mean Gaussian noise with covariance $\mathbf{Q}_t$. |
| $\mathbf{Q}_t$ | The process noise covariance matrix, which representsthe uncertainty in the motion model. It defines how much uncertainty we expect in the predicted state due to the robot's movement. |
| $\mathbf{Q}_t$ | The process noise covariance matrix, which represents the uncertainty in the motion moddel. It defines how much uncertainty we expect in the predicted state due to the robot's movement. |
| $\mathbf{Q}_t$ | The process noise covariance matrix, which represents the uncertainty in the motion model. It defines how much uncertainty we expect in the predicted state due to the robot's movement. | 
| $\mathbf{P}_t$ | The predicted covariance matrix at time $t$. This matrix represents the uncertainty of the robot's pose and the map features after the prediction step. It includes both the robot's pose uncertainty and the uncertainty in the positions of the landmarks. |
| $\mathbf{P}_{t-1}$ | The previous covariance matrix at time $t-1$, representing the uncertainty in the state at the previous time step. |
| $\mathbf{F}_t^{\top}$ | The transpose of the state transition matrix, used to propagate the covariance forward in time.|
| $\mathbf{K}_t$ | The Kalman Gain matrix which determines how much we should trust themeasurement compared to the predicted state. It weights the importance of the new measurement relative to the uncertainty in the prediction. |
| $\mathbf{H}_t$ | The observation matrix, which maps the predicted state to the measurement space. It defines how the measureents (e.g., the observed positions of landmarks) are related to the state. |
| $\mathbf{R}_t$ | The measurement noise covariance matrix, representing the uncertainty in the sensor measurements. |
| $z_t$ | The measurement vector, which contains the actual sensor measurements received at time $t$ (e.g., the observed positions of landmarks relative to the robot).|
| $(z_t - \mathbf{H}_t \mathbf{x}_t)$ | The innovation (measurement residual), which represents the difference between the actual measuremen and the predicted measurement based on the current state estimate. This difference is used to corect the state estimate. |
| $\mathbf{I}$ | The identity matrix used to scale the covariance matrix update. |
| $\mathbf{P}_t$| The updated covariance matrix after incorporating the new measurement. It reflecs the reduced uncertainty in the state after the update step.|

## 2. Particle Filter (FastSLAM)

**FastSLAM** uses a particle filter to represent multiple possible robot trajectories (poses), adn each particle maintains its own map of landmarks. This approach can handle nonlinear systems and multi-modal distributions better than EKF.

**FastSLAM Steps**:

1. **Prediction**: Each particle represents a possible robot pose, and its position is updated based on the motion model.
2. **Correction**: Each particle updates its map using the measurement model, associating landmarks with the observed features.
3. **Resampling**: After each update, particles are resampled based on their importance weight (how ell they explain the observations). Particles that better explain the data survive, while others are discarded.

## 3. Graph-Based SLAM

Graph-Based SLAM constructs a graph where:

- **Nodes**: Represent robot poses and landmark positions
- **Edges**: Represent constraints between nodes, based on sensor measurement or odometry

The SLAM problem is solved as a graph optimization problem. The goal is to find the configuration of robot poses and landmarks that minimizes the error between predited and actual measurements.

**Graph-Based SLAM Steps**:

1. **Graph Construction**: Create a graph where each node corresponds to a robot pose or a landmark, and edges represent measurements (constraints).
2. **Error Minimization**: Solve the graph optimization problem to findthe best configuration of poses and landmarks that minimizes the measurement error.

Graph-Based SLAM is widely used in modern applications because it can represent large-scale environments and efficiently optimize the robot's trajectory over long periods.

## 4. Dense SLAM (e.g., ORB-SLAM, LSD-SLAM)

In **dense SLAM**, the goal is to reconstruct a 3D map of the environment, often using visual data (e.g., RGB-D or stereo cameras). Unlike feature-based methods, dense SLAM tries to recover the entire geometry of the environment.

- **ORB-SLAM**: A widely used monocular and stereo visual SLAM system that uses ORB (Oriented FAST and Rotated BRIEF) features for tracking, mapping, and loop closure.
- **LSD-SLAM**:  A direct monocular SLAM approach that builds semi-dense 3D maps by directly minimizing pixel intensities without relying on feature extraction.

## 3. Key Concepts in SLAM

### 3.1. Data Association

Data association is the process of determining which measurements correspond to which landmarks or features in the map. Correctly associating measurements is crucial for building an accurate map and avoiding errors.

### 3.2. Loop Closure

Loop closure refers to the detection of when the robot revisits a previously explored location. Detecting loop closures helps correct accumulated errors in the robot’s trajectory and improves the map's accuracy.

### 3.3. Map Optimization

Map optimization refers to the process of adjusting the map and robot trajectory to minimize the error between predicted and observed measurements. This is crucial in graph-based SLAM approaches, where optimization methods (e.g., Gauss-Newton or Levenberg-Marquardt) are used to solve the graph.

## 4. SLAM Applications

SLAM has a wide range of applications across various domains, including

### 4.1. Autonomous Vehicles

Self-driving cars use SLAM to understand their surroundings, detect obstacles, and navigate safely in real-time. LiDAR-based SLAM is commonly used for precise mapping, while visual SLAM provides a cheaper alternative using cameras.

### 4.2. Robotics

SLAM is essential for robots operating in unknown or dynamic environments, such as drones, robotic vacuums, or industrial robots. It enables robots to explore, map, and localize themselves within environments without relying on external GPS signals.

### 4.3. Augmented and Virtual Reality (AR/VR)

SLAM is used in AR/VR systems to track the user’s movements in real-time and provide a realistic overlay of virtual elements on the real world. For example, AR applications on smartphones (e.g., Apple’s ARKit and Google’s ARCore) use visual SLAM to understand the environment.

### 4.4. Mapping and Surveying

SLAM systems are used in mapping and surveying applications, such as creating 3D maps of buildings, tunnels, or archaeological sites. SLAM enables mobile mapping systems to capture detailed maps of environments without needing GPS.

### 4.5. Exploration

SLAM is crucial for exploration tasks where GPS is unavailable, such as underwater, underground, or space exploration. Robots and autonomous systems use SLAM to navigate and map these unknown environments.

SLAM is a fundamental technology for any autonomous system that needs to operate in an unknown or dynamic environment. It enables simultaneous estimation of both the agent's position and the environment's structure, making it a critical component in robotics, autonomous vehicles, and augmented reality.

The field of SLAM has advanced significantly over the years, with approaches ranging from traditional Extended Kalman Filters (EKF) and Particle Filters (FastSLAM) to modern graph-based and dense visual SLAM techniques. Each SLAM method has its strengths and trade-offs, making it suitable for specific applications depending on the system's constraints and requirements.
