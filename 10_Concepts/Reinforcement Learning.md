### Core 

Reinforcement Learning is a paradigm of machine learning focused on **Control** and **Prediction**. Unlike Unsupervised Learning (Clustering, Dimensionality Reduction), RL is driven by a reward signal.

- **Key Trade-off:** RL is characterized by **slow learning**, which is computationally compensated for by high-volume **iteration**.

### Properties of the RL Problem

- **Delayed Reward:** Unlike supervised learning, the "label" (reward) might not appear until many steps after the critical action (delayed gratification).
    
- **Observability:** 
	* **Full:** Environment state ($S_t$) is fully visible to the agent.
    - **Partial:** Agent receives an observation ($O_t$) which is a subset or noisy version of the state.

- **State/Action Space:** Can be **Finite** (Discrete) or **Infinite** (Continuous).

- **Dynamics:**
    - **Deterministic:** $P(s' | s, a) = 1$. The next state is certain.
    - **Stochastic:** Transitions involve a probability distribution.

- **Stationarity:** 
	* **Stationary:** Transition probabilities $P(s'|s, a)$ remain constant over time.
    - **Non-Stationary:** Dynamics change over time (common in real-world or multi-agent systems).

- **Agent Count:** 
	* **Single-Agent:** Environment is passive (automata).
    - **Multi-Agent:** Environment contains other strategic actors (Game Theory).
---

## RL History and Milestones

| **Year** | **Milestone**         | **Significance**                                        |
| -------- | --------------------- | ------------------------------------------------------- |
| **2013** | **Atari Games (DQN)** | First major Deep RL (End-to-End) success.               |
| **2016** | **AlphaGo**           | Defeated world champion; combined Tree Search with RL.  |
| **2018** | **Learning to Run**   | Advances in continuous control and locomotion.          |
| **2019** | **AlphaStar**         | Mastered StarCraft II; complex multi-agent/hidden info. |
| **2022** | **ChatGPT (RLHF)**    | Use of RL from Human Feedback to align LLMs.            |
