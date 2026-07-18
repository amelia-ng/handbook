# M3.1.2. Reinforcement Learning

<blockquote> 
If the LaTex Math notation are not displayed correctly, please reload the site.
</blockquote>

Reinforcement Learning (RL) is a subfield of Machine Learning that focuses on how agents should tak actions in an environment to maximize cumulative reward. Unlike supervised learning, where the learning process is guided by a labeled dataset, reinforcement learning relies on interactions with the environment to learn optimal behaviors through trial and error.

## I. Core Concepts

1. **Agent and Environment**: The *agent* is the learner or decision-maker, while the *environment* represents everything the agent interacts with. At each time step, the agent observes a state from the environment, takes an action, and receives a reward.
2. **State** ($S$): A representation of the current situation the agent is in. It contains all the information necessary to make a decision.
3. **Action** ($A$): The set of all possible moves the agent can make in a give state.
4. **Reward** ($R$): A scalar feedback signal indicating the immediate benefit of an action taken in a particular state.
5. **Policy** ($\pi$): A strategy or mapping from states to actions. The policy dictates the agent's behavior.
6. **Value Function** ($V$ or $Q$): Estimates of how good it is for the agent to be in a particular state ($V(s)$ in state $s$, $s \in S$), to perform a certain action in a state ($Q(s,a)$ in state $s$ taking action $a$, $s \in S$, $a \in A$) in terms of expected cumulative reward.
7. **Model of the Environment**: Some algorithms use a model that predicts the next state and reward, allowing planning by simulating future scenarios.

## II. The Reinforcement Learning Problem

The goal in RL is to find a policy that maximizes the expected cumulative reward over time. This involves solving the exploration-exploitation trade-off:

- **Exploration**: Trying new actions to discover their effects.
- **Exploitation**: Leveraging known information to maximize reward.

## III. Mathematical Framework

Reinforcement learning problems are often formaluzed using *Markov Decision Processes (MDPs)*, characterized by:

- A set of states $S$
- A set of actions $A$
- A transition function $T$ defining the probability of moving from one state to another, given an action
- A reward function $R$
- A discount factory $\gamma \in [0, 1]$ that reduces future rewards' value, ensuring the sum of total rewards converges.

## IV. Key Algorithms

### 1. Model-Free Methods

Do not build an explicit model of the environment.

#### 1.1. Value-Based Methods

Focus on estimating the value functions.

**Q-Learning**: Learns the optimal action-value function $Q(s, a)$ using the Bellman equation.

$$
Q(s, a) \leftarrow Q(s, a) + \alpha [r + \gamma max_{\alpha'} Q(s', a') - Q(s,a)]
$$


| Component | Description |
|----------|-------------|
| $Q(s, a)$ | Current estimate of the action-value for state $s$ and action $a$ |
| $\alpha$ | Learning Rate (step size between 0 and 1) |
| $r$ | Immediate reward received after taking action $a$ in state $s$|
| $\gamma$ |Discount factor for future rewards|
| $s'$ | Next state after taking action $a$ |
| $max_{\alpha'} Q(s', a')$ | Maximum estimated action-value for all possible actions $a'$ in state $s'$ |

**SARSA (State-Action-Reward-State-Action)**: Similar to Q-Learning but updates the action-value function using the action actually taken.

$$
Q(s, a) \leftarrow Q(s, a) + \alpha[r + \gamma Q(s', a') - Q(s, a)]
$$

| Component | Description |
|-----------| ----------------|
|$a'$| Action taken in the next state $s'$ following the current policy|

#### 1.2. Policy-Based Methods

Directly parameterize the policy and optimize it

**REINFORCE Algorithm**: Uses gradient ascento update policy parameters $\theta$

$$
\theta \leftarrow \theta + \alpha \nabla _ \theta \log \pi _ \theta(a|s) G_t
$$

| Component | Description |
|-----------| ----------------|
| $\theta$ | Paramaters of the policy function \pi _ \theta|
| $alpha$ | Learning rate |
| $\nabla _ \theta$ | Gradiet with respect to $\theta$ |
| $\pi _ \theta (a \| s)$ | Natural logarithm of the policy probability |
| $G_t$ | Return (total discounted future rewards) from time step $t$, calculated as $G_t = \Sigma ^{\infty} _ {k=0} {\gamma ^ k r_{t+k+1}}$ | 


#### 1.3. Actor-Critic Methods
Combine value-based and policy-based approaches

* **Actor**: Updates the policy parameters in the direction suggested by the critics
* **Critic**: Estimates value functions to critique the actions made by the acto

### 2. Model-Based Approaches

Build a model of the environment and use it for planning

#### 2.1. Dynamic programming

Requires a complete model of the environment and solves the Bellman equations iteratively.

#### 2.2. Monte Carlo Tree Search (MCTS)

Builds a search tree based on random sampling of the decision space.

## V. Advanced Topics

- **Deep Reinforcement Learning**: Combines Neural Networks with RL to handle high-dimensional state and action spaces.
    - **Deep Q-Networks (DQN)**: Use neural networks to approximate the Q-function
    - **Policy Gradient Methods**: Use neural networks to represent policies, optimized using gradient ascent
- **Exploration Strategies**:
    - **ϵ-Greedy**: With probability ϵ, select a random action; otherwise, select the best-known action
    - **Upper Confidence Bound (UCB)**: Balances exploration aand exploitation based on the uncertainty in the value estimates.
- **Temporal Difference (TD) Learning**: A blend of Monte Carlo methods and dynamic programming
    - **TD(0)**: Updates value estimates based on the difference betweenestimated values of consecutive states.
    - **TD(λ)**: Uses eligibility traces to consider multiple future steps

## VI. Applications of Reinforcement Learning

1. **Game Playing**: AlphaGo and AlphaZero have demonstrated superhuman performance in Go and Chess
2. **Robotics**: RL enables robots to learn complex tasks through interaction with their environment
3. **Autonomous vehicles**: Decision-making and control
4. **Natural Language Processing**: Dialogue systems and language modeling
5. **Finance**: Portfolio management and algorithmic trading
6. **Healthcare**: Personalized treatment strategies and resource allocation

## VII. Challenges in Reinforcement Learning

1. **Sample Efficiency**: RL often requires a large number of interactions with the environment.
2. **Credit Assignment Problem**: Determining which actions are responsible for future rewards.
3. **Sparse Rewards**: Environments where rewards are infrequent make learning difficult.
4. **Exploration in Large Spaces**: Efficient exploration in high-dimensional spaces remains a challenge.
5. **Safety and Ethics**: Ensuring RL agents act safely and ethically, especially in real-world applications.

## VIII. Future Directions
1. **Meta-Reinforcement Learning**: Agents learn to learn, adapting quickly to new tasks.
2. **Hierarchical RL**: Learning policies at multiple levels of abstraction.
3. **Multi-Agent RL**: Coordination and competition among multiple learning agents.
4. **Transfer Learning**: Applying knowledge from one task to improve learning in another.

Reinforcement Learning is a powerful framework for sequential decision-making problems. Its ability to learn optimal behaviors through interaction makes it suitable for a wide range of applications. Despite its challenges, ongoing research continues to advance the field, promising even more sophisticated and capable agents in the future.