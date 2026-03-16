### 1. Model-Based vs. Model-Free

The primary divider is whether the agent attempts to map the environment's dynamics ($T: S \times A \to S'$).

- **Model-Based:** Learns the transition and reward functions explicitly.
    - **Pros:** Sample efficient (uses experience better), allows for planning via Dynamic Programming.
    - **Cons:** High computational cost; model inaccuracies can lead to suboptimal performance.

- **Model-Free:** Skips the model and learns the solution (Value or Policy) directly.
    - **Pros:** Low memory/computation; simpler to implement.
    - **Cons:** Requires more interaction samples; no inherent exploration/exploitation guarantees.

---

### 2. The Learning Context (Online vs. Offline)

- **Online RL:** Learning happens incrementally while interacting with the environment. Traditional RL.
    
- **Offline RL (Batch RL):** Learning over a fixed dataset (e.g., **Fitted Q-Iteration**).
    - _Note:_ Avoids bootstrapping problems and works well with function approximation, but "Out-of-Distribution" actions remain a challenge.

---

### 3. Policy Update Logic (On-Policy vs. Off-Policy)

- **On-Policy (e.g., [[SARSA]]):** Learns the value of the policy being executed. Evaluation and improvement are simultaneous.
    
- **Off-Policy (e.g., [[Q-Learning]]):** Learns the optimal policy ($\pi^*$) regardless of what the agent is currently doing. Allows for aggressive exploration strategies.
![[Pasted image 20260208190840.png]]

### 5. Advanced Paradigms

#### Exploration Strategies

Crucial for efficient learning, especially in sparse environments:

- **R-max:** Optimistic initialization (assume unknown states are high reward).
- **$E^3$:** "Explicit Exploration, Exploitation."
- **Bayesian RL:** Maintains a distribution over models.

#### Hierarchical RL (HRL)

Solves the "flat action" problem by treating actions as hierarchies.

- **Formalism:** Semi-Markov Decision Processes (**SMDPs**).
- **Frameworks:** Options, HAMs, MAX-Q.
- **Sub-goal Discovery:** Automatically identifies "bottleneck" states to create reusable skills.

#### Transfer Learning

Aims to solve the "learn from scratch" problem by reusing knowledge (Values, Policies, or Samples).
- **Risk:** **Negative Transfer**, where old knowledge hinders performance on a new, dissimilar task.