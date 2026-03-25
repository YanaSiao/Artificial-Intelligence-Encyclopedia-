## The SARSA Algorithm

SARSA stands for the sequence of events that trigger an update: **State, Action, Reward, next State, next Action**. Unlike Monte Carlo control, which waits for the end of an episode, SARSA updates the $Q$-value at every time step.

**The Update Rule:**

$$Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \alpha [R_{t+1} + \gamma Q(S_{t+1}, A_{t+1}) - Q(S_t, A_t)]$$

- **On-Policy Nature:** SARSA is on-policy because it uses the actual action $A_{t+1}$ chosen by the current policy $\pi$ to update $Q(S_t, A_t)$. It "learns about the policy it is currently following."
![[Pasted image 20260317124314.png]]
## Convergence Conditions

For SARSA to converge to the optimal action-value function $Q^*$, two main conditions must be satisfied:

1. **GLIE (Greedy in the Limit with Infinite Exploration):** 
	* All state-action pairs must be visited infinitely often.
    
    - The policy must eventually become greedy (e.g., $\epsilon \to 0$ as $t \to \infty$).
        
2. **Robbins-Monro Sequence:** The step-size $\alpha_t$ must satisfy:
    
    - $\sum_{t=1}^{\infty} \alpha_t = \infty$ (Step size is large enough to overcome any initial conditions).
        
    - $\sum_{t=1}^{\infty} \alpha_t^2 < \infty$ (Step size decreases fast enough to ensure convergence despite stochasticity).
        
    - _Practical note:_ A constant $\alpha$ does not satisfy the second condition, meaning $Q$ will oscillate around the mean rather than converging to a single point.

---

## SARSA(lambda)

**SARSA($\lambda$)** extends the algorithm by incorporating eligibility traces, allowing the reward to propagate back to multiple previous state-action pairs.

### Forward View: $n$-step SARSA

Similar to TD($\lambda$) for prediction, we can look at $n$-step returns for control.

- **$n$-step Return:** $q_t^{(n)} = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{n-1} R_{t+n} + \gamma^n Q(S_{t+n}, A_{t+n})$
    
- **$\lambda$-return:** The SARSA($\lambda$) target is a geometric weighted average of all $n$-step returns.

### Backward View: Eligibility Traces

In the backward view, we maintain an **eligibility trace** for every state-action pair $(s,a)$.

**Update Rules:**

1. **Trace Update:** $E_t(s,a) = \gamma \lambda E_{t-1}(s,a) + \mathbb{1}(S_t=s, A_t=a)$
    
2. **TD Error:** $\delta_t = R_{t+1} + \gamma Q(S_{t+1}, A_{t+1}) - Q(S_t, A_t)$
    
3. **$Q$-Value Update:** $Q(s,a) \leftarrow Q(s,a) + \alpha \delta_t E_t(s,a)$

---
## Comparison: SARSA(0) vs. SARSA($\lambda$)

| **Feature**           | **SARSA(0)**                                 | **SARSA(λ)**                                                  |
| --------------------- | -------------------------------------------- | ------------------------------------------------------------- |
| **Credit Assignment** | Updates only the immediate previous $(s,a)$. | Updates all recently visited $(s,a)$ based on trace strength. |
| **Propagation Speed** | Information travels one step per episode.    | Information can travel many steps in a single update.         |
| **Complexity**        | $O(1)$ per step.                             | $O(\|\mathcal{S}\| \times \|\mathcal{A}\|)$                   |
| **Bias/Variance**     | Lower variance, higher bias.                 | Higher variance, lower bias (as $\lambda \to 1$).             |

---
### Off-Policy SARSA

Unlike Q-learning, standard SARSA is on-policy. To make SARSA off-policy, we must apply Importance Sampling because the update depends on the next action $A_{t+1}$ sampled from $b$.

**The Update Rule:**

$$Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \alpha \rho_t [R_{t+1} + \gamma Q(S_{t+1}, A_{t+1}) - Q(S_t, A_t)]$$

where $\rho_t = \frac{\pi(A_{t+1}|S_{t+1})}{b(A_{t+1}|S_{t+1})}$.