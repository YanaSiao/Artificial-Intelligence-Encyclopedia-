###  The Policy-Based Approach

In Policy-Based Reinforcement Learning, the agent learns a policy $\pi$ directly without necessarily relying on a value function. While Value-Based methods (like Q-Learning) learn a policy implicitly via $\pi(s) = \arg\max_a Q(s,a)$, Policy-Based methods search the policy space directly.

**Advantages over Value-Based Methods:**

1. **Continuous Action Spaces:** Value-based methods require a `max` over actions, which is computationally expensive in continuous or high-dimensional spaces. Policy gradients can handle these via continuous distributions.
    
2. **Stochastic Policies:** Can naturally learn stochastic optimal policies (important in POMDPs or adversarial games like Rock-Paper-Scissors), whereas value-based methods are typically deterministic.
    
3. **Simpler Approximation:** Sometimes the policy is a simpler function to approximate than the value function (e.g., "if the ball is left, move left").
    
4. **Convergence Properties:** Policy gradient methods generally have better convergence guarantees compared to the "Deadly Triad" risks associated with off-policy value-based learning.

---

###  Parametric Policies

A **Parametric Policy** $\pi_\theta$ is a mapping from a parameter vector $\theta \in \mathbb{R}^d$ to a distribution over actions. The goal is to find the $\theta$ that maximizes the expected return $J(\theta)$.

**Discrete Action Space: Softmax Policy**

The most common parametric policy for discrete actions uses a linear combination of features $\phi(s,a)$ and the parameter vector $\theta$:

$$\pi_\theta(a|s) = \frac{\exp(\theta^T \phi(s,a))}{\sum_{a' \in \mathcal{A}} \exp(\theta^T \phi(s,a'))}$$

_The action preferences are scaled by the exponential to ensure positive probabilities that sum to 1._
**Connection to DL:** This is identical to the [[CNN Anatomy. The Classifier Head|The Classifier Head (Softmax)]] in CNNs.

**Continuous Action Space: Gaussian Policy**

For a continuous action $a \in \mathbb{R}$, we often model the policy as a Gaussian distribution:

$$\pi_\theta(a|s) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{(a - \mu_\theta(s))^2}{2\sigma^2} \right)$$

- **Mean ($\mu_\theta(s)$):** Usually a linear combination of features $\theta^T \phi(s)$.
    
- **Standard Deviation ($\sigma$):** Can be fixed or parameterized (often using $\exp(\eta)$ to ensure positivity).

---

### Optimization Taxonomy

There are two primary ways to optimize the parameters $\theta$:

#### Black-Box Approaches

These treat the RL objective $J(\theta)$ as a function that can be sampled but whose internal structure is unknown.

- **Methods:** Evolutionary Strategies (ES), Genetic Algorithms, Hill Climbing.
    
- **Mechanism:** Perturb $\theta$, evaluate the resulting total return, and keep the best $\theta$.
    
- **Cons:** Ignores the temporal structure of the MDP (the state-action-reward sequence).

#### White-Box Approaches (Policy Gradients)

These exploit the structure of the MDP. They use the **Policy Gradient Theorem** to compute the gradient of the performance measure with respect to the parameters: $\nabla_\theta J(\theta)$.

- **Methods:** REINFORCE, G(PO)MDP, Actor-Critic.
    
- **Mechanism:** Update $\theta$ in the direction of the gradient: $\theta_{k+1} = \theta_k + \alpha \nabla_\theta J(\theta_k)$.

_See notes_: [[Optimization Approaches]]: Black-Box (Evolutionary) vs. White-Box (Gradient).