## Metric Learning

Metric learning shifts the focus from predicting a specific class label to learning a **distance metric** between data points. The goal is to find a mapping $f_W(I)$ such that the distance between representations of similar images is small, while the distance between different images is large.


- **Problem with Classification for ID:** Standard classification requires a fixed number of classes (people). If a new person needs to be added to a system (e.g., a face ID for a door), a classification model would require retraining or adding a new output neuron.

- **The Goal:** Learn a transformation $W$ such that:    
    $f_W(I_i) - f_W(T_i)^2 < f_W(I_i) - f_W(T_j)^2$ for all $j \neq i$. This allows the system to recognize new individuals by simply comparing their features to a stored "template" without retraining the whole network.
    

## Siamese Networks

A Siamese Network consists of two (or more) identical branches—meaning they share the same architecture and the **same weights $W$**.

- **Architecture:** Two images ($I_i, I_j$) are passed through the same CNN branches to produce feature embeddings $f_W(I_i)$ and $f_W(I_j)$.
    
- **Purpose:** The network is trained to output embeddings that are close together in the feature space if the inputs belong to the same class and far apart if they do not.

### Pros

- **Dynamic Scalability:** You do not need to retrain the network to add a new person/class to the system. You only need to extract the embedding $f_W(I)$ for the new person and compare it to existing templates.
- **Learning Relationships:** Instead of learning what "Giacomo" looks like in isolation, the network learns the concept of "similarity" and "dissimilarity".
- **Dimensionality Reduction:** They project high-dimensional data (e.g., $224\times224\times3$ images) into a much smaller, meaningful feature space where Euclidean distance corresponds to semantic similarity.
- **Efficient Representations:** They allow for face identification even with very few images per person, as the network is trained on pairs or triplets rather than just single-image labels.

### Cons

- **High Computational Cost for Inference:** To find the best match in a database of $N$ people, you may have to compute $N$ distance comparisons for every single query.
- **Training Complexity:** Training with **Contrastive** or **Triplet Loss** is significantly harder than standard Cross-Entropy. It requires careful "mining" of hard pairs or triplets—if you pick easy pairs, the loss becomes zero and the network stops learning.
- **Sensitivity to Margin:** The performance is highly dependent on the hyperparameter $m$ (margin). If $m$ is poorly tuned, the network may collapse all embeddings into a single point or fail to separate classes effectively.
- **Lack of Absolute Reference:** Because they only learn relative distances, the network doesn't inherently "know" what a specific class is; it only knows how different it is from another.

### Use cases

- **Large-Scale Identification:** When the number of classes is extremely large or unknown at training time (e.g., face ID for millions of users).
    
- **Verification Tasks:** When the goal is to determine if two images are the same thing (e.g., signature verification, matching a passport photo to a live camera feed).
    
- **Low-Data Regimes:** When you have many categories but only 1 or 2 examples per category (One-Shot Learning).
    
- **Anomaly Detection:** When you need to find samples that do not look like anything in your "normal" template set.
## Contrastive Loss

Contrastive loss is used to train Siamese Networks by looking at **pairs of images**.

- **Mathematical Definition:**
    
    $\mathcal{L}_W(I_i, I_j, y_{i,j}) = (1 - y_{i,j}) \frac{1}{2} \|f_W(I_i) - f_W(I_j)\|^2 + y_{i,j} \frac{1}{2} \max(0, m - \|f_W(I_i) - f_W(I_j)\|)^2$.
    
- **Key Components:**
    
    - **$y_{i,j} \in \{0, 1\}$**: A label indicating if the pair is similar ($0$) or different ($1$).
        
    - **Margin ($m$)**: A radius that defines how far apart dissimilar points must be pushed. If dissimilar points are already further than $m$, the loss becomes zero.
        
    - **Behavior**: It minimizes the distance between similar pairs and maximizes the distance between dissimilar pairs up to the margin $m$.
        

## Triplet Loss

Triplet loss is an evolution of contrastive loss that looks at **three samples** simultaneously: an **Anchor ($I$)**, a **Positive ($P$)** (same class as anchor), and a **Negative ($N$)** (different class).

- **Goal:** Ensure the distance between the Anchor and Positive is smaller than the distance between the Anchor and Negative by at least a margin $m$.
    
- **Mathematical Definition:**
    
    $\mathcal{L}_W(I, P, N) = \max(0, m + \|f_W(I) - f_W(P)\|^2 - \|f_W(I) - f_W(N)\|^2)$.
    
- **Advantage:** Instead of forcing similar items to a single point or dissimilar items to be exactly $m$ distance away, it focuses on the **relative** ordering of distances.
## Contrastive vs. Triplet Loss

|**Feature**|**Contrastive Loss (Pairs)**|**Triplet Loss (Triplets)**|
|---|---|---|
|**Input**|Pairs: $(x_i, x_j)$ with label $y \in \{0,1\}$|Triplets: $(A, P, N)$ (Anchor, Positive, Negative)|
|**Pros**|- **Simpler Implementation**: Easier to construct pairs.<br><br>  <br><br>- **Intuitive**: Directly forces similar items together in the feature space.|- **Relative Learning**: Focuses on the **ranking** of distances rather than absolute values.<br><br>  <br><br>- **Robustness**: Better separation in complex tasks (e.g., FaceID).|
|**Cons**|- **Rigid Constraints**: Forcing dissimilar pairs to be exactly $m$ distance apart is often too restrictive and hard to optimize.|- **Hard Triplet Mining**: Extremely sensitive to data selection. "Easy" triplets result in zero loss/no learning; requires complex mining.|