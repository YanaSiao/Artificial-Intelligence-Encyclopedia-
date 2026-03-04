### 1. Motivation: Beyond Semi-Gradients

Standard TD methods (like Semi-gradient TD(0)) do not follow the gradient of any objective function, which is why they can diverge under the "Deadly Triad" conditions (Off-policy + Bootstrapping + VFA). Gradient TD methods were developed to be true gradient-descent algorithms that converge reliably even in off-policy linear settings.

### 2. The Right Objective: MSPBE

To achieve stability, these methods minimize the **Mean Square Projected Bellman Error (MSPBE)**:

$$PBE(w) = \left\| \Pi \delta_w \right\|^2_\mu$$

- **$\delta_w$**: The Bellman error.
    
- **$\Pi$**: The projection operator that maps the Bellman error back into the space representable by your features.
    
- **Intuition**: While we might not be able to zero out the Bellman error entirely, we minimize the part of it that our function approximator can actually "see".
    ![[Pasted image 20260210171237.png]]

### 3. Key Algorithms

These algorithms use a **two-time scale** architecture, maintaining a primary weight vector $w$ and an auxiliary weight vector $v$ to estimate the gradient of the MSPBE.

#### **Gradient TD 2 (GTD2)**

Designed to minimize the MSPBE directly using an auxiliary vector to avoid computing expensive matrix inverses.

- **Update for $w$**: $\Delta w = \alpha (x(S_t) - \gamma x(S_{t+1})) (x(S_t)^T v)$
    
- **Update for $v$**: $\Delta v = \beta (\delta_{w,t} - v^T x(S_t)) x(S_t)$
    

#### **Temporal Difference with Correction (TDC)**

Also known as Gradient-TD (0), this improves upon semi-gradient TD by adding a correction term that ensures convergence.

- **Update for $w$**: $\Delta w = \alpha \delta_{w,t} x(S_t) - \gamma x(S_{t+1}) (x(S_t)^T v)$
    
- **Update for $v$**: Identical to GTD2 (aims to estimate the quasi-gradient).
    
- **Note**: The term $\gamma x(S_{t+1})x(S_t)^T v$ is the "correction" that separates it from standard semi-gradient TD(0).
    

### 4. Convergence Properties

- **Two-Time Scales**: For these algorithms to converge, the auxiliary vector $v$ must learn faster than the primary vector $w$ (i.e., $\beta$ must decrease slower than $\alpha$ such that $\alpha/\beta \to 0$).
    
- **Stability**: Unlike semi-gradient TD, GTD2 and TDC are guaranteed to converge in the **Off-policy + Linear VFA** case.![[Pasted image 20260210170436.png]]
- 