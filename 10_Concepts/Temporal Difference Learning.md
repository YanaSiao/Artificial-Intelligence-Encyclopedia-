## Core Concept: Bootstrapping from Experience

Temporal Difference learning is a **model-free** approach that learns directly from raw experience. Unlike Monte Carlo methods, which must wait until the end of an episode to update a value, TD methods update estimates based in part on other learned estimates, without waiting for a final outcome. This process is known as **bootstrapping**.

## The TD(0) Algorithm

The simplest form of TD is **TD(0)**. At each time step $t$, the agent transitions from state $S_t$ to $S_{t+1}$ by taking action $A_t$ and receiving reward $R_{t+1}$. It then immediately updates its estimate of $V(S_t)$.

**The TD Update Rule:**

$$V(S_t) \leftarrow V(S_t) + \alpha [R_{t+1} + \gamma V(S_{t+1}) - V(S_t)]$$

**The TD Target:**

The quantity $R_{t+1} + \gamma V(S_{t+1})$ is called the **TD Target**. It is an estimate of the return $G_t$. While the Monte Carlo target is the actual return (unbiased), the TD target is biased because it uses the current (potentially incorrect) estimate of $V(S_{t+1})$.

**The TD Error:**

The difference between the TD target and the current estimate is the **TD Error** ($\delta_t$):

$$\delta_t = R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$$

This error represents the "surprise" or the discrepancy between what the agent expected and what it actually observed after one step.

## Advantages of TD Learning

TD methods offer several practical advantages over Monte Carlo:

- **Online/Incremental:** Updates happen at every step. This allows for learning in **continuing tasks** (non-episodic) and provides faster feedback during learning.
    
- **Low Variance:** Because the update only depends on one immediate reward and the next state's value (rather than a long sequence of random rewards), the variance is significantly lower than in MC.
    
- **Convergence:** TD(0) is guaranteed to converge to $v_\pi$ for any fixed policy $\pi$ given a sufficiently small step-size $\alpha$ and enough exploration.

