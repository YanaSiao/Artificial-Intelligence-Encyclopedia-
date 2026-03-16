### The Retrieval Pipeline

Modern systems use a two-stage architecture to balance speed and accuracy:

1. **Retrieval (Stage 1):** Uses fast, efficient methods like BM25 or Vector Space Similarity to narrow down millions of documents to a few hundred candidates.
    
2. **Reranking (Stage 2):** Uses a complex Machine Learning model to score and re-sort those few hundred candidates using thousands of features that would be too expensive to compute for the whole corpus.

### The Learning to Rank Problem

Unlike standard classification, the goal of LtR is to predict the **optimal order** of items. The model learns a scoring function $s = f(x)$ where $x$ is a feature vector representing the (Query, Document) pair.

**Feature Normalization**

Since a reranker combines very different signals—such as BM25 scores (range 0–20+), PageRank (range 0–1), and document age (in days)—features must be normalized (e.g., Z-score or Min-Max) before being fed into the model to prevent one feature from dominating the gradients.

### Loss Functions for Reranking

The objective of a reranker is to take a candidate set $D$ for a query $q$ and find a scoring function $s = f(q, d)$ that produces the optimal permutation. The "loss" measures how much the predicted order deviates from the ideal order. There are three main approaches to defining the [[Loss Functions]] during training:

**Pointwise Approach**

This approach transforms the ranking problem into a standard supervised task where each (Query, Document) pair is an independent data point.

- **Regression (MSE):** The model predicts a continuous relevance score. The loss is calculated as $L = (y_i - \hat{y}_i)^2$. This is used when relevance labels are numerical (e.g., a scale of 0 to 4).
    
- **Classification (Log Loss):** Used for binary relevance (Clicked vs. Not Clicked). The model uses Binary Cross-Entropy: $L = -[y \log(\hat{y}) + (1-y) \log(1-\hat{y})]$.
    
- **The "Independent" Flaw:** Pointwise loss does not consider the relative position of documents. If the model predicts $0.9$ for a "Relevant" document and $0.8$ for a "Highly Relevant" one, the pointwise loss may be small, but the ranking error for the user is significant.
    

**Pairwise Approach**

Instead of absolute scores, the pairwise approach focuses on the relative order of document pairs $(d_i, d_j)$ within the same query.

- **Logistic Preference Loss (RankNet):** The model minimizes the probability of misordering a pair. It calculates the probability that $d_i$ should be ranked higher than $d_j$ using the sigmoid of the score difference:
    
    $$\hat{P}_{ij} = \sigma(s_i - s_j) = \frac{1}{1 + e^{-(s_i - s_j)}}$$
    
- **Cross-Entropy Application:** The loss is the cross-entropy between this predicted probability and the ground truth (1 if $y_i > y_j$, 0.5 if $y_i = y_j$).
    
- **Margin-Based (BPR):** Common in Recommender Systems, this maximizes the distance between the score of an observed interaction ($s_i$) and an unobserved one ($s_j$): $L = -\ln \sigma(s_i - s_j)$.
    

**Listwise Approach**

Listwise loss considers the entire candidate set for a query as a single instance. This is the most complex but most effective approach for search engines because it aligns with the final evaluation metrics.

- **Probability Distribution (ListNet):** The scores for all documents in a list are converted into a probability distribution using a Softmax function. The model then minimizes the **KL-Divergence** between the predicted distribution and the true relevance distribution.
    
- **Metric Optimization (LambdaMART):** Since metrics like NDCG are not differentiable (they depend on discrete ranks), LambdaMART uses "pseudo-gradients" (Lambdas). These Lambdas represent the change in NDCG that would occur if two documents were swapped, allowing the model to focus on correcting errors at the very top of the list.
    

### Evaluation: Ranking Metrics

Rerankers are [[Ranking metrics| evaluated]] on their ability to put the most relevant items at the top:

- **MAP (Mean Average Precision):** Emphasizes finding as many relevant documents as possible in the top results.
    
- **NDCG (Normalized Discounted Cumulative Gain):** Specifically accounts for "graded" relevance (e.g., a "Perfect" match is worth more than a "Relevant" match) and uses a logarithmic discount to penalize relevant items found lower in the list.
    

### Pros and Cons

**Pros**

- **Precision:** Allows the use of non-lexical features like user location, device type, or deep semantic embeddings.
    
- **Flexibility:** Can optimize for multiple business goals (relevance, diversity, freshness) simultaneously.

**Cons**

- **Latency:** Computing features and running a model (especially an ensemble or neural net) adds time to the query.
    
- **Data Hunger:** Requires large amounts of "Clickstream" data or human-labeled relevance judgments to train effectively.