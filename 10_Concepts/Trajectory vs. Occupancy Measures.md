### The Trajectory View (Path-Based)

In the trajectory view, we treat the RL process as a sequence of random variables. A trajectory $\tau$ is a sequence of states and actions $(s_0, a_0, s_1, a_1, \dots, s_T)$. We view the performance of a policy $\pi$ as the expected return over the distribution of all possible trajectories.

The probability of a specific trajectory under a policy $\pi$ is:

$$P(\tau|\pi) = \mu(s_0) \prod_{t=0}^{T-1} \pi(a_t|s_t) P(s_{t+1}|s_t, a_t)$$

This view is the foundation of **Monte Carlo** methods like REINFORCE. It treats the episode as a single data point. The primary challenge here is high variance: because a trajectory depends on many stochastic decisions and transitions, two trajectories from the same policy can result in wildly different returns.

---

### The Occupancy View (Distribution-Based)

The occupancy view shifts focus away from the _order_ of events to the _density_ of events. Instead of asking "What path did the agent take?", we ask "How much time does the agent spend in state $s$ taking action $a$?"

We define the **discounted state-action occupancy measure** $d^\pi(s, a)$ as:

$$d^\pi(s, a) = (1-\gamma) \sum_{t=0}^{\infty} \gamma^t P(S_t = s, A_t = a | \pi)$$

This represents the total discounted probability of visiting $(s, a)$. It effectively "flattens" the temporal structure of the MDP into a single distribution. If you know the occupancy measure, you don't need to know the trajectories to calculate the expected reward.

---

### The Equivalence Principle

The "Magic" of RL theory is that these two views are mathematically dual. The expected return $J(\pi)$ can be calculated in two equivalent ways:

**Expectation over Trajectories:**

$$J(\pi) = \mathbb{E}_{\tau \sim P(\cdot|\pi)} \left[ \sum_{t=0}^{T} \gamma^t R(s_t, a_t) \right]$$

**Expectation over Occupancy:**

$$J(\pi) = \frac{1}{1-\gamma} \sum_{s \in \mathcal{S}} \sum_{a \in \mathcal{A}} d^\pi(s, a) R(s, a)$$

This equivalence allows us to derive the **Policy Gradient Theorem**. It proves that we can find the gradient of the performance by looking at the average behavior ($d^\pi$) rather than having to differentiate the complex probability of every possible path.
![[Pasted image 20260323130955.png]]
![[Pasted image 20260323131010.png]]

---

### Applications and Implications

**Variance Reduction:** By moving from trajectories to occupancy (and eventually to Value Functions), we use the Markov property to average out the noise of individual paths. This is why Actor-Critic methods are generally more stable than pure REINFORCE.

**Inverse Reinforcement Learning (IRL):** In IRL, we often try to match the "Feature Expectations" of an expert. This is entirely an occupancy-view task—we want the agent's $d^\pi$ to match the expert's $d^{\pi_{E}}$.

**Policy Optimization:** This duality is the basis of **Dual RL** algorithms, where we optimize the occupancy measure directly as a linear programming problem, bypassing the policy parameters entirely in the initial stages.