### The Compatible Function Approximation Theorem

The Policy Gradient Theorem requires the exact action-value function $Q^\pi(s, a)$, which is rarely known. In practice, we use a learned critic $Q_w(s, a)$ as an approximation. The Compatible Function Approximation Theorem defines the specific conditions under which replacing $Q^\pi$ with $Q_w$ introduces zero bias into the gradient estimate.

To maintain an exact gradient, the approximation must satisfy two conditions:

1. **Value Function Compatibility**: The gradient of the critic with respect to its parameters must match the score function of the policy: $\nabla_w Q_w(s, a) = \nabla_\theta \log \pi_\theta(a|s)$. This implies $Q_w$ must be linear in the features defined by the policy's gradient.
    
2. **Mean Squared Error Minimization**: The parameters $w$ must be chosen to minimize the mean squared error between the true action-value function and the approximation: $\epsilon = \mathbb{E}_{s \sim d^\pi, a \sim \pi} [(Q^\pi(s, a) - Q_w(s, a))^2]$.
    

If these conditions are met, the gradient calculated using the critic is identical to the true policy gradient.
![[Pasted image 20260325133806.png]]

### The Purpose and Selection of Baselines

Policy gradient estimators like REINFORCE are unbiased but suffer from high variance because the returns $G_t$ can fluctuate significantly across different trajectories. A baseline $b(s)$ is used to reduce this variance without shifting the expected value of the gradient (i.e., without introducing bias).

### Why Baselines are Unbiased

Subtracting a baseline is mathematically "safe" because the expectation of the score function multiplied by any function that does not depend on the current action is zero: $\mathbb{E}_{a \sim \pi} [\nabla_\theta \log \pi_\theta(a|s) b(s)] = 0$. This allows the algorithm to focus on the relative difference between actions rather than the absolute magnitude of the rewards.

### Choosing the Optimal Baseline

While any $b(s)$ preserves unbiasedness, the goal is to choose a baseline that minimizes variance. The most practical and common choice is the state-value function, $b(s) = V^\pi(s)$. By subtracting the expected value of being in a state, the update signal becomes the "Advantage," highlighting only those actions that performed better or worse than the state's average.

### Advantage Function: Bias and Instability

The advantage function $A(s, a) = Q(s, a) - V(s)$ is the theoretical ideal for variance reduction. However, in Actor-Critic settings, its implementation often leads to practical difficulties.

### The Problem of Bias

In Actor-Critic, we typically approximate the advantage using the Temporal Difference (TD) error: $\delta = r + \gamma V(s') - V(s)$. Unlike the Monte Carlo returns used in REINFORCE, this TD signal is biased because it relies on the Critic's current (and likely imperfect) estimate of $V$. If the Critic's value function is inaccurate, the Actor receives "bad advice," which can lead the policy to converge to a sub-optimal local maximum or fail to converge entirely.

### Stability and the Feedback Loop

Using the advantage function creates a "moving target" problem. As the Actor improves the policy $\pi$, the state-value function $V^\pi$ changes. The Critic must constantly update its estimates to catch up with the new policy. If the Critic is too slow, the advantage estimates become outdated; if it is too fast or unstable, the Actor may experience "chatter" or divergence. This interaction is a primary source of instability in deep reinforcement learning.