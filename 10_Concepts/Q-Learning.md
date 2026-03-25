Q-learning is the most famous off-policy TD control algorithm. It is unique because it **does not require importance sampling** for its 1-step update.

**The Update Rule:**

$$Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \alpha [R_{t+1} + \gamma \max_{a'} Q(S_{t+1}, a') - Q(S_t, A_t)]$$

**Why is it Off-Policy?**

The "Target" is the greedy policy (implied by the $\max$ operator), but the agent can follow any behavior policy (like $\epsilon$-greedy) to choose $A_t$. Because it directly uses the maximum value of the next state rather than the value of the action actually taken, the discrepancy is handled without an IS ratio.