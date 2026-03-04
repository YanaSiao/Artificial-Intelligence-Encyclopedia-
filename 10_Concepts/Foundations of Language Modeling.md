## 1. The Core Objective

A Language Model (LM) determines the probability $P(s)$ of a sequence of words. Traditionally, this is calculated as:

$$P(S_k) = \prod_{i=1}^{k} P(w_i | w_1, \dots, w_{i-1})$$

In **N-gram models**, we assume a word depends only on the previous $n-1$ words.

## 2. The Failure of Local Representations

Traditional NLP treats words as discrete atomic symbols (e.g., `'cat' = Id537`). This leads to two major problems:

- **Word Similarity Ignorance:** In a one-hot vector space, every word pair has a dot product of 0. The model cannot "know" that _President_ is similar to _Obama_ or that _Chicago_ is similar to _Illinois_ based solely on their encoding.
    
- **The Curse of Dimensionality:** If you use a 10-gram model with a 100,000-word vocabulary, the model "lives" in a space with $100,000^{10}$ possible slots. Most of these slots will have 0 observations, meaning the **probability mass vanishes** and the model cannot generalize to unseen word combinations.

## 3. Comparison of Input Encodings

The performance of NLP applications depends heavily on how you encode these inputs:

|**Local Representations (Sparse)**|**Continuous Representations (Dense)**|
|---|---|
|**N-grams:** Probability based on frequency counts.|**Latent Semantic Analysis (LSA):** SVD-based.|
|**Bag-of-Words (BoW):** Document as a word set.|**Latent Dirichlet Allocation (LDA):** Topic-based.|
|**1-of-N (One-Hot):** Discrete atomic symbols.|**Distributed Representations:** Dense vectors.|

## 4. The Benefits of Distributed Representations

Mapping words to a continuous $m$-dimensional space (where $100 < m < 500$) solves the foundation issues via three mechanisms:

1. **Compression:** Dimensionality reduction from $|V| > [cite_start]10^6$ down to $m$.
    
2. **Smoothing:** Moving from discrete counts to continuous values to avoid zero probabilities.
    
3. **Densification:** Turning a massive sparse vector into a small, dense feature vector