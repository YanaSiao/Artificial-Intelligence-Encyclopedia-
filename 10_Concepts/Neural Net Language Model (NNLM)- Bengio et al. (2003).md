The NNLM was a landmark shift in NLP, moving from discrete N-gram counts to a continuous "distributed representation" of words. It simultaneously learns the **word feature vectors** and the **parameters of the probability function** of the word sequence.

### Model Architecture

The model uses a fixed context window of size $n-1$ (the previous words) to predict the $n$-th word.

#### 1. Input Layer

- **Input:** $n-1$ words represented as discrete indices from a vocabulary $V$.
    
- **Representation:** Each word is initially a high-dimensional one-hot encoded vector.

#### 2. Projection Layer (Embedding Layer)

- **Mechanism:** A shared matrix $C$ (of size $|V| \times m$) is used to map each word index into a continuous, low-dimensional space $m$.
    
- **Result:** The one-hot vectors are multiplied by $C$ to produce **word embeddings**. These embeddings are concatenated into a single long vector $x$.
    
- **Key Characteristic:** The mapping is shared across all positions in the context window.

#### 3. Hidden Layer

- **Mechanism:** A fully connected layer that takes the concatenated embeddings as input.
    
- **Activation:** Typically uses a **tanh** or **sigmoid** function to introduce non-linearity.
    
- **Purpose:** To learn the complex, non-linear interactions between the words in the context window.

#### 4. Output Layer

- **Mechanism:** A fully connected layer with a **Softmax** activation function.
    
- **Output:** A probability distribution over the entire vocabulary $V$.
    
- **Goal:** $P(w_t | w_{t-1}, \dots, w_{t-n+1}) = \frac{e^{y_{w_t}}}{\sum_i e^{y_i}}$
    
- **Characteristic:** This layer is the "bottleneck" for computation because the size of the output is equal to the vocabulary size (which can be millions of words).

![[Pasted image 20260214185404.png]]

---

### Key Characteristics & Impact

- **Distributed Representation:** Words are no longer "islands"; similar words (e.g., "dog" and "cat") end up with similar vectors in the projection space because they appear in similar contexts.
    
- **Solving the Curse of Dimensionality:** Unlike N-grams, which require exponential data to see every possible word combination, NNLM generalizes by using the similarity of word vectors.
    
- **Computational Complexity:** The main drawback is the computational cost of the Softmax over a huge vocabulary at every training step.