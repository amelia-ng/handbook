# M2.1.1. Algorithms in Robotics

<blockquote> 
If the LaTex Math notation are not displayed correctly, please reload the site.
</blockquote>

​Robotic systems have advanced significantly, ​thanks to the development and ​application of advanced algorithms. ​These algorithms enable robots to ​perform complex tasks on their own, ​from navigating uncertain environments ​to manipulating objects with precision. ​To understand how robots work across various industries, ​it's important to graph these core algorithms. ​

The five key areas of algorithms in robotics ​include:

- Navigation and path planning
- Robot manipulation
- Localization and mapping
- Machine learning algorithms
- Swarm robotics algorithms. 


## 1. Navigation and Path Planning

### 1.1. SLAM

- ​Navigation is a fundamental challenge in robotics, ​especially in dynamic or unknown environments. **​SLAM**, which stands for ​simultaneous localization and mapping, ​is essential for autonomous navigation. 

<center>
<img src="../../../../../images/image10.png" width=30%><figcaption><em>Schematic of robot path planning.</em></figcaption>
</center>

- SLAM allows robots to create a map of ​their surroundings while keeping ​track of their locations. 

<center>
<img src="../../../../../images/image11.png" width=30% height=10%>
</center>

- ​SLAM combines sensor data from sources (LiDAR, cameras) to build a detailed map ​that guides the robot's movement. 

<center>
<img src="../../../../../images/image12.png" width=30% height=10%>
</center>

### 1.2 Path Planning
- ​For **path planning**, ​A\* and Dijkstra algorithms are widely used. 
    - ​A\* is a heuristic-based search algorithm that ​efficiently finds ​the shortest path by balancing movement, ​cost, and distance to the goal. ​
    - Dijkstra's algorithm explores all possible paths ​to determine the shortest one, ​which is especially useful in fully mapped environments. 
- ​In dynamic settings, the D\* lite extends ​A\* by updating paths as the environment changes. ​This makes it ideal for real time navigation. 
    - D\* Lite used in dynamics; while RRT, amd PRM algorithms used in high-dimensional spaces for real-time and collision-free path planning
    - RRT: Rapid-exploring Random Trees
    - PRM: Probabilistic Roadmaps

## 2. Robot Manipulation

- Focus on the precise control of robotic arms and grippers for object manipulation.
- Advanced algorithms optimize movements for accuracy, efficiency, and adaptability in handling a wide range of tasks.

### 2.1. Inverse Kinematic

- Inverse Kinematic (IK): Calculate joint angles to position a robot's end-effector at a desired location and orientation. This algorithm is important for tasks ​such as welding, assembly, and painting. 

<center>
<img src="../../../../../images/image13.png" width=50% height=30%>
</center>

### 2.2. Trajectory Planning

- Trajctory Planning ensures smooth, accurate movements by calculating the best path for the robot's end-effector, considering:
    - Velocity
    - Acceleration
    - Obstacles

For example, subline-based trajectory planning ​ensures continuous and smooth movements, ​which are critical for delicate manipulation tasks. ​Here, X points represent the trajectory path. ​The orange line illustrates ​the curve without interpolation. ​While the blue line shows ​the smooth curve generated using spline interpolation.  

<center>
<img src="../../../../../images/image14.png" width=50% height=30%>
</center>

### 2.3. Grasp Planning

- Use sensor data and ML to determine the best way for a robot to grasp an object based on shape, size, material
- This adaptability is essential when handling ​objects that come in ​different shapes or that might be fragile
- Allow robots to adjust their grip dynamically
- This example of grasp planning involves two main stages. ​Grasp skill recognition and grasp synthesis. ​First, the system recognizes where the hand makes ​contact and the direction it ​approaches based on a human demonstration. ​Next, grasp optimization is used. ​This means the system adjusts the gripper ​so its fingers feed closely to the object's surface, ​matching the contact region from the human demonstration.

<center>
<img src="../../../../../images/image15.png" width=50% height=30%>
</center>

​Once the grasp is created, ​it's checked to make sure there are no collisions. ​If a collision is found, ​the gripper is moved to a safe position, ​and the optimization process starts ​again to create a new safe grasp. 

​In this process, the hand plane is formed by ​fitting hand joints on the thumb finger and index finger. 

<center>
<img src="../../../../../images/image16.png" width=30% height=10%>
</center>

## 3. Localization and Mapping

​Localization is about determining ​a robot's position within a known environment. ​While SLAM is used for unknown environments, ​common filters and particle filters ​are commonly used in known settings. 

### 3.1. Kalman and Particle Filters

- ​The Kalman filter gives ​accurate position estimates by ​filtering out sensor noise. 
- The particle filter takes a probabilistic approach, ​which is useful in ​non-linear and non-Gaussian environments.

### 3.2. Occupancy grid mapping

- ​For mapping, occupancy grid mapping ​represents the environment as a grid of cells, ​with each cell indicating ​the likelihood of being occupied by an obstacle. 
- ​This method is essential for ​navigation and planning in complex environments, ​whether indoors or outdoors. 
- Here is an example of occupancy grid mapping. ​In the diagram on the right, ​the occupied grid cells are shown in black. ​Three cells are marked in white. ​The number of pawn objects corresponding to ​each real world object equals the number of ​grid cells occupied by the real world object. 

<center>
<img src="../../../../../images/image17.png" width=50% height=30%>
</center>

## 4. Machine Learning

- ​Machine learning is increasingly improving robotics, ​particularly in perception, ​decision making, and adaptability. 

​The commonly used algorithms include: 

- **Reinforcement learning**
    - Where robots learn to perform tasks by interacting with ​their environment and optimizing ​their actions based on rewards. 
    - Effective in ​dynamic settings where robots need ​to adapt to complex and changing behaviors.
- **Convolutional neural network**
    - Deep learning has transformed robotic vision, making it possible for robots to perform tasks such as object detection, scene understanding, and autonomous navigation.
    - ​Convolutional neural networks help process visual data, ​allowing robots to recognize ​objects and interpret their surroundings. 
- **Imitation learning**
    - Allows robots to ​learn tasks by observing human demonstrations, ​which simplifies the programming of complex tasks. 
    - ​This is particularly beneficial for ​service robots where human-like behavior is desirable. 
- **Unsupervised learning** and **Anomaly detection**: used in situations ​where robots operate in ​uncertain or unpredictable environments. 

​These algorithms enable robots to ​identify patterns or detect neural events, ​enhancing their ability to operate ​autonomously and safely in diverse situations. 

## 5. Swarm Robotics Algorithms

- **Consensus algorithms** 
    - Ensure that ​multiple robots in a swarm agree on ​parameters such as direction or task allocation. 
    - ​This allows them to work together efficiently.
- **​Boids algorithms** (​inspired by the flocking behavior in birds) 
    - Manage the collective movement of robots in a swarm. 
    - ​They help achieve coordinate actions ​without requiring centralized control.
