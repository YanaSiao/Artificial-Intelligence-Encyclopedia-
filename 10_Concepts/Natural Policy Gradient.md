# The Core Concept of Natural Gradient

Standard Gradient Descent (Vanilla Gradient) operates in the Euclidean space of the parameters $\theta$. It assumes that a change of magnitude $||\Delta \theta||$ has a uniform effect on the resulting policy regardless of the current values of $\theta$. However, in reinforcement learning, the relationship between parameters and the resulting distribution (the policy) is non-linear. A small change in $\theta$ might lead to a negligible change in the policy in some regions of the parameter space, but a massive, catastrophic shift in others.

Natural Gradient Descent solves this by operating on the manifold of probability distributions rather than the parameter space. It measures the "distance" of a step not by how much the coordinates $\theta$ change, but by how much the actual distribution $\pi_\theta$ changes. This ensures that the update step is consistent in terms of the agent's behavior, leading to more stable and reliable convergence.

# KL Divergence and the Distribution Manifold

The mathematical tool used to measure the distance between two probability distributions is the **Kullback-Leibler (KL) Divergence**. For a policy $\pi_\theta$, we want to find a step $\Delta \theta$ that maximizes the improvement in performance $J(\theta)$ while keeping the KL divergence between the old and new policy small.

$KL(\pi_\theta || \pi_{\theta+\Delta\theta}) = \mathbb{E}_{s \sim d^\pi} [ KL(\pi_\theta(\cdot|s) || \pi_{\theta+\Delta\theta}(\cdot|s)) ]$

When $\Delta \theta$ is very small, a second-order Taylor expansion of the KL divergence around $\theta$ reveals that the first-order term is zero and the second-order term is determined by the curvature of the space. This curvature is represented by the Fisher Information Matrix.

# The Fisher Information Matrix (FIM)

The Fisher Information Matrix $F(\theta)$ is a local quadratic approximation of the KL divergence. It represents the Riemannian metric of the manifold of the policy distribution.

$F(\theta) = \mathbb{E}_{s \sim d^\pi, a \sim \pi_\theta} [ \nabla_\theta \log \pi_\theta(a|s) \nabla_\theta \log \pi_\theta(a|s)^T ]$

# Properties of the FIM

- It is a symmetric and positive semi-definite matrix.
    
- It is independent of the parameterization of the policy; it captures the intrinsic geometry of the distribution space.
    
- It represents the "information" that the observable action carries about the unknown parameters.
    
- Unlike the Hessian matrix used in Newton's method (which depends on the loss function), the FIM depends only on the policy structure itself.

# Derivation of the Natural Policy Gradient

To derive the natural gradient update, we set up a constrained optimization problem: maximize the local gain in performance subject to a small, constant change in the policy distribution (measured by KL divergence).

Objective: $\max_{\Delta \theta} \nabla_\theta J(\theta)^T \Delta \theta$

Constraint: $\frac{1}{2} \Delta \theta^T F(\theta) \Delta \theta \le \epsilon$

Using the method of Lagrange multipliers, we define the Lagrangian $L(\Delta \theta, \lambda) = \nabla_\theta J(\theta)^T \Delta \theta - \lambda (\frac{1}{2} \Delta \theta^T F(\theta) \Delta \theta - \epsilon)$. Taking the derivative with respect to $\Delta \theta$ and setting it to zero gives:

$\nabla_\theta J(\theta) - \lambda F(\theta) \Delta \theta = 0 \implies \Delta \theta = \frac{1}{\lambda} F(\theta)^{-1} \nabla_\theta J(\theta)$

The "Natural Gradient" is thus defined as $\tilde{\nabla}_\theta J(\theta) = F(\theta)^{-1} \nabla_\theta J(\theta)$. This direction points toward the steepest ascent on the distribution manifold rather than the steepest ascent in the parameter coordinates.

# Natural Actor-Critic (NAC)

Calculating and inverting the FIM in every step is computationally expensive, especially for large neural networks ($O(d^3)$ complexity). Natural Actor-Critic algorithms simplify this by using **Compatible Function Approximation**.

If we use a critic $Q_w(s,a)$ that is compatible with the actor (i.e., its features are the score functions $\nabla_\theta \log \pi_\theta(a|s)$), the parameters $w$ that minimize the mean squared error are exactly the natural gradient: $w = F(\theta)^{-1} \nabla_\theta J(\theta)$. This allows the agent to follow the natural gradient path simply by performing a standard linear regression in the critic's weight space.

![[Pasted image 20260325180540.png]]
# Use Cases and Differences from Other Approaches

The primary use case for Natural Policy Gradient is to avoid the "plateau" problem in RL, where vanilla gradients become very small and training stalls. Because NPG is invariant to parameter rescaling, it moves efficiently through these regions.

# Comparison with Other Methods

- **vs. Gradient Descent:** NPG is faster in "flat" regions and safer in "steep" regions of the probability manifold.
    
- **vs. Newton's Method:** Newton's method uses the Hessian of the objective function ($J$). NPG uses the Fisher Information of the policy. NPG is generally more stable in RL because the Hessian of $J$ can be non-positive definite, whereas the FIM is always positive semi-definite.
    
- **Relationship to TRPO/PPO:** Trust Region Policy Optimization (TRPO) is essentially a practical, large-scale implementation of the Natural Policy Gradient that uses conjugate gradient descent to avoid the explicit inversion of the FIM.