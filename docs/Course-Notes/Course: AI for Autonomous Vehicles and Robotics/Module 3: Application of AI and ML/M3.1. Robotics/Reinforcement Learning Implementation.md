# M3.1.3. Reinforcement Learning (Q-Table) Implementation

<blockquote> 
If the LaTex Math notation are not displayed correctly, please reload the site.
</blockquote>

## Problem Setup: GridWorld

- The agent starts in a specific position on a grid and must reach a goal position.
- The grid contains obstacles that the agent must avoid.
- The agent can move in four directions: up, down, left, or right.
- The agent receives rewards:
    - +1 for reaching the goal.
    -1 for hitting an obstacle.
    0 for moving through empty space.

## Explanattion of the Code
* **Grid Definition**: We define a 5 x 5 grid with a start position at the top-left, a goal at the bottom-right, and an obstacle in the center.

<center>
<img src="../../../../../images/image30.png" width=30% height=10% >
</center>

- **Rewards**: The agent receives a reward of +1 for reaching the goal and -1 for hitting the obstacle.
- **Q-values Table**: This is a 3D array that stores the Q-values for each state-action pair. Initially, all Q-values are set to 0.
- **Epsilon-greedy Policy**: The agent chooses actions either by exploring (choosing a random action) or by exploiting (choosing the action with the highest Q-value).
- **Q-value Update Rule**: The Q-values are updated according to the Q-learning formula in each step of the episode.
- **Visualization**:
    - We visualize the learned Q-values using a heatmap.
    - The learned policy is shown with arrows indicating the best action at each step.


## Visualization

The following two outputs are generated:

- **Q-values Heatmap**: The heatmap visualizes the Q-values for each state in the grid, showing how much the agent values each state. The state values are derived by taking the maximum Q-value across all actions for each state. The goal state has the highest value, and the obstacle state has a low value.
- **Learned Policy**: The learned policy shows the best action to take from each state, based on the learned Q-values. The policy will typically show the agent moving towards the goal while avoiding the obstacle.

## Full Python Scripts

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import random
```

```python
# Define the environment
grid_size = 5
goal_state = (4, 4)
obstacle_state = (2, 2)
start_state = (0, 0)
```

```python
# Rewards: +1 for goal, -1 for obstacle, 0 for everything else
rewards = np.zeros((grid_size, grid_size))
rewards[goal_state] = 1
rewards[obstacle_state] = -1
```

```python
# Actions: up, down, left, right
actions = ['up', 'down', 'left', 'right']
action_mapping = {
    'up': (-1, 0),
    'down': (1, 0),
    'left': (0, -1),
    'right': (0, 1)
}
action_arrows = {
    'up': (0, 0.3),
    'down': (0, -0.3),
    'left': (-0.3, 0),
    'right': (0.3, 0)
}
```

```python
# Initialize Q-values table
Q_values = np.zeros((grid_size, grid_size, len(actions)))

```
```python
# Hyperparameters
alpha = 0.1  # Learning rate
gamma = 0.9  # Discount factor
epsilon = 0.1  # Exploration rate
episodes = 500  # Number of episodes
```
```python
# Helper function: get next state based on action
def get_next_state(state, action):
    delta = action_mapping[action]
    next_state = (state[0] + delta[0], state[1] + delta[1])
    
    # Check boundaries (stay within grid)
    next_state = (max(0, min(grid_size - 1, next_state[0])),
                  max(0, min(grid_size - 1, next_state[1])))
    
    return next_state

# Helper function: choose action using epsilon-greedy strategy
def choose_action(state):
    if random.uniform(0, 1) < epsilon:
        # Exploration: choose random action
        return random.choice(actions)
    else:
        # Exploitation: choose action with max Q-value
        state_Q_values = Q_values[state[0], state[1], :]
        return actions[np.argmax(state_Q_values)]
```
```python
# Q-learning algorithm
for episode in range(episodes):
    state = start_state
    
    while state != goal_state:
        action = choose_action(state)
        next_state = get_next_state(state, action)
        reward = rewards[next_state]
        
        # Q-learning update
        current_q_value = Q_values[state[0], state[1], actions.index(action)]
        max_future_q_value = np.max(Q_values[next_state[0], next_state[1], :])
        
        new_q_value = current_q_value + alpha * (reward + gamma * max_future_q_value - current_q_value)
        Q_values[state[0], state[1], actions.index(action)] = new_q_value
        
        # Move to next state
        state = next_state
```
```python
# Q-learning algorithm
# Visualization of learned Q-values
fig, ax = plt.subplots(figsize=(10, 8))

# Sum Q-values for each state to visualize overall state values
state_values = np.max(Q_values, axis=2)

# Plot heatmap of state values
sns.heatmap(state_values, annot=True, cmap='coolwarm', cbar=True, fmt=".2f", ax=ax)

# Highlight the goal and obstacle
ax.add_patch(plt.Rectangle(goal_state, 1, 1, fill=False, edgecolor='green', lw=3))
ax.add_patch(plt.Rectangle(obstacle_state, 1, 1, fill=False, edgecolor='red', lw=3))

# Adding arrows for the learned policy
for i in range(grid_size):
    for j in range(grid_size):
        if (i, j) != goal_state and (i, j) != obstacle_state:
            best_action_idx = np.argmax(Q_values[i, j, :])
            best_action = actions[best_action_idx]
            arrow = action_arrows[best_action]
            ax.arrow(j + 0.5, i + 0.5, arrow[0], -arrow[1], head_width=0.2, head_length=0.2, fc='black', ec='black')

plt.title('Q-values Heatmap with Optimal Policy Arrows')
plt.show()
```

## Results

- **Q-values Heatmap**: The heatmap will show high values near the goal state and low values near the obstacle.
- **Learned Policy**: The policy will guide the agent from the start position to the goal while avoiding the obstacle, with arrows indicating the best action at each step.

<center>
<img src="../../../../../images/image31.png" width=40% height=20% >
</center>

