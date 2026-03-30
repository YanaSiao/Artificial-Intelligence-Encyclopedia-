# Asynchronous (A3C) vs. Synchronous (A2C)

The Advantage Actor-Critic architecture was first proposed on a large scale as A3C (Asynchronous Advantage Actor-Critic). In A3C, multiple independent agents (actors) interact with their own copies of the environment in parallel. Each actor computes gradients locally and reports them asynchronously to a global central model.

A2C (Synchronous Advantage Actor-Critic) is the synchronous version of this approach. Instead of updating a global model whenever an actor finishes, A2C waits for all parallel actors to complete their segments of experience (rollouts). It then averages the gradients and performs a single update. A2C is often preferred in modern implementations because it makes more efficient use of GPU acceleration compared to the overhead of managing many asynchronous CPU threads.

# The Core Ingredient: Advantage Estimation

The "Advantage" in A2C refers to the use of an advantage function $\mathbb{A}^{\pi}(S,A)$ rather than raw returns or Q-values in the policy gradient update.

- **The Advantage Function**: Defined as $\mathbb{A}^{\pi}(S,A) = q_{\pi}(S,A) - v_{\pi}(S)$.
    
- **Role**: It represents how much better an action $A$ is compared to the average behavior of the policy at state $S$.
    
- **Estimation**: In practice, the advantage is estimated using the Temporal Difference (TD) error: $\delta_t^w = R_{t+1} + \gamma v_w(S_{t+1}) - v_w(S_t)$. This is because the expected value of the TD error is equal to the advantage.

# Differences from Standard Actor-Critic

While standard Actor-Critic methods use a critic to estimate values, A2C/A3C introduce specific improvements for stability and efficiency:

1. **Advantage instead of Q-Value**: By subtracting the state-value $v(S)$ (the "baseline"), the variance of the gradient estimates is significantly reduced without introducing bias.
    
2. **Parallelization**: Using multiple workers to collect data simultaneously helps decorrelate the samples, which is a crucial stability requirement in deep learning.
    
3. **Generalized Advantage Estimation (GAE)**: Modern A2C often uses GAE to compute the advantage, which allows for a tunable trade-off between bias and variance using a $\lambda$ parameter.

# The A2C Algorithm and Loss Functions

The training loop for A2C involves collecting rollouts, computing advantages, and minimizing a combined loss function.

### The Combined Loss

The network is trained by minimizing the sum of the actor loss and the critic loss: $L_{total} = L_{actor}(\theta) + L_{critic}(w)$

1. **Actor Loss (Policy Gradient)**:
    
    $L_{actor}(\theta) = -\sum_{t=0}^{T-1} \log \pi_{\theta}(A_t|S_t)\delta_t^w$ 
    This encourages actions that lead to positive advantages (better than average) and discourages those with negative advantages.
    
2. **Critic Loss (Value Error)**:
    
    $L_{critic}(w) = \frac{1}{2T} \sum_{t=0}^{T-1} (v_w(S_t) - G_t)^2$ 
    This is a standard Mean Squared Error (MSE) loss that trains the critic to predict the actual returns-to-go $G_t$.
    
3. **Entropy Bonus (Optional)**: Many implementations add an entropy term $-\beta H(\pi_{\theta})$ to the actor loss. This penalizes the policy for becoming "too certain" too quickly, effectively forcing it to keep exploring different actions.
    

# On-Policy Nature and Data Disposal

A2C is an **on-policy** algorithm. This means that the data used to update the policy must be collected using that exact same policy.

### Why Data is Discarded

In A2C, the rollout (experience) collected during an iteration is discarded immediately after the policy update is performed.

- **Theoretical Constraint**: The Policy Gradient Theorem depends on samples drawn from the current policy distribution $\pi_{\theta}$.
    
- **The Risk of Old Data**: If we were to use data from a previous policy (off-policy data), the gradient would be "wrong" or biased because the actions in that data were taken by a different version of the agent.
    
- **Comparison**: This is the opposite of DQN, which uses a "Replay Buffer" to store and reuse old data multiple times (off-policy). While discarding data is less sample-efficient, it ensures that the training signals are always mathematically consistent with the current policy.

# A2C + GAE Integration

When A2C is implemented with [[Generalized Advantage Estimation (GAE)]], the actor's update rule is modified. Instead of using the 1-step TD error or a fixed n-step return, the gradient is scaled by the GAE value.

### The A2C-GAE Update Rule

The policy gradient becomes:

$\nabla_\theta J(\theta) \approx \mathbb{E} [ \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t|s_t) \hat{A}_t^{GAE(\gamma, \lambda)} ]$

In a typical deep RL implementation, the agent collects a segment of experience (e.g., 20 or 128 steps). The GAE values are calculated backward from the last state in the segment using the critic's value predictions. These values then serve as the "weights" for the log-probabilities in the loss function.

### Stability and Performance

A2C with GAE is significantly more stable than vanilla A2C. By reducing the variance of the advantage estimate, the policy updates become more consistent. Furthermore, GAE helps the agent deal with delayed rewards better than 1-step TD, as the $\lambda$ parameter allows reward signals to propagate backward through the trajectory more effectively.

# Comparison with Eligibility Traces

GAE is conceptually the "Policy Gradient version" of $TD(\lambda)$. Just as $TD(\lambda)$ uses eligibility traces to unify TD and MC for value estimation, GAE uses the $\lambda$-weighted average of residuals to unify TD and MC for advantage estimation. This makes it a standard component in virtually all state-of-the-art policy gradient algorithms, including PPO and TRPO.