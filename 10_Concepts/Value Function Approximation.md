### The Scaling Problem

Tabular representations (Lookup Tables) fail in real-world environments for two primary reasons:

- **Memory Constraints**: Large or infinite state spaces $|\mathcal{S}| [cite_start]= \infty$ (e.g., Go with $10^{170}$ states) cannot be stored.
- **Learning Speed**: It is too slow to learn the value of every state individually.
    
- **Solution**: Estimate the value function with parameters $w \in \mathbb{R}^n$ to **generalize** from seen states to unseen ones.
    
    - $v_w(s) \approx v_{\pi}(s)$
        
    - $q_w(s,a) \approx q_{\pi}(s,a)$

### General Form & Objective Function

To find the best parameter vector $w$, we minimize the **Mean Square Error (MSE)** between the approximate and true value function:

$$L(w) = \frac{1}{2}\mathbb{E}_{S \sim d_{\pi}}\left[ (v_{\pi}(S) - v_w(S))^2 \right]$$

- $d_{\pi}$ is the stationary state distribution under policy $\pi$.
    
- The goal is to reach a local (or global) minimum via **Stochastic Gradient Descent (SGD)**.

### Linear Value Function Approximation

A common choice where the value function is a linear combination of features:

$$v_w(s) = x(s)^T w = \sum_{j=1}^n x_j(s)w_j$$

- **Properties**:
    
    - The objective function is quadratic, meaning SGD converges to a **global optimum**.
        
    - The gradient is simply the feature vector: $\nabla_w v_w(s) = x(s)$.
        
- **The Update Rule**: $\Delta w = \alpha (v_{\pi}(s) - v_w(s))x(s)$.
        
    - _Interpretation_: Update = Step-size × Prediction Error × Feature Value.

### Feature Engineering

The quality of approximation depends heavily on the feature vector $x(s)$.

- **Table Lookup**: A special case where $x(s)$ is a one-hot vector (1 if $s=s_i$, 0 otherwise).
    
- **Coarse Coding**: Large overlapping features provide a distributed representation.
    
- **Tile Coding**: Space is partitioned into multiple overlapping grids (tilings). It is computationally efficient because only a constant number of binary features are active at once.
    
- **Radial Basis Functions (RBFs)**: Continuous features (e.g., Gaussians) that measure the distance to specific centers: $x_i(s) = \exp\left(-\frac{||s-c_i||^2}{2\sigma_i^2}\right)$.
    

### Challenges vs. Supervised Learning

Unlike standard regression, RL training is difficult because:

1. Non-i.i.d. Data: Successive timesteps are statistically dependent.
    
2. **Non-Stationarity**: The agent's actions affect future data, and regression targets (like TD targets) change as $w$ is updated.