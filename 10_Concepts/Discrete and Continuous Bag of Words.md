### Bag of Words (BoW)

The Bag of Words model is a simplifying representation used in Natural Language Processing and Information Retrieval. In this model, a text (such as a sentence or a document) is represented as the multiset of its words, disregarding grammar and even word order but keeping multiplicity.

**The Algorithm**

1. **Vocabulary Building:** Compile a list of all unique tokens (words) appearing in the corpus. This list is of size $V$.
    
2. **Vectorization:** For any given document, create a vector of length $V$. Each index $i$ in the vector corresponds to a word in the vocabulary.
    
3. **Counting:** The value at index $i$ is the number of times the $i$-th vocabulary word appears in the document.

**Formulaic Representation**

For a document $D$, the vector $\vec{v}$ is:

$$\vec{v} = [count(w_1), count(w_2), \dots, count(w_V)]$$

**Pros**:
- **Simple:** Easy to implement and understand.
- **Efficient:** Works well for document classification (e.g., Spam vs. Not Spam).

**Cons**:
- **Sparse:** Most entries are zero, leading to high memory usage for large vocabularies.
- **Loss of Context:** "Man bit dog" and "Dog bit man" result in identical vectors.    
- **No Semantics:** "Great" and "Excellent" are treated as completely different dimensions with zero correlation.

---

### Continuous Bag of Words (CBOW)

CBOW is one of the two core architectures of **Word2Vec**. Unlike the discrete BoW, which represents an entire document, CBOW is a "predictive" model that learns **word embeddings** by predicting a target word from a window of surrounding context words.

**The Architecture**

1. **Input Layer:** One-hot encoded vectors of context words within a window $C$ (e.g., 2 words before and 2 words after the target).
    
2. **Projection Layer:** The one-hot vectors are multiplied by an embedding matrix $W$. The resulting vectors are **averaged** to create a single hidden layer vector $h$.
    
    - _The "Bag" part:_ The order of context words in the window does not matter; they are simply averaged together.
        
3. **Output Layer:** The hidden vector is multiplied by an output weight matrix $W'$ to produce scores for every word in the vocabulary.
    
4. **Softmax:** Converts scores into probabilities. The model is trained to maximize the probability of the actual target word.

**Mathematical Objective**

Maximize the log-likelihood of the target word $w_t$ given its context $Context$:

$$J = \log P(w_t | w_{t-C}, \dots, w_{t-1}, w_{t+1}, \dots, w_{t+C})$$

The hidden layer calculation is:

$$h = \frac{1}{2C} \sum_{-C \leq j \leq C, j \neq 0} v_{w_{t+j}}$$

**Pros and Cons**

- **Dense:** Resulting vectors are low-dimensional (e.g., 300) and dense (no zeros).
- **Semantic:** Captures relationships; words with similar "bags of context" will have similar vectors.
- **Speed:** Faster to train than [[Skip-gram Architecture|Skip]]-gram and works well for frequent words.

- **Smoothing Effect:** Averaging context words can "wash out" specific nuanced relationships compared to Skip-gram.

---

### Summary Comparison

| **Feature**        | **Bag of Words (BoW)**   | **Continuous Bag of Words (CBOW)** |
| ------------------ | ------------------------ | ---------------------------------- |
| **Model Type**     | Discrete / Statistical   | Distributed / Neural               |
| **Representation** | Sparse (mostly zeros)    | Dense (real numbers)               |
| **Context**        | Global (entire document) | Local (fixed window)               |
| **Goal**           | Represent a document     | Learn word embeddings              |
| **Ordering**       | Ignores word order       | Ignores context word order         |