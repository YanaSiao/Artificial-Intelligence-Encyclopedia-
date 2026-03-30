### Evaluation: Perplexity ($PP$)

Perplexity quantifies the level of **surprise** or **confusion** a language model feels when it sees new, unseen text. It measures how unlikely the observed data is under the model's probability distribution.

#### 1. Mathematical Definition

The perplexity of a sequence of words $W = (w_1, w_2, \dots, w_n)$ is defined as the inverse probability of the sequence, normalized by the number of words $n$.

$$PP(W) = P(w_1, w_2, \dots, w_n)^{-\frac{1}{n}} = \sqrt[n]{\frac{1}{P(w_1, w_2, \dots, w_n)}}$$

**Step-by-Step Calculation:**

1. **Compute Sequence Probability:** Use the Chain Rule (and the Markov Assumption for N-grams). For a Bigram model, this is:
    
    $$P(w_1, \dots, w_n) = P(w_1) \times P(w_2|w_1) \times P(w_3|w_2) \dots \times P(w_n|w_{n-1})$$
    
2. **Geometric Mean:** Calculate the average per-word probability by taking the $n$-th root: $\sqrt[n]{P(W)}$.
    
3. **Inversion:** Invert that value to get the uncertainty/perplexity.

#### 2. Intuition: The Branching Factor

Perplexity can be interpreted as the **weighted average branching factor** of the language.

- If a model has a perplexity of $10$, it means that every time the model predicts the next word, it is as confused as if it had to pick uniformly at random from a set of $10$ possible words.
    
- A **perfect model** would have a perplexity of $1$ (it is never surprised).
    
- A **completely random model** (over a vocabulary $V$) would have a perplexity equal to $|V|$.

#### 3. Relationship to Information Theory

Perplexity is mathematically linked to the metrics used to train neural networks:

- **Negative Log-Likelihood (NLL):** Minimizing NLL is equivalent to minimizing Perplexity.
    
- **Cross-Entropy (CE):** Perplexity is $2^{H(W, P)}$, where $H$ is the cross-entropy of the model.
    
    $$PP(W) = 2^{\text{Cross-Entropy}}$$
#### 4. Evaluating the Model

When comparing two language models (e.g., a Unigram vs. a Trigram model) on the same test set:

- **Lower Perplexity = Better Model:** A lower value indicates the model assigned a higher probability to the test data (it "predicted" the test set more accurately).
    
- **Consistency:** You must evaluate models on the **same test set** and the **same vocabulary** for the comparison to be fair. If the vocabularies differ, the perplexity values are not directly comparable.

### The Sparsity Problem

If a word pair never appears in the training data, $P(w_n | w_{n-1}) = 0$. This causes the probability of the entire sentence to become zero.

**Smoothing Techniques**

1. **Laplace Smoothing (Add-1):** Adds 1 to every count so no probability is ever zero.
    
2. **Add-k Smoothing:** A variation where we add a fraction $k$ instead of 1.
    
3. **Backoff:** If the N-gram count is zero, "back off" to the $(N-1)$-gram (e.g., if the trigram is unknown, use the bigram).
    
4. **Interpolation:** Combine the probabilities of all N-grams (Unigram, Bigram, Trigram) using weights $\lambda$ that sum to 1.