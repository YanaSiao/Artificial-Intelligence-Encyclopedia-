### The Core Theory

Logistic Regression is a **Discriminative Classifier** that models the probability of a binary response. Unlike Linear Regression, which predicts continuous values, Logistic Regression uses the **Sigmoid (Logistic) function** to "squash" any real-valued number into the range $[0, 1]$.

**The Sigmoid Function:**
$$\sigma(z) = \frac{1}{1 + e^{-z}}$$

### Geometric Interpretation: The Hyperplane

In a feature space, Logistic Regression defines a **Decision Boundary** in the form of a hyperplane.

- **Linear Combination:** The model calculates $z = w^T x + b$ (where $w$ are weights and $b$ is the bias).
- **The Boundary:** The hyperplane is the set of points where $w^T x + b = 0$.
- **Classification:** 
	* If $z > 0$, the point is on one side of the hyperplane ($P > 0.5$).
    - If $z < 0$, the point is on the other side ($P < 0.5$).
- **Distance:** The magnitude of $z$ represents the distance from the hyperplane, which correlates to the model's confidence in its prediction.

### Loss Function: Cross-Entropy

Since we are dealing with probabilities, we don't use Mean Squared Error. Instead, we use **Log Loss** (also known as **Binary Cross-Entropy**).

- **Objective:** To penalize the model heavily when it is confident and wrong.
- **Formula:**
    $$J(w) = -\frac{1}{m} \sum_{i=1}^{m} [y^{(i)} \log(\hat{y}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{y}^{(i)})]$$
- **Optimization:** This loss function is **convex**, meaning we can use **Gradient Descent** to find the global minimum for the weights $w$.

### Regularization: Preventing Overfitting

In NLP, because our Bag-of-Words vectors are so high-dimensional, Logistic Regression will easily overfit. We add a penalty term to the loss function:

- **L2 Regularization (Ridge):** Adds $\lambda ||w||^2$. Penalizes large weights; keeps all features but shrinks their impact.
- **L1 Regularization (Lasso):** Adds $\lambda ||w||_1$. Can shrink weights all the way to zero, effectively performing **Feature Selection**.

### Pros and Cons

**Pros**

- **Probabilistic Output:** Provides a well-calibrated probability, not just a hard label.
- **Efficiency:** Very fast to train and requires very little memory (only stores weights).
- **Transparency:** The weights $w$ tell you exactly which features (words) are most important for a class.

**Cons**

- **Linear Assumption:** It can only find linear boundaries. If your data is "circles within circles," it will fail without kernel tricks or polynomial features.
- **Multicollinearity:** If features are highly correlated, the individual weights $w$ can become unstable and hard to interpret.

### Use Cases

- **Sentiment Analysis:** (e.g., words like "excellent" get high positive weights).
- **Medical Diagnosis:** Predicting the probability of a disease based on clinical features.
- **Click-Through Rate (CTR):** Standard in online advertising to predict if a user will click.