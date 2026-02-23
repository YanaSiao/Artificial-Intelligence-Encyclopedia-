### The "Perfect" Scenario

The goal of the agent is to navigate an **unknown environment** and derive an optimal policy solely by **learning from data** (interactions).

### 1. Spaces & Time

- **State Space ($S$):**
    
    - **Discrete:** Solved via **Tabular Approaches** (Exact solutions).
    - **Continuous:** Requires **Function Approximation** (Neural Networks, Linear).

- **Action Space ($A$):**
    - **Finite:** Discrete choices (e.g., Left, Right).
    - **Continuous:** Requires optimization methods (e.g., Actor-Critic, Policy Gradients).

- **Time ($T$):** Usually **Discrete** in this course ($t, t+1$).

### 2. Observability

- **Full Observability:** Agent sees the true state $S_t$.
- **Partial Observability (POMDP):** Agent sees $O_t$.
    - **Belief MDP:** Mapping the history of observations into a probability distribution (belief) over the possible true states.

### 3. Reward Architectures

- **Discounted (Q-Learning):** Focuses on immediate vs. future rewards using $\gamma$.
    
- **Average-Reward:** Optimizes the long-term arithmetic mean of rewards (common in continuing tasks).
    
- **Intrinsically Motivated:** The agent generates its own rewards (e.g., curiosity) to explore rare states. Useful for "pre-training" before a sparse external task is given.
    
- **Inverse RL (IRL):** The agent is given trajectories of expert behavior and must **infer the underlying reward function** $R(s, a)$.
    

### 4. Agent & Objective Count

- **Multi-Agent (MARL):** 
	* **Single-Agent View:** Other agents are part of the environment dynamics.
    - **Multi-Agent View:** Actions of Agent A change the optimal strategy for Agent B.
        
- **Multi-Objective:**
    - Results in a **Pareto Frontier**: A set of solutions where you cannot improve one objective without degrading another.