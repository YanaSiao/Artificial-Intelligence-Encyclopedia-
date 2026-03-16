## Core Characteristics

Monte Carlo (MC) methods learn value functions and optimal policies directly from **episodes of experience**.

- **Model-Free:** MC does not require knowledge of the MDP's transitions $P(s'|s,a)$ or reward function $R(s,a)$. It only requires sequences of states, actions, and rewards.
    
- **Complete Episodes:** MC methods are only defined for episodic tasks. They require a terminal state to be reached because they update values based on the **Total Return** ($G_t$).
    
- **Unbiased Estimation:** Because MC uses the actual realization of the return, the estimates are unbiased ($E[V(s)] = v_\pi(s)$).
    
- **High Variance:** Since the return $G_t$ depends on many stochastic actions and transitions until the end of the episode, the variance is typically much higher than in DP or TD.

## Prediction: Estimating $V(s)$

The value of a state is the expected return. MC estimates this by averaging the returns observed after visiting that state.

- **The Return:** $G_t = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{T-1} R_T$.
    
- **Value Update:** $V(s) \approx \text{average}(G_t)$ for all episodes where $s$ was visited.

## First-Visit vs. Every-Visit MC

A state $s$ may occur multiple times within a single episode. There are two ways to handle this:

|**Feature**|**First-Visit MC**|**Every-Visit MC**|
|---|---|---|
|**Definition**|Only the return following the _first_ time $s$ is visited in an episode is recorded.|The returns following _every_ time $s$ is visited in an episode are recorded.|
|**Bias**|**Unbiased**. Each episode provides one independent sample.|**Biased** in finite samples, but consistent.|
|**Variance**|Higher (fewer samples per episode).|Lower (more samples, though they are correlated).|
|**Standard**|The most common version for theoretical proofs.|Often used in complex implementations for sample efficiency.|

**Which is better?**

In the RL literature, **First-visit MC** is the standard because it provides an unbiased estimate. However, **Every-visit MC** is consistent and often converges to the same value, making the choice often a matter of implementation preference or specific task dynamics.

![[Pasted image 20260314130215.png]]![[Pasted image 20260314130232.png]]
## Incremental Mean Update

To avoid storing all returns, the value function is updated incrementally. For each state $S_t$ with return $G_t$:

1. $N(S_t) \leftarrow N(S_t) + 1$
    
2. $V(S_t) \leftarrow V(S_t) + \frac{1}{N(S_t)} [G_t - V(S_t)]$

In non-stationary environments, we replace $\frac{1}{N(s)}$ with a constant step-size **$\alpha$**:

$$V(S_t) \leftarrow V(S_t) + \alpha [G_t - V(S_t)]$$

## MC Control: Policy Improvement

To perform control without a model, we must estimate **Action-Values** $Q(s,a)$ instead of $V(s)$.

- **The Policy:** $\pi(s) = \arg\max_a Q(s,a)$.
    
- **The Exploration Problem:** If a policy is deterministic, we might never explore certain actions.
    
- **Solutions:** 
	* **Exploring Starts:** Every $(s,a)$ pair has a non-zero probability of being the start of an episode.
    - **$\epsilon$-Greedy:** The agent takes the best action with probability $1-\epsilon$ and a random action with probability $\epsilon$.