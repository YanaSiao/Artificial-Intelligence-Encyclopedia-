## The Bag-of-Words (BoW) Mechanism

The core challenge in NLP is that text is of arbitrary length, but Machine Learning models require fixed-length inputs. BoW solves this by treating a document as a **multiset (bag)** of its words, disregarding grammar and word order.

- **Process:** 
	1. Define a vocabulary $V$ of all unique words in the corpus.
    2. Represent each document as a vector of length $|V|$.
    3. Each dimension $i$ contains the count (frequency) of the $i$-th word of the vocabulary in that document.

## Statistical Laws of Text

These laws explain why BoW vectors look the way they do (mostly zeros):

- **Zipf’s Law:** The frequency of a word ($f$) is inversely proportional to its rank ($r$).
    - _Formula:_ $f \propto \frac{1}{r}$
    - _Implication:_ A few "stop words" (the, a, is) dominate the counts, while most words are extremely rare (the "Long Tail").
        
- **Heap’s Law:** Describes how the vocabulary size ($M$) grows with the total number of tokens ($T$) in a collection.
    - _Formula:_ $M = kT^\beta$ (where $k \approx 10–100$ and $\beta \approx 0.5$)
    - _Implication:_ Vocabulary grows indefinitely, but at a diminishing rate (approximately the square root of document length).

- **From Zipf to Heaps**:
	- 1. **Infinite Vocabulary:** Zipf's Law implies that if you had an infinite corpus, you would eventually see every possible word (including rare names, technical terms, and typos).
    
	1. **Probability of New Words:** According to Zipf, the probability of the $r$-th most frequent word is $P_r \approx \frac{1}{r}$.
	2. **Integration:** As you increase the number of total tokens ($T$), you are essentially "sampling" further and further down the Zipfian tail.
	3. **The Result:** When you integrate the Zipfian distribution to find the expected number of unique types ($M$), the resulting function takes the form of $M = kT^\beta$.
	
	Specifically, if the Zipf exponent is $\alpha$, the Heaps exponent $\beta$ is approximately $1/\alpha$. In most natural languages, $\alpha \approx 1$, which is why $\beta \approx 0.5$ (the square root).
## Sparse (BoW) vs. Dense (Embeddings)

_See  [[Word2Vec and Data Embedding]] for dense alternatives._

| **Feature**          | **Sparse (BoW / TF-IDF)**                        | **Dense (Word2Vec / BERT)**                           |
| -------------------- | ------------------------------------------------ | ----------------------------------------------------- |
| **Dimensionality**   | High (Size of Vocabulary, e.g., 50k+)            | Low (Fixed, e.g., 300 or 768)                         |
| **Meaning**          | No semantic relationship (words are independent) | Captures semantics (similar words are close in space) |
| **Efficiency**       | Computationally expensive for large $V$          | Very efficient for modern hardware                    |
| **Interpretability** | High (each dimension = a specific word)          | Low (dimensions are abstract features)                |
## Pros and Cons

### ✅ Pros
- **Simple & Fast:** Very easy to implement and train.
- **Interpretable:** You can see exactly which words triggered a classification (e.g., "excellent" $\rightarrow$ Positive).
- **Effective for short text:** Works surprisingly well for tasks where keywords are the main signal (e.g., spam detection).
### ❌ Cons
- **Order-less (Lossy):** It cannot distinguish between "Dog bites man" and "Man bites dog."
- **Sparsity:** Most vector entries are zero, leading to the "Curse of Dimensionality."
- **Polysemy/Synonymy:** It doesn't know that "bank" (river) and "bank" (money) are different, or that "buy" and "purchase" are the same.

## Best Practices & Optimization

To make BoW effective, you must apply **Text Normalization** first:

- **Stop-word Removal:** Filter out high-frequency words (the, is, at) that don't carry specific meaning.
- **Stemming/Lemmatization:** Collapse "running", "runs", and "ran" into a single dimension.
- **N-grams:** Instead of just single words (unigrams), use pairs (bigrams) like "not good" to capture some local context.
- **TF-IDF Weighting:** Instead of raw counts, weight words by how "informative" they are. (See [[TF-IDF]]).

## BoW-Based Classifiers

According to Lesson 02, the "traditional" stack for text classification relies on BoW:

1. **Naive Bayes:** The baseline. High bias, low variance. Great for small datasets.
2. **Support Vector Machines (SVM):** Historically the "Gold Standard" for BoW before Deep Learning. Very effective at finding boundaries in high-dimensional sparse spaces.
3. **Regularized Logistic Regression:** Uses L1 (Lasso) or L2 (Ridge) to prevent the model from focusing too much on rare, noisy words.

## Use Cases

- **Spam Filtering:** Identifying specific keywords.
- **Topic Classification:** Sorting news into "Sports", "Politics", or "Finance".
- **Authorship Attribution:** Analyzing the frequency of specific function words.

