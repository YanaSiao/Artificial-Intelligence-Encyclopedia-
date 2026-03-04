### Concept and Geometry

A linear classifier makes a classification decision based on the value of a linear combination of its input features. Mathematically, it attempts to learn a weight vector $w$ and a bias $b$ to define a **decision boundary**.

- **The Decision Function:** $f(x) = w^T x + b$
- **The Hyperplane:** In a 2D space, the boundary is a line; in 3D, it is a plane; in high-dimensional NLP (thousands of words), it is a **hyperplane**.
- **Space Partitioning:** The hyperplane divides the input space into two half-spaces. The sign of the output determines the predicted class, while the magnitude (distance from the plane) often represents the confidence.

### Key Components

Linear classifiers differ primarily in how they find the "best" weights ($w$):

- **Loss Function:** Defines what "wrong" looks like (e.g., Hinge Loss for SVM, Cross-Entropy for Logistic Regression).
- **Optimization:** The process of adjusting $w$ to minimize the loss (e.g., Gradient Descent or Closed-form solutions).
- **Regularization:** A penalty added to prevent the weights from becoming too large or focusing on noise (crucial for high-dimensional text).

### Why Linear Classifiers dominate traditional NLP

Traditional NLP relies heavily on linear models like **Linear SVM** and **Regularized Logistic Regression**.

- **High Dimensionality:** Text data (Bag-of-Words) typically has more features than samples. In such high-dimensional spaces, data is almost always **linearly separable**.
    
- **Efficiency:** Calculating a dot product ($w^T x$) is computationally cheap, making these models ideal for large-scale document classification or edge devices.

### Pros and Cons

**Pros**

- **Interpretability:** You can inspect the weights $w$. A high positive weight for the word "excellent" tells you exactly why a document was classified as "Positive."
- **Speed:** Both training and inference are significantly faster than non-linear models or Deep Learning.
- **Lower Overfitting Risk:** Because they are "simpler" models (low capacity), they are less likely to memorize noise than deep neural networks, provided they are regularized.

**Cons**

- **Linear Limitation:** They cannot capture complex, non-linear feature interactions (e.g., they cannot learn the XOR logical operation without manual feature engineering).
- **Feature Engineering dependency:** To capture context (like "not good"), you must manually create features like **Bigrams**, whereas non-linear models (RNNs/Transformers) learn these patterns automatically.

### Taxonomy and Related Notes

Linear classifiers can be divided into two philosophical camps:

- **Generative:** Models the joint probability $P(x, y)$.
    - [[Naive Bayes]]

- **Discriminative:** Models the decision boundary directly $P(y|x)$.
    - [[Logistic Regression]]
    - [[Support Vector Machines (SVM)]]
    - [[The Perceptron]]