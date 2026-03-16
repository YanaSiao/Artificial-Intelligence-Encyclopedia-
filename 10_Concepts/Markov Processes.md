#### Stochastic Processes & The Markov Property

A **Stochastic Process** is an indexed collection of random variables $\{X_t\}_{t \in T}$.

- **The Markov Property (Memoryless):** The future is independent of the past given the present.
    $$P(X_{t+1} = j \mid X_t = i, X_{t-1} = i_{t-1}, \dots, X_0 = i_0) = P(X_{t+1} = j \mid X_t = i)$$
- **Intuition:** The current state $X_t$ contains all the information needed to predict $X_{t+1}$. If an environment is **Partially Observable**, it appears non-Markovian unless we incorporate memory into the state.

#### The Transition Matrix ($P$)

For a finite state space, we define the probability of moving from state $i$ to state $j$ as $p_{ij}$. The matrix $P$ collects these:

$$P = \begin{pmatrix} p_{11} & p_{12} & \dots \\ p_{21} & p_{22} & \dots \\ \vdots & \vdots & \ddots \end{pmatrix}$$

- **Property:** Each row sums to 1 (it is a stochastic matrix).
- **Stationary Transitions:** If $P$ does not change over time, the process is time-homogeneous.

#### State Distribution Evolution

If $\mu^{(0)}$ is the initial probability distribution over states (a row vector), the distribution after $n$ steps is:

$$\mu^{(n)} = \mu^{(0)}P^n$$

#### Chapman-Kolmogorov Equations

This provides the method for calculating $n$-step transition probabilities.

- **Formula:** $P(X_{t+n} = j \mid X_t = i) = (P^n)_{ij}$
    
- **The Identity:** $p_{ij}^{(n+m)} = \sum_{k \in S} p_{ik}^{(n)} p_{kj}^{(m)}$
    
- **Intuition:** To go from $i$ to $j$ in $(n+m)$ steps, you must pass through some intermediate state $k$ at step $n$.

#### Recurrence and First Passage

To understand long-term behavior, we look at how often a process returns to a state.

- **First Passage Probability ($f_{ij}^{(n)}$):** The probability that the _first_ time the process reaches state $j$ starting from $i$ is exactly at step $n$.
    
- **Recurrence Time ($T_i$):** A random variable representing the number of steps until the process returns to state $i$ for the first time ($T_i = \min \{n \geq 1 : X_n = i \mid X_0 = i\}$).
    
- **Recurrent State:** A state $i$ is recurrent if the probability of eventually returning to it is 1 ($\sum_{n=1}^{\infty} f_{ii}^{(n)} = 1$).
    
    - If it is not recurrent, it is **Transient** (there is a non-zero probability you will never return).