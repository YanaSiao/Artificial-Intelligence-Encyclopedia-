# The Motivation for GAE

In Actor-Critic methods like [[Deep Actor-Critic|A2C]], the quality of the policy gradient depends heavily on the accuracy of the advantage estimate $\hat{A}_t$. We face a fundamental trade-off: 1-step TD residuals have low variance but high bias (due to the imperfect critic), while Monte Carlo returns have zero bias but high variance (due to the accumulation of rewards over long trajectories). GAE was introduced to provide a tunable way to navigate this trade-off using an exponential moving average of n-step advantage estimators.

# Mathematical Formulation

GAE is built upon the TD residual $\delta_t^V = r_t + \gamma V(s_{t+1}) - V(s_t)$. We can define different k-step advantage estimators:

- 1-step: $\hat{A}_t^{(1)} = \delta_t^V$
    
- 2-step: $\hat{A}_t^{(2)} = \delta_t^V + \gamma \delta_{t+1}^V$
    
- $\infty$-step: $\hat{A}_t^{(\infty)} = \sum_{l=0}^{\infty} \gamma^l \delta_{t+l}^V = \sum_{l=0}^{\infty} \gamma^l r_{t+l} - V(s_t)$
    

GAE($\gamma, \lambda$) is defined as the exponentially weighted average of these k-step estimators:

$\hat{A}_t^{GAE(\gamma, \lambda)} = (1-\lambda) (\hat{A}_t^{(1)} + \lambda \hat{A}_t^{(2)} + \lambda^2 \hat{A}_t^{(3)} + \dots)$

This simplifies to a highly elegant recursive form:

$\hat{A}_t^{GAE(\gamma, \lambda)} = \sum_{l=0}^{\infty} (\gamma \lambda)^l \delta_{t+l}^V$

# The Role of the Lambda Parameter

The hyperparameter $\lambda \in [0, 1]$ controls the bias-variance exchange:

- If $\lambda = 0$: We get $\hat{A}_t = \delta_t^V$. This is the 1-step TD return. It has the lowest variance but highest bias because it relies heavily on the critic's value estimate.
    
- If $\lambda = 1$: We get $\hat{A}_t = \sum_{l=0}^{\infty} \gamma^l r_{t+l} - V(s_t)$. This is equivalent to the Monte Carlo return minus a baseline. It has zero bias (assuming an infinite horizon) but very high variance.
    
- Intermediate values ($0 < \lambda < 1$) allow the agent to benefit from some bootstrapping (reducing variance) while looking further into the future (reducing bias).