## Fundamental Comparison

While both methods learn from experience without a model, their update mechanisms lead to significantly different learning dynamics.

|**Feature**|**Monte Carlo (MC)**|**Temporal Difference (TD)**|
|---|---|---|
|**Update Goal**|Actual Return ($G_t$)|Estimated Return ($R_{t+1} + \gamma V(S_{t+1})$)|
|**Bootstrapping**|**No**: Learns from full episodes|**Yes**: Updates a guess based on a guess|
|**Bias**|**Zero**: The return is an unbiased estimate of $v_\pi$|**Some**: The TD target is biased unless $V$ is already optimal|
|**Variance**|**High**: Return depends on many random actions and transitions|**Low**: Target depends on only one random transition and reward|
|**Task Type**|Only **Episodic**|**Episodic or Continuing**|

---

## Convergence and "Batch" Learning

When learning from a fixed set of experience (Batch learning), MC and TD converge to different mathematical solutions:

- **Monte Carlo (Min MSE):** MC converges to the value function that minimizes the **Mean-Squared Error (MSE)** relative to the observed returns. It provides the best fit to the specific outcomes seen in the data.
    
- **Temporal Difference (Certainty Equivalence):** TD(0) converges to the solution of the **Maximum Likelihood Markov Model** that best fits the data. It implicitly builds a model of the transitions and rewards and finds the exact solution for that model.
    

**Example (The AB Problem):**

Imagine two states, A and B. In one episode, you see $A \to 0, B \to 0$. In seven other episodes, you see $B \to 1$.

- **MC** would say $V(A) = 0$, because the only time $A$ was seen, the return was 0.
    
- **TD** would say $V(A) = 0.75$, because it learned that $A$ always leads to $B$, and $B$ has a value of 0.75.

---

## Sensitivity to the Markov Property

The efficiency of these methods depends heavily on whether the environment satisfies the Markov property.

- **TD Exploits Markovian Structure:** Because TD uses bootstrapping (updating state values based on the values of subsequent states), it assumes that the current state is a sufficient statistic for the future. It is typically **more efficient** in Markovian environments.
    
- **MC Ignores Markovian Structure:** MC does not care about the relationship between states; it only cares about the final outcome (the return). This makes it **more robust** in non-Markovian environments where the current state does not fully capture the necessary information for prediction.

---

## Primary Use Cases

- **Use Monte Carlo when:**
    
    - The environment is non-Markovian or partially observable.
        
    - You have an episodic task and want to ensure zero bias.
        
    - You are using function approximation where bootstrapping can cause instability.
        
- **Use Temporal Difference when:**
    
    - The environment is highly Markovian (where exploiting this structure speeds up learning).
        
    - The tasks are continuing (never-ending) or very long, making waiting for the end of an episode impractical.
        
    - You need online updates for real-time learning.

## Bias-Variance Tradeoff: MC vs. TD vs. DP

Understanding the relationship between these three foundational methods is essential for navigating the RL landscape.

- **Monte Carlo (MC):** Deep backups (until end of episode), No bootstrapping, High Variance, Zero Bias.
    
- **Temporal Difference (TD):** Shallow backups (one step), Bootstraps, Low Variance, Some Bias.
    
- **Dynamic Programming (DP):** Shallow backups, Bootstraps, Zero Variance (deterministic backups), No Bias (requires a perfect model).
![[Pasted image 20260314132958.png]]