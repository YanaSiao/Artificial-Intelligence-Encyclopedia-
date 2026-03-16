Dynamic Programming (DP) refers to a collection of algorithms used to compute optimal policies given a perfect model of the environment as a Markov Decision Process (MDP).

### Core Assumptions

To apply DP, two environmental properties must hold:

1. **Optimal Substructure**: The optimal solution to the problem can be broken down into optimal solutions of its subproblems (provided by the Bellman Equation).
    
2. **Overlapping Subproblems**: Subproblems recur many times, allowing their solutions to be cached and reused (provided by the Value Function).

### DP vs. Brute Force Search

Solving an MDP means finding the best policy $\pi^*$.

- **Brute Force**: Involves enumerating all possible deterministic policies. The number of policies is $|\mathcal{A}|^{|\mathcal{S}|}$. This is exponential and computationally infeasible for even medium-sized problems.
    
- **Dynamic Programming**: Exploits the recursive nature of the Bellman equations to reduce complexity to polynomial time in the number of states and actions.

### Time Horizons

- **Finite Horizon**: The process ends after a fixed number of steps $T$. Solved via **Backward Induction**, starting from the final timestep and working toward $t=0$.
    
- **Infinite Horizon**: The process continues indefinitely. Convergence is guaranteed by the discount factor $\gamma < 1$, which ensures the total return remains bounded.

### Bellman Equations Comparison

It is crucial to distinguish between the two types of Bellman equations:

| **Feature**  | **Bellman Expectation Equation**            | **Bellman Optimality Equation**             |
| ------------ | ------------------------------------------- | ------------------------------------------- |
| **Purpose**  | Evaluates a _fixed_ policy $\pi$.           | Characterizes the _optimal_ policy $\pi^*$. |
| **Operator** | Linear.                                     | Non-linear (due to the `max` operator).     |
| **Use Case** | Used in Policy Evaluation.                  | Used in Value Iteration.                    |
| **Form**     | $V^\pi = \mathbb{E}_\pi [R + \gamma V^\pi]$ | $V^* = \max_a \mathbb{E} [R + \gamma V^*]$  |
|              |                                             |                                             |
**Bellman Expectation Equations**:

$$V^\pi(s) = \sum_{a \in \mathcal{A}} \pi(a|s) \left[ R(s,a) + \gamma \sum_{s' \in \mathcal{S}} P(s'|s,a) V^\pi(s') \right]$$

**Bellman Optimality Operator**:
$$V_{k+1}(s) = \max_{a \in \mathcal{A}} \left[ R(s,a) + \gamma \sum_{s' \in \mathcal{S}} P(s'|s,a) V_k(s') \right]$$
