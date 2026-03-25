### The Core Update Mechanism: Stochastic Gradient Descent (SGD)

In incremental methods, we do not store a table. Instead, we adjust a parameter vector $w$ to minimize the Mean Square Error (MSE) between our approximation $v_w(s)$ and the true value $v_\pi(s)$.

- **Gradient Descent Rule:** $w \leftarrow w + \Delta w$, where $\Delta w = -\alpha \nabla_w L(w)$.
    
- **SGD for VFA:** Since we cannot compute the full expectation over all states, we sample the gradient at each step:    $$\Delta w = \alpha (v_\pi(S) - v_w(S)) \nabla_w v_w(S)$$
- **Linear Case Advantage:** If $v_w(s) = x(s)^T w$, then $\nabla_w v_w(s) = x(s)$, leading to a very simple update: **Update = step-size × prediction error × feature value**.

### Incremental On-Policy Prediction

In RL, the "true" $v_\pi(S)$ is unknown, so we substitute it with a **target**.
![[Pasted image 20260210164028.png]]

#### The "Semi-Gradient" Nuance

Methods like TD(0) and TD($\lambda$) are called **semi-gradient methods**.

- **Why?** The target (e.g., $R_{t+1} + \gamma v_w(S_{t+1})$) depends on the current parameters $w$.
    
- **The Shortcut:** We only take the gradient of the _estimate_ $v_w(S_t)$, ignoring how changing $w$ would change the _target_.
    
- **Consequence:** This is not a true gradient of any objective function, but it is computationally efficient and often converges to a good solution (the TD fixed point).

### Incremental Off-Policy Prediction

Estimating $v_\pi(s)$ while following a behavior policy $b$.

- **Importance Sampling (IS):** We weight updates by the likelihood ratio $\rho_t = \frac{\pi(A_t|S_t)}{b(A_t|S_t)}$.
    
- **Challenge:** IS is unbiased but can suffer from **extremely high (or infinite) variance**, making learning unstable.

### Incremental Control (Action-Value)

To perform control, we approximate $q_w(s, a) \approx q_\pi(s, a)$.

- **Semi-Gradient SARSA (On-Policy):**
    
    $$\Delta w = \alpha (R_{t+1} + \gamma q_w(S_{t+1}, A_{t+1}) - q_w(S_t, A_t)) \nabla_w q_w(S_t, A_t)$$
    - _Note:_ Uses the action $A_{t+1}$ actually taken by the current policy.
    
- **Semi-Gradient Q-Learning (Off-Policy):**
    
    $$\Delta w = \alpha (R_{t+1} + \gamma \max_{a'} q_w(S_{t+1}, a') - q_w(S_t, A_t)) \nabla_w q_w(S_t, A_t)$$
    
    - _Note:_ Directly estimates $q_*$ by taking the maximum over possible next actions, regardless of the behavior policy.

### Eligibility Traces (Backward View)

For $TD(\lambda)$, the backward view allows us to update parameters incrementally without waiting for the end of an episode.

- **Eligibility Trace ($z_t$):** $z_t = \gamma \lambda z_{t-1} + \nabla_w v_w(S_t)$.
    
- **Update:** $\Delta w = \alpha \delta_t z_t$, where $\delta_t$ is the standard TD error.