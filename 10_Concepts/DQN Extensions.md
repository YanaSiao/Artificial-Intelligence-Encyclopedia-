# Double DQN (DDQN)

Standard DQN tends to overestimate action values because the $\max$ operator uses the same values to both select and evaluate an action. DDQN decouples these:

- Use the online network $\theta$ to select the best action $a^* = \arg\max_a Q(s', a; \theta)$.
    
- Use the target network $\theta^-$ to evaluate the value of that action: $y = r + \gamma Q(s', a^*; \theta^-)$.
    
    This significantly reduces overestimation bias and leads to more stable learning.

# Dueling Network Architecture

This architecture splits the network into two streams after the convolutional layers:

- State-Value Stream: $V(s)$, estimating the value of being in a state.
    
- Advantage Stream: $A(s, a)$, estimating the relative benefit of each action.
    
    The streams are combined at the output: $Q(s, a) = V(s) + (A(s, a) - \frac{1}{|\mathcal{A}|}\sum_{a'} A(s, a'))$.
    
    This allows the agent to learn which states are valuable without having to learn the effect of every action in every state, which is especially useful when many actions have the same effect.
    

# Prioritized Experience Replay (PER)

Instead of sampling uniformly from the replay buffer, PER samples transitions based on their TD error. Transitions where the agent's prediction was highly inaccurate are sampled more frequently, focusing the learning on the most "informative" data points.