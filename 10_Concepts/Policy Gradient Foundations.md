### Introduction

Unlike value-based RL (which computes $Q$ and acts greedily), Policy-Based RL learns a parametric policy $\pi_\theta(a|s)$ directly.

- **Goal**: Maximize $J(\theta) = \mathbb{E}_{s \sim d^\pi, a \sim \pi_\theta} [R(s,a)]$.
    
- **Advantages**:
    
    - Better convergence properties (no "max" operator oscillation).
        
    - Effective in high-dimensional or **continuous action spaces**.
        
    - Can learn **stochastic policies** (optimal in partially observable environments).
        

### The Log-Likelihood Trick (The "Score Function")

We want to calculate $\nabla_\theta J(\theta)$, but the distribution of trajectories depends on $\theta$. To avoid differentiating through the environment dynamics (which are unknown), we use the identity:

$$\nabla_\theta \pi_\theta(a|s) = \pi_\theta(a|s) \frac{\nabla_\theta \pi_\theta(a|s)}{\pi_\theta(a|s)} = \pi_\theta(a|s) \nabla_\theta \log \pi_\theta(a|s)$$

The term $\nabla_\theta \log \pi_\theta(a|s)$ is known as the **Score Function**. It tells us how to "push" the parameters to increase the probability of action $a$ in state $s$.

### REINFORCE: The Vanilla Policy Gradient

REINFORCE is the simplest "White-Box" policy gradient algorithm. It uses a single Monte Carlo estimate of the total return to update the policy parameters. The core principle is "Trial and Error": it pushes up the probability of trajectories that resulted in high rewards and decreases the probability of those with low rewards.

**Algorithm Mechanism**

1. Sample $m$ trajectories $\tau_i$ using the current policy $\pi_\theta$.
    
2. For each trajectory, compute the gradient estimate: $\hat{g}_i = \left( \sum_{t=0}^{T-1} \nabla_\theta \log \pi_\theta(a_t|s_t) \right) \left( \sum_{t=0}^{T-1} \gamma^t r_t \right)$.
    
3. Update the parameters: $\theta \leftarrow \theta + \alpha \frac{1}{m} \sum \hat{g}_i$.

**Pros and Cons**

- **Pros:** It is easy to compute and does not require the Markov property. It can be applied to partially observable MDPs (POMDPs) without modification because it treats the entire trajectory as a single unit. Unbiased. 
    
- **Cons:** It suffers from extremely high variance because it uses a single Monte Carlo estimate for the return. This variance grows with the length of the horizon $T$, often leading to slow convergence.    

---

### G(PO)MDP: Exploiting Causality

G(PO)MDP (Gradient of a Partially Observable MDP) is an improvement over REINFORCE that reduces variance by applying the **Causality Property**. It recognizes that a reward collected at time $t$ cannot be influenced by actions taken in the future ($t+1, t+2, \dots$).
![[Pasted image 20260323193057.png]]

**Algorithm Mechanism** Instead of multiplying the entire trajectory's return by the sum of all score functions, G(PO)MDP ensures each reward $r_t$ is only multiplied by the gradient of the actions that preceded it.

$$\hat{g}_{G(PO)MDP} = \sum_{t=0}^{T-1} \gamma^t r_t \left( \sum_{l=0}^{t} \nabla_\theta \log \pi_\theta(a_l|s_l) \right)$$

**Comparison with REINFORCE** While both provide unbiased estimates of the gradient, G(PO)MDP is significantly more efficient because its variance no longer depends on the horizon length $T$. This makes it a more stable "white-box" approach for longer tasks.

---

### Managing Variance and Bias

The primary challenge in policy gradients is the high variance of the stochastic gradient estimates.

**Baselines** To reduce variance without introducing bias, a baseline $b(s)$ can be subtracted from the return. As long as the baseline does not depend on the current action, the estimate remains unbiased because the expectation of the score function $\nabla_\theta \log \pi_\theta(a|s)$ is zero.

$$\nabla_\theta J(\theta) \approx \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(A_t|S_t) (G_t - b(S_t))$$

- **REINFORCE Baseline:** Often a time-independent constant or the average return.
    
- **Advantage Function:** A powerful baseline where we subtract the state-value $v(s)$ from the action-value $q(s,a)$. The result, $A(s,a) = q(s,a) - v(s)$, tells us how much better an action is compared to the average, significantly lowering variance.


**Bias in Actor-Critic** When using a "Critic" (a learned value function) instead of Monte Carlo returns, we introduce **bias** because the critic's estimate might be wrong. This can lead the agent to converge to sub-optimal solutions. To eliminate this, we use **Compatible Function Approximation**, which requires the critic to be structured in a specific way relative to the policy's score function.

---

### Optimality and Convergence

Policy gradient methods are essentially stochastic gradient ascent on a non-convex objective function $J(\theta)$.

- **Convergence:** They generally converge asymptotically to a **local minimum** rather than a global optimum.
    
- **Stability:** Unlike value-based methods (like Q-learning), policy gradients offer smoother updates because they make small, incremental changes to the policy parameters $\theta$ rather than drastic "greedy" jumps.
    
- **Exploration:** They can suffer from insufficient exploration if the policy becomes deterministic too quickly, which is why stochastic policies (like Softmax or Gaussian) are used to maintain exploration.