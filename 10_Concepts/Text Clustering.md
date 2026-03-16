### Why Cluster Text?

Clustering is an **Unsupervised Learning** task used to find natural groupings in data without labels.

- **Discovery:** Finding hidden themes in a large corpus of documents.
    
- **Organization:** Automatically creating folder structures for news or support tickets.
    
- **Dimensionality Reduction:** Representing a document by its cluster ID instead of a 50,000-word vector.
### Foundations: Distance and Similarity

Clustering algorithms rely on a mathematical definition of "closeness." For text data, the choice of metric is critical to the quality of the clusters.

- **Primary Metric:** Refer to **[[Similarity and Distance Measures]]** for details on Euclidean Distance and Cosine Similarity.
    
- **Recommendation:** Use **Cosine Similarity** for TF-IDF vectors to ensure length normalization across documents of varying sizes.

### K-Means Clustering

A "hard" clustering algorithm where every document belongs to exactly one of $K$ clusters.

**The Algorithm:**

1. **Initialize:** Pick $K$ random points as initial **centroids** ($\mu$).
    
2. **Assignment:** Assign each document to the nearest centroid (typically using Squared Euclidean Distance).
    
3. **Update:** Calculate the new centroid for each cluster by taking the mean of all assigned points.
    
    $$\mu_j = \frac{1}{|C_j|} \sum_{x \in C_j} x$$
    
4. **Repeat:** Iterate until centroids no longer move or the **Inertia** (Within-Cluster Sum of Squares) is minimized.

### Topic Models (Soft Clustering)

Unlike K-Means, Topic Models allow for **Soft Clustering**, where a document can belong to multiple clusters (topics) with different weights.

- **LSA (Latent Semantic Analysis):** Uses Singular Value Decomposition (SVD) on the TF-IDF matrix to find "latent" dimensions. It is essentially a linear dimensionality reduction.
    
- **LDA (Latent Dirichlet Allocation):** A probabilistic model that assumes documents are a mixture of topics and topics are a mixture of words.

### Hierarchical Agglomerative Clustering (HAC)

HAC is a "bottom-up" approach to clustering. It does not require the number of clusters ($K$) to be specified in advance.

**The Algorithm**

1. **Start:** Treat every single document as its own individual cluster.
    
2. **Merge:** Find the two "closest" clusters and merge them into a single cluster.
    
3. **Repeat:** Continue merging until only one giant cluster (containing all documents) remains.
    

**Linkage Criteria**

The definition of "closest" depends on the linkage function used to measure distance between two clusters ($A$ and $B$):

- **Single Linkage:** Distance between the two closest members. (Can lead to "chaining" where clusters become long and thin).
    
- **Complete Linkage:** Distance between the two furthest members. (Tends to find compact, spherical clusters).
    
- **Average Linkage:** Average distance between all pairs of points in the two clusters.
    
- **Ward’s Method:** Merges clusters that result in the minimum increase in total within-cluster variance. This is often the most effective for text data.
    

**The Dendrogram**

The result of HAC is typically visualized as a **Dendrogram** (a tree diagram). By "cutting" the tree at a certain height, you can choose the number of clusters after the algorithm has finished.

---

### DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

DBSCAN is a density-based algorithm that groups together points that are closely packed while marking points in low-density regions as **outliers (noise)**.

**Key Parameters**

- **$\epsilon$ (Epsilon):** The maximum distance between two points to be considered neighbors.
    
- **MinPts:** The minimum number of points required to form a "dense region."
    

**Point Classification**

- **Core Points:** Points that have at least $MinPts$ within their $\epsilon$-neighborhood.
    
- **Border Points:** Points that are within $\epsilon$ distance of a Core point but have fewer than $MinPts$ neighbors themselves.
    
- **Noise Points:** Anything that is neither a Core nor a Border point.
    

**Pros and Cons**

- ✅ **No $K$ required:** It discovers the number of clusters automatically.
- ✅ **Non-Spherical Shapes:** Unlike K-Means, it can find clusters of arbitrary shapes (e.g., U-shapes or concentric circles).
- ✅ **Robust to Outliers:** It explicitly identifies noise, which is useful for filtering "junk" documents in a corpus.
    
- ❌ **Varying Densities:** It struggles if the clusters have very different densities.
- ❌ **High Dimensionality:** In very high-dimensional text spaces (thousands of words), the concept of "density" becomes blurred because points tend to be equidistant (the Curse of Dimensionality).


---

### Comparison for Text Data

|**Feature**|**K-Means**|**HAC**|**DBSCAN**|
|---|---|---|---|
|**Cluster Shape**|Spherical|Depends on Linkage|Arbitrary|
|**Speed**|Fast $O(n)$|Slow $O(n^2)$|Medium $O(n \log n)$|
|**Number of Clusters**|Must specify $K$|Choose from Dendrogram|Automatic|
|**Outlier Handling**|Pulls centroids toward outliers|Included in tree|Explicitly ignores noise|

---
### Visualization and Dimensionality Reduction

High-dimensional text data (thousands of words) cannot be visualized directly. We use dimensionality reduction to map it down to 2D or 3D.

- **Linear (PCA):** Finds projections that maximize the separation of data points. Useful for understanding word correlations.
    
- **Non-linear (t-SNE and UMAP):** These techniques assume high-dimensional data lives on a lower-dimensional **manifold**. They attempt to discover this manifold and map it to 2D, though this causes some local distortions (similar to a 2D map of the Earth).