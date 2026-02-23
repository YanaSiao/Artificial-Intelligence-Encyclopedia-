## 1. Models & Foundations
- [[Reinforcement Learning]]: General overview and the Agent-Environment interface.
- [[Sequential Decision Process]]: Problem formulation and examples.
- **Markov Models**:
	- [[Markov Decision Process]]: The core mathematical model (Finite).
	- [[Continuous MDP]]: Handling continuous state/action spaces.
	- [[Markov Games]]: Extension to multi-agent settings.

## 2. Algorithms
**[[RL Algorithm Classes]]**: High-level taxonomy of solutions (Model-based vs. Model-free, On-policy vs. Off-policy, Value-based vs. Policy-based).
### 2.1 Tabular & Exact Solutions
- [[Dynamic Programming]]: 
	- [[Policy Iteration]]
	- [[Value Iteration]]
- **Model-Free Prediction**:
	- [[Monte Carlo Methods]]
	- [[Temporal Difference Learning]] (TD)
- **Model-Free Control**:
	- [[Q-Learning]] (Off-policy)
	- [[SARSA]] (On-policy)

### 2.2 Approximation & Scaling
- [[Value Function Approximation]]: **Foundations** (Features, MSE, Linear VFA)
    
- **Incremental (Online) Methods**:
    - [[Incremental Methods]]: **Standard Updates** (TD, SARSA, Q-learning)
    - [[Convergence & The Deadly Triad]]: Theory and Stability
    - [[Gradient TD Methods]]: GTD, TDC
- **Batch (Offline) Methods**:
    - [[Batch RL]]: [[LSTD]], [[LSPI]], [[FQI]]
- **Policy Search (Foundations)**:  
	- [[Policy Gradients]]: Definitions, Score Function, & REINFORCE. 
	- [[Policy Gradient Theorem]]: The math for Actor-Critic. 
	- [[Natural Policy Gradient]]: Natural Gradient and Fisher Information.

### 2.3 Deep & Advanced RL
- [[Deep Reinforcement Learning]]:
	- Value-based: [[DQN]]
	- Policy-based: [[TRPO]], [[PPO]]
	- Actor-Critic: [[DDPG]], [[SAC]]
- **Planning & Learning**:
	- [[MCTS]]: Monte Carlo Tree Search.
	- [[AlphaZero]]: Combining MCTS with Deep Learning.
- [[Multi-agent RL]]: Overview of MARL challenges.

## 3. Applications
- [[Autonomous Driving]]
- [[Water Resources Management]]
- [[Algorithmic Trading]]