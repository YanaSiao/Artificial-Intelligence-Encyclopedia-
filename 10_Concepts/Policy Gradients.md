#### 1. Introduction

Unlike value-based RL (which computes $Q$ and acts greedily), Policy-Based RL learns a parametric policy $\pi_\theta(a|s)$ directly.

- **Goal**: Maximize $J(\theta) = \mathbb{E}_{s \sim d^\pi, a \sim \pi_\theta} [R(s,a)]$.
    
- **Advantages**:
    
    - Better convergence properties (no "max" operator oscillation).
        
    - Effective in high-dimensional or **continuous action spaces**.
        
    - Can learn **stochastic policies** (optimal in partially observable environments).
        

#### 2. The Log-Likelihood Trick (The "Score Function")

We want to calculate $\nabla_\theta J(\theta)$, but the distribution of trajectories depends on $\theta$. To avoid differentiating through the environment dynamics (which are unknown), we use the identity:

$$\nabla_\theta \pi_\theta(a|s) = \pi_\theta(a|s) \frac{\nabla_\theta \pi_\theta(a|s)}{\pi_\theta(a|s)} = \pi_\theta(a|s) \nabla_\theta \log \pi_\theta(a|s)$$

The term $\nabla_\theta \log \pi_\theta(a|s)$ is known as the **Score Function**. It tells us how to "push" the parameters to increase the probability of action $a$ in state $s$.

#### 3. Foundational Algorithms (White-Box)

##### **REINFORCE (Monte-Carlo Policy Gradient)**

The most basic implementation. It updates the policy based on the total return of a full trajectory.

- **Update Rule**: $\theta_{t+1} = \theta_t + \alpha G_t \nabla_\theta \log \pi_\theta(A_t|S_t)$
    
- **Pros**: Simple, unbiased.
    
- **Cons**: **High Variance**. Because $G_t$ is a sample from the end of the episode, a single "lucky" or "unlucky" event can cause massive gradient swings.
    

##### **G(PO)MDP**

An improvement that reduces variance by utilizing **causality**. It acknowledges that the reward at time $t$ is only affected by actions taken _before_ or _at_ time $t$.

- **Core Idea**: It breaks the trajectory return into components, ensuring that $\nabla \log \pi$ only multiplies the rewards it could actually have influenced.
    

#### 4. Variance Reduction: The Baseline

To make the gradient more stable without introducing bias, we subtract a baseline $b(s)$ from the return.

$$\nabla_\theta J(\theta) \approx \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(A_t|S_t) (G_t - b(S_t))$$

- **Standard Baseline**: The state-value function $V(s)$.
    
- **Result**: This measures the **Advantage** ($G_t - V(S_t)$). If an action resulted in a reward better than average, we increase its probability; if worse, we decrease it.