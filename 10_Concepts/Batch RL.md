### 1. Motivation: Why Batch?

While incremental (online) methods are simple, they are often **not sample efficient**.

- **Incremental**: Processes each sample once and discards it.
    
- **Batch**: Collects experience $D = \{(S_1, A_1, R_2, S_2), \dots \}$ and finds the parameter vector $w$ that minimizes the global error over this "batch".
    

### 2. Batch Prediction (Evaluating $\pi$)

The goal is to minimize the **Mean-Squared Error (MSE)** between our approximation $v_w(S_t)$ and a target value $\tilde{G}_t$ across all time steps in the batch:

$$L(w) = \frac{1}{T} \sum_{t=1}^{T} (\tilde{G}_t - v_w(S_t))^2$$

**Linear Least Squares Solutions**

Unlike incremental methods that use gradients, linear batch methods often have a **closed-form solution** for the fixed point:

- **LSMC (Least Squares Monte-Carlo)**:
    
    - **Target**: Actual returns $G_t$.
        
    - **Solution**: $\hat{w}_{MC} = \left( \sum_{t=1}^{T} x(S_t)x(S_t)^T \right)^{-1} \sum_{t=1}^{T} x(S_t)G_t$.
        
- **LSTD (Least Squares Temporal Difference)**:
    
    - **Target**: TD targets $R_{t+1} + \gamma v_w(S_{t+1})$.
        
    - **Solution**: $\hat{w}_{TD} = \left( \sum_{t=1}^{T} x(S_t)(x(S_t) - \gamma x(S_{t+1}))^T \right)^{-1} \sum_{t=1}^{T} x(S_t)R_{t+1}$.
        
- **LSTD($\lambda$)**: Uses eligibility traces $z_t$ and returns to solve for the TD($\lambda$) fixed point.
    

**Key Property**: Batch prediction methods (LSMC, LSTD) are generally **stable** and converge for both on-policy and off-policy data in the linear case.

### 3. Batch Control (Finding $\pi^*$)

Batch control typically follows one of two frameworks:

#### A.

Least Squares Policy Iteration (LSPI)

LSPI implements **Approximate Policy Iteration (API)**.

1. **Policy Evaluation**: Uses **LSTDQ** to learn the action-value function $q_w(s, a)$ for the current policy $\pi$.
    
2. **Policy Improvement**: Updates $\pi$ to be greedy with respect to the new $q_w$.
    
3. **Repeat**: Continues until the policy converges.
    

- **Stability**: It is remarkably robust and converges in the linear case.

#### B.

Fitted Q-Iteration (FQI)

FQI implements **Approximate Value Iteration (AVI)**. It treats RL as a sequence of supervised learning regression problems.

- **Process**:
    
    1. Start with a dataset $D = \{(S_i, A_i, R_{i+1}, S'_i)\}$.
        
    2. At each iteration $i$, create a training set where the target is $R_{i+1} + \gamma \max_{a'} \hat{q}_{i-1}(S'_i, a')$.
        
    3. Train a regression model (e.g., Random Forests, Neural Networks, ExtraTrees) to predict these targets.
        
- **Flexibility**: Unlike LSTD, FQI can use **non-linear approximators** like decision trees or deep networks.