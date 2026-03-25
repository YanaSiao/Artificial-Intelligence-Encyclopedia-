# Off-Policy Policy Gradient Foundations

Off-policy policy gradient methods allow an agent to learn an optimal target policy while following a different behavior policy. This is critical for scenarios where we want to reuse historical data, learn from demonstrations, or maintain a highly exploratory behavior while optimizing a nearly deterministic target. The fundamental challenge is the distribution shift: the trajectories we observe are sampled from the behavior policy, but we need to estimate the gradient of the target policy performance.

# Importance Weighted Policy Gradient

To correct for the difference in policies, we apply Importance Sampling (IS). The importance weighted policy gradient uses the ratio of the target policy to the behavior policy to weight the gradients. If we consider a trajectory view, the gradient is multiplied by the product of ratios for every action in the episode. While this estimator is mathematically unbiased, it suffers from the curse of horizon. Because we are multiplying many ratios together, the variance of the estimate grows exponentially with the length of the trajectory, often making the gradient signal too noisy for effective learning.

![[Pasted image 20260325181650.png]]
# The Curse of Horizon and Causality Fixes

The curse of horizon occurs because the product of importance weights can either vanish to zero or explode to infinity as the time step increases. To mitigate this, we apply the causality property, similar to the logic in G(PO)MDP. Instead of using the full trajectory ratio for every transition, we only use the product of ratios up to the current time step. This reduces variance significantly because it eliminates the dependence on future random actions that cannot causally affect the current reward. However, even with causality, the variance remains high for long-horizon tasks.

![[Pasted image 20260325181908.png]]
# Occupancy View of Off-Policy Gradient

A more stable approach involves the occupancy view. Instead of weighting entire trajectories, we weight the gradient at each state using the ratio of the state occupancy measures. This formulation looks at the density of states under the target policy versus the behavior policy. By using the ratio of the discounted state-action distributions, we avoid the product of ratios entirely. This method is often more stable but requires estimating the occupancy ratio, which can be challenging in high-dimensional environments.

# Trust Region Methods (TRPO and PPO)

Trust region methods are a subset of advanced gradient methods designed to solve the instability of vanilla policy gradients. In standard gradient descent, a large step in parameter space can lead to a catastrophic drop in performance. Trust region methods prevent this by ensuring that the new policy does not deviate too far from the old policy in terms of behavior. They fit perfectly into the advanced gradient methods category because they replace simple first-order updates with constrained optimization problems.

# How Trust Region Methods Work

[[Trust Region Methods. TRPO and PPO|Trust Region Policy Optimization]] (TRPO) enforces a hard constraint on the change in the policy using the Kullback-Leibler (KL) divergence. It effectively says: find the best improvement in the objective such that the average KL divergence between the old and new policy is less than a small threshold. This ensures monotonic improvement, meaning the policy is theoretically guaranteed to get better or stay the same at every step. Proximal Policy Optimization (PPO) is a more practical evolution of this idea. Instead of a hard KL constraint, PPO uses a clipped objective function that penalizes changes that move the policy too far from the current one, providing similar stability with much simpler implementation.

# Variance Comparison and Stability

In terms of variance, off-policy methods are generally noisier than on-policy methods due to the importance sampling weights. However, they are more sample-efficient because they can reuse data multiple times. Trust region methods provide the necessary stability to handle this trade-off. By clipping the importance weights (as in PPO) or constraining the update step (as in TRPO), these algorithms prevent the high-variance IS ratios from causing large, destructive updates to the neural network weights. This makes them the industry standard for robust reinforcement learning.