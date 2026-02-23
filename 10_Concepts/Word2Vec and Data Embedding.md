## Foundations of Word Representations
### 1. N-Gram Language Model
In n-gram models, one-hot vectors can be used to represent the words in a corpus.
Each unique word in the corpus is represented by a vector with a single element set to 1, and all other elements set to 0. The goal of N-gram language models is to predict the probability of a sentence given the previous words, called “context”.
![[Pasted image 20260214183743.png]]
### 2. The Problem with One-Hot Encoding

Traditionally, words are treated as discrete symbols (one-hot vectors).

- **No similarity:** The dot product between "cat" $[1, 0, 0]$ and "dog" $[0, 1, 0]$ is always 0.
    
- **Curse of Dimensionality:** In a 500,000-word vocabulary, each word is a 500,000-dimensional vector.
    

### 3. Word2Vec (Mikolov et al., 2013)

Instead of one-hot vectors, we learn **dense embeddings** where words that appear in similar contexts are mapped to nearby points in a low-dimensional space.

|**Feature**|**CBOW (Continuous Bag-of-Words)**|**Skip-gram**|
|---|---|---|
|**Objective**|Predicts the **target** word from the surrounding context.|Predicts the **context** words from a single target word.|
|**Complexity**|Faster to train; slightly better for frequent words.|Slower; works better for small amounts of data and rare words.|
|**Philosophy**|"Smoothing" over context.|Treating each context-target pair as a new observation.|
![[Pasted image 20260214190156.png]]
![[Pasted image 20260214190216.png]]
### 4. Vector Regularities (Analogies)

One of the most famous properties of Word2Vec is that semantic relationships are captured as linear offsets:

$$\text{vec("King") - vec("Man") + vec("Woman")} \approx \text{vec("Queen")}$$

### 5. GloVe: Global Vectors (Pennington et al., 2014)

GloVe improves on Word2Vec by explicitly using **global statistics**. It is trained on the ratios of word-word co-occurrence probabilities from the entire corpus.

- **Loss:** Trained by weighted least squares.
    
- **Benefit:** It makes explicit what Word2Vec does implicitly—encoding meaning as vector offsets.