### Distance Functions (Metrics)

Distance measures the "dissimilarity" between two points. A valid distance metric $d(x, y)$ must satisfy the triangle inequality and non-negativity.

- **Euclidean Distance (L2 Norm):**
    
    The straight-line distance between two points.
    
    $$d(x, y) = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}$$
    
    - _Sensitivity:_ Very sensitive to the magnitude of the vectors. In NLP, a long document and a short document about the same topic will have a large Euclidean distance.
        
- **Manhattan Distance (L1 Norm):**
    
    The sum of the absolute differences of their coordinates ("Taxicab" distance).
    
    $$d(x, y) = \sum_{i=1}^{n} |x_i - y_i|$$
    

### Similarity Measures

Similarity measures how "alike" two vectors are, usually ranging from 0 to 1.

- **Cosine Similarity:**
    
    Measures the cosine of the angle between two vectors. It ignores magnitude and focuses on orientation.
    
    $$sim(x, y) = \cos(\theta) = \frac{x \cdot y}{\|x\| \|y\|}$$
    
    - **Core for NLP & RS:** This is the standard for text because it provides **length normalization**. Two documents with the same word proportions will have a similarity of 1, regardless of their length.