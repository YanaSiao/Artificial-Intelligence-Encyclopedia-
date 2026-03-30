# The Challenge of Stability in Deep RL

Deep Learning assumes that data is independent and identically distributed (i.i.d.). Reinforcement Learning violates this because consecutive samples in a trajectory are highly correlated. Furthermore, because the policy is changing, the distribution of data and the "target" values are non-stationary. This leads to training instability where the network parameters can oscillate or diverge.

# Experience Replay

To address the correlation between samples, DQN uses a Replay Buffer. Instead of training on the most recent transition, the agent stores transitions $(s, a, r, s')$ in a large buffer and samples a random mini-batch for each training step.

- This decorrelates the data, making it more like the i.i.d. setting required for [[Backpropagation and Training Algorithms]].
    
- It allows for greater sample efficiency, as each transition can be used for multiple gradient updates.

# Fixed Target Networks

In standard Q-learning, the target $y = r + \gamma \max_{a'} Q(s', a'; \theta)$ depends on the same parameters $\theta$ that are being updated. This creates a "moving target" problem. DQN introduces a separate Target Network with parameters $\theta^-$.

- The loss function is defined as: $L(\theta) = \mathbb{E} [(r + \gamma \max_{a'} Q(s', a'; \theta^-) - Q(s, a, \theta))^2]$.
    
- The target parameters $\theta^-$ are kept frozen and only updated to match $\theta$ every $C$ steps. This provides a stable objective for the network to converge toward.

# Architecture and Pre-processing

DQN typically uses a CNN to process raw pixel inputs.

- The input is often a "stack" of the last 4 frames to capture motion (addressing the Markov property in visual environments).
    
- The final layer is a linear head that outputs a Q-value for every possible discrete action, similar to [[CNN Anatomy. The Classifier Head|The Classifier Head (Softmax)]] but without the softmax activation, as we need raw value estimates.