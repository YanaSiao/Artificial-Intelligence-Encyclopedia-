**State Properties and Accessibility**

A state $j$ is **accessible** from state $i$ (written $i \to j$) if there is a non-zero probability of reaching $j$ from $i$ in some $n \ge 0$ steps. If $i \to j$ and $j \to i$, the states **communicate**. Communication is an equivalence relation that partitions the state space into **communicating classes**. A chain is **irreducible** if it has only one communicating class.

**Classification of States**

- **Absorbing State:** A state that, once entered, cannot be left ($p_{ii} = 1$).
    
- **Recurrent State:** A state where the probability of eventually returning is 1.
    
- **Positive Recurrent:** A recurrent state where the expected time to return is finite. In finite MDPs, all recurrent states are positive recurrent.
    
- **Transient State:** A state that is not recurrent; there is a non-zero probability the process will never return.
    
- **Periodic State:** A state that can only be returned to in multiples of $d$ steps (where $d > 1$ is the period). If $d=1$, the state is **aperiodic**.
    
- **Ergodic State:** A state that is both aperiodic and positive recurrent.

**Process Classifications**

- **Ergodic Process:** An irreducible and aperiodic chain. Every state is eventually visited, and the initial distribution is eventually forgotten.
    
- **Absorbing Process:** A process that has at least one absorbing state, and from every state, it is possible to reach an absorbing state.
    
- **Regular Process:** A process where some power of the transition matrix $P^k$ has only strictly positive entries. All regular processes are ergodic, but not all ergodic processes are regular (though in finite spaces, they are often used interchangeably).

**Convergence to Steady State**

The distribution $\mu^{(n)}$ converges to a unique **steady-state distribution** $W$ (or $\pi$) regardless of the initial $\mu^{(0)}$ if and only if the chain is **Regular**.

- **Condition:** The chain must be irreducible (one class) and aperiodic (no cycles). If the chain is periodic, it may have a stationary distribution, but $\mu^{(n)}$ will oscillate and not converge to it.
    

**Theorems of Stationary Distributions**

- **Existence:** Every finite Markov chain has at least one stationary distribution $\pi$ such that $\pi P = \pi$.
    
- **Uniqueness:** If the Markov chain is irreducible, the stationary distribution $\pi$ is unique. For regular chains, $\pi_j = 1/E[T_j]$, where $E[T_j]$ is the mean recurrence time.

**Canonical Form and the Fundamental Matrix**

For an absorbing Markov chain, we reorder states to group transient states ($TR$) and absorbing states ($ABS$). The transition matrix $P$ takes the **Canonical Form**:

$$P = \begin{pmatrix} Q & R \\ 0 & I \end{pmatrix}$$

Where $Q$ describes transitions between transient states and $R$ describes transitions from transient to absorbing states.

- **Fundamental Matrix ($N$):** Defined as $N = (I - Q)^{-1} = I + Q + Q^2 + \dots$.
    
- **Theorem:** The entry $n_{ij}$ of $N$ is the expected number of times the process is in transient state $j$, given it started in transient state $i$. This is used to calculate the **time to absorption** and the probability of ending in a specific absorbing state.


**Time Reversibility**

A Markov chain is **time-reversible** if it satisfies the **detailed balance equations**:

$$\pi_i p_{ij} = \pi_j p_{ji}$$

If a distribution $\pi$ satisfies this, it is guaranteed to be a stationary distribution. Reversibility implies that the process looks the same whether time runs forward or backward.

**Mixing Rate and Mixing Time**

- **Mixing Rate:** The speed at which $\mu^{(n)}$ approaches the stationary distribution $\pi$.
    
- **Mixing Time ($t_{mix}$):** The minimum number of steps required for the total variation distance between $\mu^{(n)}$ and $\pi$ to be less than $\epsilon$.
    
- **Rapidly Mixing:** A chain where the mixing time is polylogarithmic in the number of states.


**Spectral Gap and Alon-Sinclair Theorem**

The eigenvalues of a transition matrix $P$ are $1 = \lambda_1 > \lambda_2 \ge \dots \ge \lambda_n > -1$.

- **Spectral Gap ($\gamma$):** Defined as $1 - \lambda_2$ (or more accurately, $1 - \max(|\lambda_2|, |\lambda_n|)$).
    
- **Connection:** The larger the spectral gap, the faster the chain mixes. Mixing time is inversely proportional to the spectral gap.
    
- **Alon-Sinclair Theorem:** Relates the spectral gap to the **conductance** ($\Phi$) of the graph (a measure of how "bottlenecked" the state space is). It states:
    
    $$\frac{\Phi^2}{2} \le \gamma \le 2\Phi$$
    
    This is used in RL to understand how difficult it is for an agent to explore a state space. If there is a bottleneck (low conductance), the spectral gap is small, and the agent will take a very long time to reach a steady-state distribution of exploration.