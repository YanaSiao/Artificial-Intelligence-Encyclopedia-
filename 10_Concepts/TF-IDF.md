TF-IDF (Term Frequency-Inverse Document Frequency) is a numerical statistic intended to reflect how important a word is to a document in a collection (corpus). It is the most common weighting scheme used in Lexical Search and Text Clustering.

### TF: Term Frequency

Measures how frequently a term occurs in a document.

- **Raw TF:** $tf(t, d) = \text{count of term } t \text{ in document } d$.
    
- **Log-normalized TF:** $1 + \log(tf_{t,d})$ (Standard in Information Retrieval to dampen the effect of high-frequency words).

### IDF: Inverse Document Frequency

Measures how "informative" or rare a term is across the entire corpus ($N$ documents).

- **Formula:** $idf(t) = \log\left(\frac{N}{df_t}\right)$
    
- **Logic:** If a word (like "the") appears in every document ($df_t = N$), the $idf$ becomes $\log(1) = 0$, effectively neutralizing the word. If a word is very rare, the $idf$ value is high.

### The TF-IDF Product

The final weight is the product of these two components:

$$W_{t,d} = TF_{t,d} \times IDF_t$$

---
### Vector Space Similarity

**Cosine Similarity between TF-IDF Vectors**

In the Vector Space Model, both the query ($q$) and the document ($d$) are represented as high-dimensional vectors. The similarity between them is calculated as the cosine of the angle between these vectors.

The score is the dot product of the weighted term vectors:

$$score(q, d) = \frac{\vec{V}(q) \cdot \vec{V}(d)}{|\vec{V}(q)| |\vec{V}(d)|}$$

**Length Normalization**

Without normalization, long documents are unfairly advantaged because they contain more words, increasing the probability of a term match regardless of actual relevance. Cosine similarity inherently performs length normalization by dividing the dot product by the Euclidean length (L2 norm) of the vectors. This ensures that the score depends on the **relative** distribution of terms rather than the absolute count.

---

### BM25 (Best Matching 25)

**The Evolution of TF-IDF**

BM25 is a state-of-the-art ranking function used by modern search engines (like Elasticsearch and Lucene). It improves upon standard TF-IDF by addressing two major limitations: term frequency saturation and document length over-correction.

**Term Frequency Saturation ($k_1$)**

In standard TF-IDF, the score increases linearly with term frequency. In BM25, the contribution of a single term "saturates."

- **The Logic:** Seeing a search term 100 times in a document isn't 10 times more significant than seeing it 10 times.
    
- **The Parameter ($k_1$):** Controls the saturation speed. A typical value is $1.2$ to $2.0$.
    

**Document Length Normalization ($b$)**

BM25 scales the impact of document length using the parameter $b$ (usually $0.75$).

- If $b=1$, it performs full length normalization.
    
- If $b=0$, no length normalization is applied.
    
- **The Goal:** It penalizes long documents that repeat words without adding information, but protects documents that are long because they are multi-topic.
    

**The BM25 Formula**

$$score(D, Q) = \sum_{q_i \in Q} IDF(q_i) \cdot \frac{f(q_i, D) \cdot (k_1 + 1)}{f(q_i, D) + k_1 \cdot (1 - b + b \cdot \frac{|D|}{avgdl})}$$

Where $|D|$ is the document length and $avgdl$ is the average document length in the corpus.
### Linkage to Unsupervised Learning

- **Clustering:** TF-IDF vectors serve as the input for **[[Text Clustering]]** (K-Means).
    
- **Topic Modeling:** **[[Topic Modelling]]** (specifically LSA) uses a matrix of TF-IDF values to identify latent themes.
    
- **Zipf’s Law:** TF-IDF is the practical solution to **[[Vector Space Representations#Statistical Laws of Text]]**, as the IDF component automatically discounts the "Stop Words" at the top of the Zipfian distribution.