Policy Iteration is an algorithm that generates a sequence of monotonically improving policies:

$$\pi_0 \xrightarrow{E} V^{\pi_0} \xrightarrow{I} \pi_1 \xrightarrow{E} V^{\pi_1} \xrightarrow{I} \dots \xrightarrow{I} \pi^*$$

Where **E** denotes Policy Evaluation and **I** denotes Policy Improvement.

### Policy Evaluation

The goal is to compute the state-value function $V^\pi$ for the current policy $\pi$. This is done by solving the system of **Bellman Expectation Equations**:

$$V^\pi(s) = \sum_{a \in \mathcal{A}} \pi(a|s) \left[ R(s,a) + \gamma \sum_{s' \in \mathcal{S}} P(s'|s,a) V^\pi(s') \right]$$

- **Method**: In practice, we use **Iterative Policy Evaluation**. We turn the equation into an update rule:
    
    $$V_{k+1}(s) \leftarrow \mathbb{E}_\pi [R_{t+1} + \gamma V_k(S_{t+1}) | S_t = s]$$
    
- **Full Backup**: This is a "full backup" because it considers every possible next state $s'$ and every possible action $a$ under the current policy, weighted by their probabilities.

### Policy Improvement

Once $V^\pi$ is known, we look for a better policy by acting greedily. We calculate the action-value function $Q^\pi(s,a)$:

$$Q^\pi(s,a) = R(s,a) + \gamma \sum_{s' \in \mathcal{S}} P(s'|s,a) V^\pi(s')$$

We create a new policy $\pi'$ such that:

$$\pi'(s) = \arg\max_{a \in \mathcal{A}} Q^\pi(s,a)$$

### Policy Improvement Theorem

The theorem guarantees that if we change a policy $\pi$ to a greedy policy $\pi'$, then the new policy is globally as good as or better than the old one:

$$V^{\pi'}(s) \ge V^\pi(s), \forall s \in \mathcal{S}$$

- If the improvement is strict ($V^{\pi'}(s) > V^\pi(s)$ for at least one state), then $\pi$ was not optimal.
    
- If the value function stops changing ($V^{\pi'} = V^\pi$), the **Bellman Optimality Equation** has been satisfied.
![[Pasted image 20260309185644.png]]
### Stopping Condition

Policy Iteration terminates when the policy becomes **stable**.

- **Condition**: $\pi_{k} = \pi_{k-1}$ (the greedy policy no longer changes).
    
- Since the number of deterministic policies is finite ($|\mathcal{A}|^{|\mathcal{S}|}$) and each step strictly improves the value function (until the optimum), the algorithm is guaranteed to converge in a finite number of iterations.