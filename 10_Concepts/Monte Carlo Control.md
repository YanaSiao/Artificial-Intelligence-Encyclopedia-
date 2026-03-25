### Generalized Policy Iteration (GPI) with MC

In model-free control, we follow the GPI framework: evaluate the current policy, then improve it.

- **Why Action-Values ($Q$)?** In model-based DP, we can improve a policy using $V(s)$ because we know $P(s'|s,a)$. In a model-free setting, we must use $Q(s,a)$ so that the agent knows which action to take without needing the transition dynamics.
    
- **The Loop:** $Q \to \text{greedy}(Q) \to Q \to \dots$

---

### Exploration vs. Exploitation: $\epsilon$-Greedy

A purely greedy policy in a model-free setting is dangerous because it may never explore the optimal action. To ensure continuous exploration, we use an **$\epsilon$-greedy policy**:

- With probability $1 - \epsilon$: Choose the greedy action $a^* = \arg\max_a Q(s,a)$.
    
- With probability $\epsilon$: Choose an action at random (including the greedy one).
    

**Proof of Policy Improvement**

The **Policy Improvement Theorem** holds for $\epsilon$-greedy policies. For any $\epsilon$-greedy policy $\pi$, the $\epsilon$-greedy policy $\pi'$ with respect to $Q^\pi$ is an improvement, meaning $v_{\pi'}(s) \geq v_\pi(s)$ for all $s \in \mathcal{S}$.
![[Pasted image 20260317123250.png]]

---

### Convergence to Optimality: GLIE

To converge to the true optimal policy $\pi^*$, we cannot maintain a constant exploration rate $\epsilon$. We must satisfy the **GLIE** (Greedy in the Limit with Infinite Exploration) conditions:

1. **Infinite Exploration:** All state-action pairs $(s,a)$ must be visited an infinite number of times.
    
    - $\sum_{i=1}^\infty \mathbb{1}(S_i = s, A_i = a) = \infty$
        
2. **Greedy Limit:** The policy must eventually become purely greedy.
    
    - $\lim_{k \to \infty} \epsilon_k = 0$

**Example schedule:** $\epsilon_k = 1/k$ is a common way to satisfy GLIE.

---

### Convergence Properties

- **MC Control vs. DP:** Unlike Dynamic Programming, which converges in a finite number of iterations (policy iterations), MC Control is stochastic and converges to the optimal action-value function $Q^*$ only as the number of episodes approaches infinity.
    
- **Batch vs. Online:** MC Control is "offline" in the sense that updates only occur after a complete episode is finished.

---

### Dependence: $\alpha$ vs. $\epsilon$

The learning process is a balance between the learning rate ($\alpha$) and the exploration rate ($\epsilon$):

- **$\alpha$ (Step-size):** Controls how much the current return $G_t$ affects the existing estimate $Q(s,a)$. A high $\alpha$ ignores history; a low $\alpha$ results in slow, stable learning.
    
- **$\epsilon$ (Exploration):** Controls how often the agent tries new things.
    
- **Interaction:** If $\epsilon$ is high but $\alpha$ is low, the agent explores a lot but learns very little from each experience. If $\alpha$ is high but $\epsilon$ is low, the agent may quickly "lock in" to a sub-optimal policy because it didn't explore enough.
---
### Off-Policy Monte Carlo Control

Off-policy MC uses the Importance Sampling ratio to update action-values from complete episodes.

- **Update:** $Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \alpha [ \rho_{t+1:T-1} G_t - Q(S_t, A_t) ]$.
    
- **Limitations:** Because it uses the product of ratios over a whole episode, the variance is often so high that it makes the algorithm impractical for anything other than very short episodes. If any action in the trajectory has $\pi(a|s) = 0$ while $b(a|s) > 0$, the return for that whole episode is ignored (ratio becomes 0).

_See relevant notes_: [[Monte Carlo Prediction]]