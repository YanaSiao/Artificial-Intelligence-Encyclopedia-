Value Iteration (VI) is an algorithm that computes the optimal value function $V^*$ by iteratively applying the **Bellman Optimality Backup**. Unlike Policy Iteration, it does not require an explicit policy evaluation step to reach convergence.

### The Algorithm

Starting with an arbitrary initial value function $V_0$, we update the value of every state $s$ simultaneously:

$$V_{k+1}(s) = \max_{a \in \mathcal{A}} \left[ R(s,a) + \gamma \sum_{s' \in \mathcal{S}} P(s'|s,a) V_k(s') \right]$$

Or, in operator notation: $V_{k+1} = T^* V_k$, where $T^*$ is the **Bellman Optimality Operator**.

- **Greedy Policy Extraction**: Once $V_k$ has converged to $V^*$, the optimal policy $\pi^*$ is extracted by taking the $\arg\max$:
    $$\pi^*(s) = \arg\max_{a \in \mathcal{A}} \left[ R(s,a) + \gamma \sum_{s' \in \mathcal{S}} P(s'|s,a) V^*(s') \right]$$

### Convergence and Contraction

The convergence of Value Iteration is guaranteed by the **Contraction Mapping Theorem**.

- **The Property**: The Bellman Optimality Operator $T^*$ is a $\gamma$-contraction under the maximum norm ($\infty$-norm). This means for any two value functions $U$ and $V$:
    
    $$\|T^*U - T^*V\|_\infty \le \gamma \|U - V\|_\infty$$
    
- **Implications**:
    
    1. **Uniqueness**: There exists a unique fixed point $V^*$ such that $T^*V^* = V^*$.
        
    2. **Convergence**: Starting from any $V_0$, the sequence $V_k$ converges to $V^*$ at a geometric rate.
        
    3. **Stability**: Errors in the value function are reduced by at least a factor of $\gamma$ at each iteration.
        

#### Proof
![[Pasted image 20260312091628.png]]
