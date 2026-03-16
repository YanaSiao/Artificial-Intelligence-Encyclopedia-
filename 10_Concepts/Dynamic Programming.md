### Policy Iteration vs. Value Iteration

The choice between Policy Iteration (PI) and Value Iteration (VI) often depends on the state space size and the specific structure of the MDP.

**Computational Complexity**

- **Per Iteration Cost:**
    
    - **Value Iteration:** $O(|\mathcal{A}| |\mathcal{S}|^2)$ for a full sweep. It involves one maximization over actions for every state.
        
    - **Policy Iteration:** Each iteration involves Policy Evaluation ($O(|\mathcal{S}|^3)$ if solved exactly via matrix inversion, or multiple $O(|\mathcal{A}| |\mathcal{S}|^2)$ sweeps if solved iteratively) and one Policy Improvement sweep ($O(|\mathcal{A}| |\mathcal{S}|^2)$).
        
- **Total Convergence:**
    
    - PI typically requires very few iterations to find the optimal policy (often 5–10 in practice).
        
    - VI may take many iterations to converge, as it only approaches $V^*$ asymptotically.
        

**Polynomial Complexity vs. Curse of Dimensionality**

Finding an optimal policy is **polynomial** in the number of states $|\mathcal{S}|$ and actions $|\mathcal{A}|$. While this makes DP efficient compared to brute force ($|\mathcal{A}|^{|\mathcal{S}|}$), it still suffers from the **Curse of Dimensionality**: as the number of state variables grows, the total number of states $|\mathcal{S}|$ grows exponentially, making full sweeps computationally impossible.

### Asynchronous Dynamic Programming

Synchronous DP requires updating every state in every sweep, which is redundant if some states' values aren't changing much. Asynchronous DP updates states in-place in any order, using whatever values are currently available.

**In-Place DP**

Synchronous DP stores two versions of the value function: $V_{old}$ and $V_{new}$. In-place DP uses only one copy. As soon as $V(s)$ is updated, the new value is immediately available for the calculation of the next state $s'$ in the same sweep. This often doubles the speed of convergence.

**Prioritized Sweeping**

This method prioritizes updating states that have the largest **Bellman Error**.

- **Bellman Error:** The difference between the current value and the value after a backup: $\Delta(s) = | \max_a (R + \gamma \sum P V) - V(s) |$.
    
- **Mechanism:**
    
    1. Maintain a priority queue of states.
        
    2. Update the state at the top of the queue.
        
    3. If the value of state $s$ changes significantly, calculate the potential change for its _predecessor_ states and add them to the queue.
        

**Real-Time Dynamic Programming (RTDP)**

Only states visited by the agent during "episodes" are updated. This focuses computation on parts of the state space that are actually reachable and relevant under the current policy.

---

### Backup Size and Full Backups

In the context of DP and RL, the "backup" refers to how we update the value of a state based on future information.

**Full Backups (DP)**

Dynamic Programming utilizes **Full Backups** (also called "Expected Backups").

- **Width:** A full backup has a "width" equal to the branching factor of the MDP. To update $V(s)$, we look at **all** possible next states $s'$ and **all** possible actions $a$, weighted by their probabilities.
    
- **Requirement:** Requires a perfect model of $P(s'|s,a)$ and $R(s,a)$.
    

**Sample Backups (RL)**

In contrast, Reinforcement Learning uses **Sample Backups**.

- **Width:** The width is 1. We only look at the specific state $s'$ that we actually transitioned into.
    
- **Advantage:** Does not require a model of the environment; it only requires experience (samples).
    
- **Trade-off:** Sample backups have higher variance than full backups because they rely on a single realization of the transition dynamics.