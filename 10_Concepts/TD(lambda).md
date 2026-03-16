## The $n$-step Return

Before defining TD($\lambda$), we define the **$n$-step return**, which sits between the 1-step TD target and the full MC return:

- **1-step (TD):** $G_t^{(1)} = R_{t+1} + \gamma V(S_{t+1})$
    
- **2-step:** $G_t^{(2)} = R_{t+1} + \gamma R_{t+2} + \gamma^2 V(S_{t+2})$
    
- **$n$-step:** $G_t^{(n)} = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{n-1} R_{t+n} + \gamma^n V(S_{t+n})$
    
- **$\infty$-step (MC):** $G_t$

## The $\lambda$-return (Forward View)

TD($\lambda$) averages all $n$-step returns using a weight $(1-\lambda)\lambda^{n-1}$.

- **The Formula:** $G_t^\lambda = (1-\lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_t^{(n)}$
    
- **The Weighting:** This is a geometric weighting. The factor $(1-\lambda)$ ensures the weights sum to 1.
    
- **The Extremes:**
    - If $\lambda = 0$: $G_t^\lambda = G_t^{(1)}$ (Standard **TD(0)**).
    - If $\lambda = 1$: $G_t^\lambda = G_t$ (Standard **Monte Carlo**).

## Eligibility Traces (Backward View)

The Forward View is theoretical because it requires looking into the future. The **Backward View** provides a causal, incremental algorithm using **Eligibility Traces** ($E_t$).

An eligibility trace $E_t(s)$ keeps track of which states were visited recently.

- **Update Rule for Trace:**
    
    $$E_t(s) = \gamma \lambda E_{t-1}(s) + \mathbb{1}(S_t = s)$$
    
- **Update Rule for Value:**
    
    $$V(s) \leftarrow V(s) + \alpha \delta_t E_t(s)$$
    
    where $\delta_t$ is the standard TD error: $R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$.
    
![[Pasted image 20260315134943.png]]
## Forward vs. Backward: The Equivalence Theorem

A central theoretical result in RL is that the Forward View and Backward View are equivalent under certain conditions:

- **Offline Updates:** If updates are made only at the end of an episode, the sum of offline TD updates using eligibility traces is exactly equal to the sum of offline $\lambda$-return updates.
    
- **Online Updates:** For online (step-by-step) updates, the views are approximately equivalent, though "True Online TD($\lambda$)" was later developed to make them exactly equivalent even during the episode.

## Why use TD($\lambda$)?

- **Credit Assignment:** If an agent receives a reward, eligibility traces allow that reward to be propagated back to all recently visited states, not just the single most recent state.
    
- **Bias-Variance Tuning:** By adjusting $\lambda$, you can choose a point on the spectrum that balances the low-bias of MC with the low-variance of TD.