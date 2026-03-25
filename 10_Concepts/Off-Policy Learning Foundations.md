**Core Definitions**

- **Target Policy ($\pi$):** The policy the agent is trying to learn and optimize (usually the optimal greedy policy).
    
- **Behavior Policy ($b$):** The policy used to generate behavior and collect data (usually a more exploratory policy like $\epsilon$-greedy).

**The Coverage Assumption**

For off-policy learning to be possible, the behavior policy $b$ must be able to explore everything the target policy $\pi$ might do. Mathematically, this means:

$$\pi(a|s) > 0 \implies b(a|s) > 0$$

**Why use Off-Policy?**

1. **Exploration:** Follow a highly exploratory policy while learning the deterministic optimal policy.
    
2. **Learning from others:** Learn by observing a human or another agent.
    
3. **Replay Buffers:** Re-use experience from previous versions of the policy (crucial for Deep RL).

---

### Importance Sampling

When using data from $b$ to estimate values for $\pi$, we must correct for the discrepancy in action frequencies. This is done via **Importance Sampling (IS)**.

**The Importance Sampling Ratio**

The probability of a trajectory under policy $\pi$ divided by the probability under policy $b$. Since the transition dynamics $P(s'|s,a)$ cancel out, the ratio depends only on the policies:

$$\rho_{t:T-1} = \prod_{k=t}^{T-1} \frac{\pi(A_k|S_k)}{b(A_k|S_k)}$$

**Impact and Variance**

- **Ordinary IS:** $V(s) = \frac{\sum \rho_i G_i}{N}$. This is unbiased but has **extremely high variance**, as the product of ratios can explode or vanish.
    
- **Weighted IS:** $V(s) = \frac{\sum \rho_i G_i}{\sum \rho_i}$. This reduces variance significantly and is generally preferred, though it introduces a small bias in finite samples.