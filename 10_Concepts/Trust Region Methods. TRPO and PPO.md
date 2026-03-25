# The Problem of Policy Collapse

Standard policy gradient methods like REINFORCE or Actor-Critic are highly sensitive to the choice of step size (learning rate). If the step size is too small, training is painfully slow; if it is too large, a single update can move the policy into a region of the parameter space where the agent performs poorly. Because the data collection depends on the policy, a "bad" policy leads to "bad" data, which often results in a catastrophic drop in performance from which the agent cannot recover. Trust Region methods address this by ensuring that each update stays within a "safe" distance of the previous policy.

# Trust Region Policy Optimization (TRPO)

TRPO provides a theoretical framework for making monotonic improvements to a policy. It uses the Performance Difference Lemma to define a surrogate objective that approximates the true performance of a new policy $\pi_{\theta}$ using data collected from an old policy $\pi_{\theta_{old}}$.

### The Surrogate Objective

The goal is to maximize the expected advantage of the new policy:

$L(\theta) = \mathbb{E}_{s \sim d^{\pi_{\theta_{old}}}, a \sim \pi_{\theta_{old}}} \left[ \frac{\pi_{\theta}(a|s)}{\pi_{\theta_{old}}(a|s)} A^{\pi_{\theta_{old}}}(s, a) \right]$

Here, the ratio $\frac{\pi_{\theta}(a|s)}{\pi_{\theta_{old}}(a|s)}$ is an importance sampling weight that allows us to evaluate the new policy using samples from the old one.

### The KL Constraint

To ensure the approximation remains valid, TRPO imposes a hard constraint on the average Kullback-Leibler (KL) divergence between the old and new policies:

$\mathbb{E}_{s \sim d^{\pi_{\theta_{old}}}} [KL(\pi_{\theta_{old}}(\cdot|s) || \pi_{\theta}(\cdot|s))] \le \delta$

This constraint defines the "Trust Region." As long as the change in the policy distribution is small (less than $\delta$), the surrogate objective $L(\theta)$ is a reliable predictor of actual performance improvement.

### TRPO Algorithm

1. Collect a set of trajectories using the current policy $\pi_{\theta_{old}}$.
    
2. Compute the advantages $A^{\pi_{\theta_{old}}}(s, a)$.
    
3. Solve the constrained optimization problem:
    
    Maximize $L(\theta)$ subject to $KL(\theta_{old}, \theta) \le \delta$.
    
4. Use Conjugate Gradient to solve for the search direction and a line search to ensure the constraint is met and the objective improves.
    

# Proximal Policy Optimization (PPO)

While TRPO is theoretically sound, it is computationally expensive due to the second-order optimization (Kullback-Leibler constraint and Fisher Information Matrix) required in each step. PPO was developed to achieve the stability of TRPO using only first-order optimization (standard gradient descent).

### PPO-Clip: The Clipped Surrogate Objective

PPO simplifies the trust region by "clipping" the importance sampling ratio $r_t(\theta) = \frac{\pi_{\theta}(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)}$. The objective function is defined as:

$J^{CLIP}(\theta) = \mathbb{E}_t [ \min(r_t(\theta) \hat{A}_t, \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon) \hat{A}_t) ]$

This clipping mechanism prevents the ratio from moving too far away from 1. If the advantage is positive, the objective stops increasing once the ratio exceeds $1+\epsilon$. If the advantage is negative, it stops decreasing once the ratio falls below $1-\epsilon$. This effectively discourages the policy from making large changes that the data cannot support.

### PPO-Penalty: Adaptive KL Penalty

An alternative version of PPO uses a penalty instead of a hard constraint. It adds the KL divergence to the loss function with a coefficient $\beta$ that is automatically adjusted:

$J^{KLPEN}(\theta) = \mathbb{E}_t [ r_t(\theta) \hat{A}_t - \beta KL(\pi_{\theta_{old}}, \pi_{\theta}) ]$

If the KL divergence is too high, $\beta$ increases to punish the change; if it is too low, $\beta$ decreases.

### PPO Algorithm

1. Collect $T$ timesteps of data using the current policy.
    
2. Compute advantage estimates $\hat{A}_1, \dots, \hat{A}_T$.
    
3. Optimize the clipped surrogate loss (and potentially a value function loss and entropy bonus) for $K$ epochs using a small batch size.
    
4. Update $\theta_{old} \leftarrow \theta$ and repeat.
    

# Comparison and Convergence

TRPO is more theoretically rigorous and offers stronger guarantees of monotonic improvement, but it is difficult to implement and does not scale well to large neural networks with shared architectures (like those used in Actor-Critic). PPO is significantly easier to implement, works well with deep learning frameworks, and often achieves similar or better performance with less computational overhead. PPO has become the default algorithm for many RL applications due to its balance of ease of use, sample efficiency, and stability.