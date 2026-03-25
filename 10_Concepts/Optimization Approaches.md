### Black-Box Approaches (Derivative-Free Optimization)

Black-box methods treat the objective function $J(\theta)$ as a "black box" that can be sampled but not differentiated. They do not care about the sequence of states, actions, or rewards within an episode; they only look at the total return $G$ at the end of the trajectory.

#### Definition and Mechanism

The environment and the policy are viewed as a single function $f(\theta) \to \mathbb{R}$. To find the optimal $\theta$, these methods perturb the parameters, evaluate the resulting performance, and move $\theta$ toward the perturbations that yielded the highest returns.

#### Key Methods
- **Evolutionary Strategies (ES):** Generates a population of parameter vectors by adding Gaussian noise to the current $\theta$, evaluates them in parallel, and updates the mean.
    
- **Genetic Algorithms (GA):** Uses crossover and mutation of parameter "genomes" to evolve better policies.
    
- **Hill Climbing / Simulated Annealing:** Local search methods that accept or reject parameter changes based on performance improvements.

#### Use Cases
- **Non-Differentiable Policies:** When the policy involves hard logic, discrete look-up tables, or non-differentiable components (e.g., [[Heaviside step functions]]).
    
- **Sparse/Delayed Rewards:** When a reward is only given at the very end of a long task, making "step-by-step" credit assignment difficult.
    
- **Massive Parallelization:** Since ES only requires communicating a single scalar (the return) and the seed of the noise, it scales linearly across thousands of workers more easily than gradient-based methods.

### The White-Box

White-box methods exploit the underlying Markov Decision Process (MDP) structure. They look "inside" the episode at the specific transitions $(s_t, a_t, r_{t+1}, s_{t+1})$ to compute a gradient that indicates exactly how to change $\theta$ to increase the probability of high-reward actions.

#### Definition and Mechanism

These methods rely on the **Policy Gradient Theorem**. They use the **Score Function** $\nabla_\theta \log \pi_\theta(a|s)$ to establish a causal link between a specific action taken in a specific state and the eventual return.

#### Key Methods

- **REINFORCE:** A Monte Carlo policy gradient method that uses the full return of an episode to scale the gradient.
    
- **Actor-Critic:** Uses a learned value function (Critic) to reduce the variance of the gradient estimate (Actor).
    
- **PPO / TRPO:** Advanced gradient methods that constrain the size of the policy update to ensure training stability.

####  Use Cases

- **Sample Efficiency:** When interaction with the environment is expensive, white-box methods are generally superior because they extract more information from every transition.
    
- **High-Dimensional Parameter Spaces:** Deep neural networks have millions of parameters; gradients provide the most efficient "map" for navigating such a complex landscape.
    
- **Continuous Action Control:** Precision tasks like robotics benefit from the smooth, directed updates provided by gradients.

#### The Score Function (Likelihood Ratio)

To compute $\nabla_\theta J(\theta)$, we often need the **Score Function**:

$$\nabla_\theta \log \pi_\theta(a|s) = \frac{\nabla_\theta \pi_\theta(a|s)}{\pi_\theta(a|s)}$$

- **For Softmax:** $\nabla_\theta \log \pi_\theta(a|s) = x(s,a) - \sum_{a'} \pi_\theta(a'|s) x(s,a')$.
    
- **Intuition:** The gradient tells us how to change $\theta$ to increase the probability of action $a$ in state $s$.

####  The Policy Gradient Theorem 

This theorem is the foundation of all "White-Box" methods. It provides a way to compute the gradient of the total performance $J(\theta)$ using only local gradients of the policy:

$$\nabla_\theta J(\theta) \propto \sum_s d^\pi(s) \sum_a \nabla_\theta \pi_\theta(a|s) Q^\pi(s, a)$$

- **Crucial Insight:** The gradient does **not** require the derivative of the state distribution $d^\pi(s)$, which is usually unknown and depends on the environment.

---

| **Feature**                 | **Black-Box (ES/GA)**              | **White-Box (Policy Gradient)**                       |
| --------------------------- | ---------------------------------- | ----------------------------------------------------- |
| **Information Granularity** | Episode-level (Total Return)       | Transition-level ($s, a, r, s'$)                      |
| **Policy Requirements**     | Any (can be non-differentiable)    | Must be differentiable ($\nabla_\theta \pi$)          |
| **Credit Assignment**       | Temporal structure is ignored      | Exploits the Markov property                          |
| **Variance**                | Often more robust to noisy returns | High variance (often requires baselines)              |
| **DL Connection**           | Ignores Backpropagation            | Relies on [[Backpropagation and Training Algorithms]] |
