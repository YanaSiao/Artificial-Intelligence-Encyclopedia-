Topic modeling is a type of statistical model for discovering abstract "topics" that occur in a collection of documents. It treats a corpus as a **Document-Term Matrix (DTM)** and attempts to decompose it into lower-dimensional representations.

**Latent Semantic Analysis (LSA)**

LSA (also known as Latent Semantic Indexing) uses **Singular Value Decomposition (SVD)** to reduce the dimensionality of the DTM.

- **The Math:** It decomposes the matrix $A$ into $U \Sigma V^T$.
    
    - $U$: Document-to-Topic matrix.
        
    - $\Sigma$: Strength of each topic.
        
    - $V^T$: Topic-to-Word matrix.
        
- **Goal:** To capture "Latent Semantics"—the idea that words used in similar contexts (synonyms) will be mapped to the same latent space.


**Low-Dimensional Representations**

Text data is naturally high-dimensional (one dimension per unique word). Topic modeling acts as a form of **Feature Extraction** by representing each document as a distribution over a small number of topics (e.g., 50 topics instead of 50,000 words).

- **Linear vs. Non-Linear:** LSA is linear. For complex manifolds, non-linear techniques like **t-SNE** or **UMAP** are used to visualize how these low-dimensional topic vectors cluster together.
    

---

### Latent Dirichlet Allocation (LDA)

LDA is the most common **Generative Model** for topic modeling. It assumes that documents are produced by a random process involving hidden (latent) variables.

**The Generative Process**

LDA imagines that an author writes a document following these steps:

1. **Pick a distribution of topics** for the document (e.g., 60% Politics, 30% Economics, 10% Tech) from a Dirichlet distribution $\alpha$.
    
2. **For each word in the document:**
    - Pick a topic from the distribution chosen in Step 1.
    - Pick a word from that specific topic's word distribution (from a Dirichlet distribution $\beta$).
    

**The Inverse Problem (Inference)**

In reality, we only see the documents. The goal of LDA training (using **Gibbs Sampling** or **Variational Inference**) is to work backward from the observed words to find the most likely topic distributions that could have generated them.

**Key Hyperparameters**

- **$\alpha$ (Alpha):** Controls document-topic density. High $\alpha$ means documents contain many topics; low $\alpha$ means documents are focused on one or two topics.
    
- **$\beta$ (Beta):** Controls topic-word density. High $\beta$ means topics contain many words; low $\beta$ means topics are focused on a few specific "signature" words.
    

---

### Use Cases for Topic Modeling

- **Document Categorization:** Automatically tagging large archives of news or research papers without manual labeling.
    
- **Trend Discovery:** Analyzing social media or scientific journals over time to see which topics are rising or falling in popularity (**Dynamic Topic Modeling**).
    
- **Recommendation Engines:** Suggesting articles to users based on the topic profile of what they have read in the past.
    
- **Feature Engineering:** Using topic distributions as inputs for supervised classifiers (e.g., Logistic Regression) to improve generalization.