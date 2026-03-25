### Functional Roles: The Actor and The Critic

Actor-Critic methods split the learning task into two distinct components that interact in a closed loop. This architecture combines the advantages of policy-based and value-based reinforcement learning.

**The Actor**

The Actor represents the policy $\pi_\theta(a|s)$. Its sole responsibility is to map states to action probabilities. It is the "performer" that interacts with the environment. The Actor is updated using policy gradient theorems, moving the parameters $\theta$ in a direction that increases the probability of actions that receive a positive critique.

**The Critic**

The Critic represents a value function estimator, typically $V_w(s)$ or $Q_w(s,a)$, parameterized by $w$. Its responsibility is to evaluate the actions taken by the Actor. Instead of waiting for a full return from the environment, the Critic provides a "functional" estimate of how good the current state or state-action pair is. It essentially "filters" the rewards into a more stable learning signal.

---

### The Mechanism of Interaction

The fundamental difference between Actor-Critic and pure policy gradient methods (like REINFORCE) is **bootstrapping**. In Actor-Critic, the update for the Actor is driven by the Critic's estimate rather than the actual Monte Carlo return.

Most modern Actor-Critic algorithms use the **TD Error** ($\delta$) as the critique:

$$\delta_t = R_{t+1} + \gamma V_w(S_{t+1}) - V_w(S_t)$$

The Actor update then becomes:

$$\theta \leftarrow \theta + \alpha_\theta \nabla_\theta \log \pi_\theta(a_t|s_t) \delta_t$$

Here, $\delta_t$ acts as an estimate of the **Advantage Function** $A(s,a)$. If $\delta_t > 0$, the action was better than average, and the Actor increases its probability. If $\delta_t < 0$, the probability is decreased.

---

### The Bias-Variance Tradeoff

The transition from REINFORCE to Actor-Critic is essentially a deliberate trade of bias for variance.

**Variance (The Actor-Critic Strength)**

REINFORCE and G(PO)MDP use Monte Carlo returns ($G_t$). Because $G_t$ is a sum of many stochastic rewards over a long horizon, it has extremely high variance. Actor-Critic replaces the full return with a 1-step or $n$-step TD target. This bootstrapping significantly reduces variance because the update only depends on the immediate reward and the next state's value, "smoothing out" the noise of the rest of the trajectory.

**Bias (The Actor-Critic Weakness)**

Because the Critic is a function approximator that is also being learned, its estimates are initially incorrect. Using an incorrect value estimate to update the Actor introduces **bias** into the gradient. If the Critic is poorly trained, the Actor may converge to a sub-optimal policy or diverge entirely.

---

### Why Actor-Critic Outperforms REINFORCE and G(PO)MDP

**Sample Efficiency**

REINFORCE must wait until the end of an episode to perform a single update. Actor-Critic can perform updates at every time step (online). This allows the agent to learn much faster from its interactions, especially in environments with long or infinite horizons.

**Stability and Continuity**

The reduced variance of the TD signal leads to more stable policy updates. Furthermore, REINFORCE is strictly limited to episodic tasks. Actor-Critic, through the use of the Critic and the average reward formulation, can be applied to **continuing tasks** where there is no terminal state.

**Structural Credit Assignment**

G(PO)MDP improves credit assignment by ensuring rewards only affect prior actions. Actor-Critic takes this further by using the Value Function to provide a "baseline" for every state. This tells the Actor not just "this path was good," but "this specific action was better than what I usually do in this state," which is a much more precise learning signal.

---

### When This Combination Works Well

Actor-Critic is the preferred choice for most complex, high-dimensional RL problems, but it excels specifically in the following scenarios:

**Continuous Action Spaces:** When the action space is infinite (e.g., controlling the torque of a robot arm), calculating a `max` for Q-learning is impossible. Actor-Critic handles this by having the Actor output the parameters of a continuous distribution (like a Gaussian).

**Large State Spaces:** When combined with Deep Learning (Deep Actor-Critic), the Critic can generalize value estimates across similar states, allowing the Actor to make informed decisions even in states it has never seen before.

**Compatible Function Approximation:** The combination works optimally when the Critic's features are "compatible" with the Actor's policy. Specifically, if the Critic is a linear function of the score function $\nabla_\theta \log \pi_\theta(a|s)$, the bias introduced by the Critic can be eliminated while still maintaining the low-variance benefits.