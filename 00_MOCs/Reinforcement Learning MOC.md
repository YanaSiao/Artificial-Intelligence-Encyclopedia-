## 1. Models & Foundations

- [[Reinforcement Learning]]: Overview and Agent-Environment loop.
- [[Sequential Decision Process]]: History, State, and Action definitions.
- **Markov Theory**:
	- [[Markov Processes]]: Stochastic processes, Chapman-Kolmogorov, and state properties. 
	- [[Markov Chain Dynamics and Classification]]
	- [[Markov Reward Process]]: Adding rewards and the Bellman Equation for MRPs.
	- [[Markov Decision Process]]: Adding actions and the Bellman Optimality Equation.

## 2. Algorithms

**[[RL Algorithm Classes]]**: High-level taxonomy of solutions (Model-based vs. Model-free, On-policy vs. Off-policy, Value-based vs. Policy-based).
### 2.1 Tabular & Exact Solutions

- **[[Dynamic Programming]]**: 
	- [[DP Foundations]]: Assumptions, Complexity, and Policy Evaluation. 
	- [[Policy Iteration]]: The Evaluation-Improvement loop. 
	- [[Value Iteration]]: Solving the Optimality Equation. 
	- [[Linear Programming for MDPs]]: Exact solutions via optimization.
- **Model-Free Prediction**:
	- [[Monte Carlo Prediction]] : Learning from complete episodes.
	- [[Temporal Difference Learning]] (TD): Bootstrapping from experience.
	- [[MC vs. TD]]: Comparison of bias, variance, and convergence.
	- [[TD(lambda)]]: Bridging MC and TD with Eligibility Traces.
- **Model-Free Control**:
	- [[Monte Carlo Control]] : On-policy optimization using $\epsilon$-greedy exploration.
	- [[SARSA]] (On-policy)
	
	- [[Off-Policy Learning Foundations]]
		- [[Q-Learning]] (Off-policy)
		- [[SARSA vs. Q-Learning]]

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

### 2.3 Policy Search & Policy Gradients
- [[Policy-Based RL & Parametric Policies]]: Advantages and taxonomy.
- [[Optimization Approaches]]: Black-Box (Evolutionary) vs. White-Box (Gradient).
- [[Policy Gradient Foundations]]: Score Function, REINFORCE, and G(PO)MDP.
- [[Policy Gradient Theorem]]: The fundamental math for Actor-Critic.
- [[Convergence & Compatible Approximation]]: Conditions for local optima and function compatibility.
- [[Natural Policy Gradient]]: Natural Gradient, Fisher Information, and NAC.
- [[Advanced Gradient Methods]]: Off-Policy Policy Gradients and Trust Regions (TRPO/PPO).