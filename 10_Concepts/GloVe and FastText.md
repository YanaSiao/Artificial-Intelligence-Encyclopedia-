# GloVe (Global Vectors for Word Representation)

GloVe was developed at Stanford (Pennington et al., 2014) to combine the advantages of **Global Matrix Factorization** (like LSA) and **Local Context Windows** (like [[Word2Vec and Data Embedding|Word2Vec]]).

### The Core Concept

While Word2Vec "predicts" a context word given a target word (or vice-versa), GloVe argues that the **ratio of co-occurrence probabilities** is the key to capturing semantic meaning.

- **Intuition:** The relationship between _Ice_ and _Steam_ is best captured by looking at their co-occurrence probabilities with _Solid_ vs. _Gas_.

### The Objective Function

GloVe minimizes a weighted least squares objective that targets the log-co-occurrence counts:

$$J = \sum_{i,j=1}^V f(X_{ij}) (w_i^T \tilde{w}_j + b_i + \tilde{b}_j - \log X_{ij})^2$$

- $X_{ij}$: The number of times word $j$ appears in the context of word $i$.
    
- $w_i, \tilde{w}_j$: Word and context vectors.
    
- $b_i, \tilde{b}_j$: Bias terms to account for word frequency.
    
- $f(X_{ij})$: A weighting function that prevents very frequent word pairs (like "of the") from dominating the loss, while also ignoring zero-count pairs.

### Pros and Cons

|**Pros**|**Cons**|
|---|---|
|**Global Statistics:** Uses the entire corpus statistics at once.|**Memory Intensive:** Building the co-occurrence matrix for large vocabularies requires significant RAM.|
|**Stable Training:** Less sensitive to hyperparameter changes than Word2Vec.|**Static:** Like Word2Vec, it cannot handle Out-of-Vocabulary (OOV) words.|
|**Interpretability:** Excellent at capturing word analogies.|**Pre-computation:** Requires a full pass of the data to build the matrix before training starts.|

---

# FastText (Sub-word Embeddings)

Developed by Facebook AI Research (Bojanowski et al., 2016), FastText is an extension of Word2Vec that treats each word not as an atomic unit, but as a **Bag of Character N-grams**.

### The Sub-word Model

FastText represents a word $w$ as the sum of its character [[N-grams and Language Modeling|n-gram]] vectors.

- **Example:** For the word "apple" with $n=3$, the n-grams are: `<ap, app, ppl, ple, le>`.
    
- The vector for "apple" is the sum of the vectors for these n-grams + the vector for the whole word `<apple>`.

### The Scoring Function

The similarity between a word $w$ and a context word $c$ is the sum of the dot products of the n-gram vectors:

$$s(w, c) = \sum_{g \in \mathcal{G}_w} z_g^T v_c$$

- $\mathcal{G}_w$: The set of n-grams appearing in word $w$.
    
- $z_g$: The vector representation of n-gram $g$.
    
- $v_c$: The vector representation of context word $c$.

### Use Cases & Advantages

1. **Out-of-Vocabulary (OOV) Handling:** Because it learns from character patterns, it can generate a vector for a word it has never seen (e.g., "hippopotamification") by summing the vectors of its sub-parts.
    
2. **Morphologically Rich Languages:** Highly effective for languages like Italian, German, or Turkish, where word endings change frequently (e.g., "mangio", "mangi", "mangiamo").
    
3. **Noisy Data:** Robust against typos or social media slang.

---

### Summary Comparison

|**Feature**|**Word2Vec**|**GloVe**|**FastText**|
|---|---|---|---|
|**Training Unit**|Local Context Pairs|Global Co-occurrence Matrix|Character N-grams|
|**Core Idea**|Prediction (Local)|Statistics (Global)|Sub-word structures|
|**OOV Support**|No (UNK token)|No (UNK token)|**Yes**|
|**Training Speed**|Fast|Medium|Slower (more n-grams to process)|
|**Best For**|General semantics|Large, stable corpora|Morphological variation & Noisy data|