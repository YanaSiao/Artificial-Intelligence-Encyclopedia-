### The Core Concept

The Skip-gram model reverses the logic of standard language modeling. Instead of using context to predict a target word, Skip-gram uses a **single target word** to predict the **surrounding context words** within a fixed window.

- **Goal:** To learn word representations such that words appearing in similar contexts have similar vectors.
    
- **Intuition:** If you see the word "coffee," the model should assign a high probability to seeing words like "cup," "drink," or "morning" in the nearby positions.

### How it Works (The Architecture)

1. **Input Layer:** A one-hot encoded vector $x$ representing the target word $w_t$.
    
2. **Projection Layer (Hidden):** The input is multiplied by a weight matrix $W$ (the Embedding matrix). This effectively "plucks" the $d$-dimensional row corresponding to that word.
    
3. **Output Layer:** The hidden vector is multiplied by a different weight matrix $W'$ to produce scores for every word in the vocabulary.
    
4. **Softmax:** A softmax function is applied to these scores to turn them into probabilities. The model is trained to maximize the probability of the actual context words found in the training data.

### Mathematical Objective

The objective of Skip-gram is to maximize the average log probability of the context words $w_{t+j}$ given the center word $w_t$:

$$J(\theta) = \frac{1}{T} \sum_{t=1}^T \sum_{-m \leq j \leq m, j \neq 0} \log P(w_{t+j} | w_t)$$

Where $m$ is the size of the training window. The probability is typically calculated using the Softmax function:

$$P(o|c) = \frac{\exp(u_o^T v_c)}{\sum_{w=1}^V \exp(u_w^T v_c)}$$

- $v_c$: Vector for the center word.
    
- $u_o$: Vector for the output (context) word.

### Computational Efficiency: Negative Sampling

Calculating the denominator of the Softmax is extremely expensive because it requires summing over the entire vocabulary $V$ (often 100k+ words). To solve this, Skip-gram usually uses **Negative Sampling**:

- Instead of predicting the exact word, the model learns to distinguish the real context word (positive sample) from a small set of randomly chosen "noise" words (negative samples).

### Pros and Cons

|**Pros**|**Cons**|
|---|---|
|**Handles Rare Words:** Since each (target, context) pair is a separate training instance, rare words get more "updates" than in CBOW.|**Slower Training:** It requires more passes over the data because it predicts multiple context words for each target word.|
|**High Quality:** Generally produces better semantic representations for large datasets.|**Sensitive to Window Size:** The quality of embeddings depends heavily on the chosen $m$.|