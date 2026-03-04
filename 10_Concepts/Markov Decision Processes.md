### 1. The Markov Property

- **Definition:** The future is independent of the past given the present. $P(S_{t+1} | S_t) = P(S_{t+1} | S_1, ..., S_t)$.

- **Memoryless:** In a fully observable environment, the current state $S_t$ is a sufficient statistic.

- **Non-Markovian Processes:** In **Partially Observable** environments, the process appears non-Markovian, necessitating **Memory** (e.g., Belief States).

### 2. Markov Chains (Stochastic Processes)

A sequence of random variables with the Markov property.

- **Transition Matrix ($P$):** Encodings of probabilities where $P_{ij} = P(S_{t+1}=j | S_t=i)$.
    
    - **$N$-step Transitions:** Calculated as $P^N$.
        
    - **Distribution after $N$ steps:** $\mu^{(N)} = \mu^{(0)}P^N$.
        
- **State Classifications:**
    
    - **Absorbing:** Once entered, cannot be left ($P_{ii} = 1$).
        
    - **Recurrent:** 100% probability of returning to this state.
        
    - **Transient:** Probability of returning is $< 1$; will eventually be left for an absorbing state.
        
    - **Ergodic:** Recurrent + Non-periodic.
        

### 3. Mixing and Convergence

- **Steady State ($W$):** A stationary distribution where $W = WP$. Independent of the initial state in **Regular Chains**.
    
- **Mixing Rate:** How quickly the agent "forgets" the initial state.
    
    - **Spectral Gap ($\beta$):** $1 - |\lambda_2|$.
        
        - Large Gap $\to$ Rapid Mixing (fast forgetting).
            
        - Small Gap ($\epsilon$) $\to$ Slow Mixing.
            
- **Fundamental Matrix ($N$):** For absorbing chains, $N = (I - Q)^{-1}$. It represents the **expected number of visits** to transient states before absorption.