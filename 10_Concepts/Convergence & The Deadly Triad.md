#### 1. The Stability Problem

In the tabular case, most algorithms are guaranteed to converge. In VFA, we lose these guarantees.

- **Prediction (Fixed $\pi$):** 
	* **On-policy (TD):** Converges to a "TD fixed point" $w_\infty$ which is near the optimal solution.
    - **Off-policy (TD):** Can diverge (parameters go to $\infty$).
- **Control (Optimizing $\pi$):** Often "chatter" or oscillate near a good policy but rarely converge to the exact $\pi^*$.

#### 2. The "Deadly Triad"

According to the lecture (and Sutton & Barto), divergence is likely to occur when these three components are combined:

1. **Function Approximation**: Using $v_w$ instead of a table (generalizing across states).
    
2. **Bootstrapping**: Using targets that involve existing estimates (TD, DP) instead of actual returns (MC).
    
3. **Off-policy Learning**: Learning about policy $\pi$ while following policy $b$.
    

**Why it happens:** In off-policy learning, the state distribution $d_b$ (behavior) doesn't match the distribution $d_\pi$ (target). This can cause the Bellman operator to no longer be a **contraction mapping** in the space of representable functions.

#### 3. Geometric Interpretation

When we use VFA, we are essentially "projecting" the true value function onto a smaller manifold (the space of our features).

- **The Projection Operator ($\Pi$):** Maps any value function to the closest representable function in our feature space.
    
- **The Stability Issue:** While the Bellman Operator $T^\pi$ is a contraction, the _composed_ operator $\Pi T^\pi$ (Bellman update + Projection) is **not necessarily a contraction**. If it isn't, the values can grow infinitely.
    

#### 4. Convergence Table (The "Status Quo")
Prediction: ![[Pasted image 20260210164904.png]]
Control: ![[Pasted image 20260210164920.png]]
