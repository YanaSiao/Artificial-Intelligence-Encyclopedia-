## 1. Fundamentals & Non-Personalized
- [[Introduction to Recommender Systems]]: Taxonomy, goals, and user/item definitions.
- [[Preference Modeling]]: Implicit vs. Explicit feedback (Ratings vs. Clicks).
- [[Non-Personalized Recommenders]]: Top Popular, Best Rated, and Global Biases.

## 2. Evaluation (Cross-Link to ML)
_Note: Reuse your `[[Evaluation Metrics]]` note from ML/NLP, but add RS-specific sections._

- [[RS Experimental Protocols]]: Online vs. Offline vs. Lab studies.
- [[Accuracy vs Ranking Metrics]]:
    - Error metrics (RMSE, MAE).
    - Ranking metrics (NDCG, Hit Rate, MRR).
    - [[Evaluation Metrics]]
- [[Beyond Accuracy]]: Novelty, Diversity, and Serendipity.
- [[The Cold Start Problem]]: New-User and New-Item challenges.

## 3. Content-Based Filtering (CBF)
- [[Content-Based Recommenders]]: Building user profiles from item attributes.
- [[TF-IDF]]: **(Reuse: [[Vector Space Representations]])** using attributes for similarity.
- [[Similarity Measures]]: Cosine similarity, Jaccard, and Pearson correlation.

## 4. Collaborative Filtering (CF)
- [[User-User Collaborative Filtering]]: KNN-based neighborhood models.
- [[Item-Item Collaborative Filtering]]: Why it often scales better than User-User.
- [[Similarity Weighting & Shrinkage]]: Handling low-confidence similarities.

## 5. Dimensionality Reduction & Matrix Factorization
- [[Matrix Factorization]]: The core of modern RS.
- [[SVD and LSA]]: **(Reuse: [[Topic Modelling]])** Latent Semantic Analysis.
- [[Matrix Factorization Training]]: FunkSVD, Alternating Least Squares (ALS), and BPR (Bayesian Personalized Ranking).
- [[Advanced Factorization]]: SVD++ and Asymmetric SVD.

## 6. Hybrid & Advanced Architectures
- [[Hybrid Recommender Systems]]: Combining CF and CBF.
- [[Factorization Machines]]: Handling side information and sparse features.
- [[Graph-Based Recommenders]]:
    - Random Walks (PageRank, P3Alpha, RP3Beta).
    - [[Graph Convolutional Networks (GCN)]] for RS (LightGCN).

## 7. Deep Learning for Recommenders
- [[Neural Recommender Models]]: Loss functions for top-N recommendation.
- [[Two-Tower Architectures]]: Scaling retrieval for millions of items.
- [[Autoencoders for RS]]: **(Reuse: [[Autoencoders and Sparse Autoencoders]])** Denoising and Variational (VAE-CF).