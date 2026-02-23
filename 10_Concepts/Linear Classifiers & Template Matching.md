## 1. Image Unfolding (Flattening)

To use a standard Linear Classifier, we must convert the 3D image tensor ($R \times C \times 3$) into a 1D vector.

- **Process**: The pixels are "unrolled" (usually column-wise).
    
- **Result**: An image becomes a single point $x \in \mathbb{R}^d$, where $d$ is the total number of pixels multiplied by the number of channels.
    - _Example_: A $32 \times 32 \times 3$ image (CIFAR-10) becomes a vector of **3,072** dimensions.

## 2. The Linear Classifier Model

The classifier is defined by the function:

$$f(x, W, b) = Wx + b$$

- **$x$**: The flattened input image (vector of size $d \times 1$)
- **$W$**: The weight matrix (size $K \times d$), where $K$ is the number of classes.
- **$b$**: The bias vector (size $K \times 1$).
- **Score**: The output is a vector of $K$ scores. The class with the highest score is the predicted label.
    ![[Pasted image 20260217175036.png]]

## 3. Weights as Templates

A powerful way to interpret a linear classifier is to look at each row of the weight matrix $W$.

- **The Interpretation**: Each row $W_i$ corresponds to a specific class. If we reshape this row back into the original image dimensions ($R \times C \times 3$), it looks like a **template** of that class.
    
- **The Dot Product**: Calculating $W_i \cdot x$ is mathematically equivalent to calculating the **correlation** (similarity) between the input image and the learned template for that class.
    ![[Pasted image 20260217175731.png]]

### The "Two-Headed Horse" Problem

Linear classifiers struggle with spatial variance. If the training set contains horses facing both left and right, the classifier tries to learn a single template that satisfies both.

- **Result**: The learned "horse template" might look like a blurry horse with two heads.
- **Conclusion**: A single linear template is too rigid to capture the complex variations (rotation, scale, pose) of real-world objects.
    
## 4. Training the Linear Classifier 
Training a linear classifier is the process of finding the optimal parameters $W$ and $b$ that minimize a **Loss Function** over the entire training dataset ($TR$).

### 4.1 The Loss Function ($\mathcal{L}$)

The loss function is a mathematical formula that measures our "unhappiness" with the scores the model assigns to training images.

- **High Loss**: Occurs when the model assigns a low score to the correct class or a high score to an incorrect one.
- **Low Loss**: Occurs when the model correctly identifies the image with high confidence.
- **Common Types**: While the raw scores are used for prediction, calculating common losses like **Categorical Cross-Entropy** requires adding a **Softmax** activation to the output layer to convert scores into probabilities.

### 4.2 Regularization ($\mathcal{R}$)

To prevent the model from simply "memorizing" the training data (overfitting) and to ensure a unique, stable solution, a **Regularization** term is typically added to the loss.
- **The Objective Function**: $W, b = \text{argmin} \sum \mathcal{L}(x_i, y_i) + \lambda \mathcal{R}(W, b)$.
- **Hyperparameter ($\lambda$)**: A positive value that balances the trade-off between fitting the training data perfectly and keeping the weights "simple" or small.

### 4.3 Optimization: Gradient Descent

The minimization of the loss function is achieved using **Gradient Descent** or its variants.
- The algorithm iteratively adjusts $W$ and $b$ in the direction that most reduces the loss.
- Once training is complete, the training images can be discarded; only the learned parameters $W$ and $b$ are needed to classify new images.

## 5. Geometric Interpretation

In this view, each image is a **point** in a high-dimensional space ($d$-dimensional).

- **Decision Boundaries**: The linear classifier defines **hyperplanes** (high-dimensional planes) that slice through the space to separate different classes.
    
- **Linear Separability**: This only works if the classes can be separated by straight lines/planes. If the data is "wrapped" or complex, a linear classifier will fail.
    ![[Pasted image 20260217182833.png]]

## 6. Challenges of Image Classification

The "Semantic Gap" is the fundamental difficulty in Computer Vision: the difference between a grid of numbers (pixels) and the concept of an object (e.g., "Dog").

### 6.1 Visual Variance & Transformations

A robust classifier must be invariant to several types of transformations that change pixel values without changing the identity of the object:

- **Viewpoint Variation**: A single object can look drastically different when viewed from different angles.
    
- **Illumination Conditions**: Drastic changes in light levels or directions.
    
- **Deformation**: Objects like animals or clothes can take on many non-rigid shapes.
    
- **Occlusion**: Only a small part of the object might be visible (e.g., a cat hiding behind a couch).
    
- **Background Clutter**: The object may blend into a background that looks similar to its own texture.
    

### 6.2 Variability and Ambiguity

- **Intra-class Variability**: Within a single category, members can look very different. (e.g., Both a **Ferrari F1** car and a **Fiat 500** are "cars" but share almost no pixel-level similarities).
    
- **Label Ambiguity**: Images often contain multiple objects or boundaries where the "correct" label is subjective or unclear (e.g., a hybrid animal or an object that belongs to two categories).
    

### 6.3 The Curse of High Dimensionality

Even "small" images are high-dimensional. A $100 \times 100$ RGB image lives in a **30,000-dimensional space**.

- **The Curse**: As dimensionality increases, the "volume" of the space increases exponentially.
    
- **Data Sparsity**: To maintain the same level of "coverage" (density) of data points as you have in 2D, you would need an astronomical number of training images. In high-dimensional space, most points (images) are effectively "far" from each other, making traditional distance metrics less meaningful.
    

---

## 7. The k-Nearest Neighbor (k-NN) Classifier

The k-NN is the simplest possible classifier. It does not "learn" a model; instead, it memorizes the training data.

### 7.1 How it Works

1. **Training Phase**: Simply store all the training images and their labels ($O(1)$ complexity).
    
2. **Inference (Test) Phase**:
    - Calculate the distance between the test image and **every single image** in the training set.
    - Pick the **$k$** closest images.
    - The predicted label is the **majority vote** among those $k$ neighbors.
        

### 7.2 Distance Metrics for Images

To find "neighbors," we must define what "close" means for pixels:

- **L1 Distance (Manhattan)**: $d_1(I_1, I_2) = \sum_{p} |I_1^p - I_2^p|$ (Sum of absolute differences).
    
- **L2 Distance (Euclidean)**: $d_2(I_1, I_2) = \sqrt{\sum_{p} (I_1^p - I_2^p)^2}$ (Square root of sum of squares).
    

### 7.3 Pros and Cons

|**Pros**|**Cons**|
|---|---|
|Very simple to implement.|**Slow at test time**: Must compare against $N$ training samples.|
|No training time required.|**Memory intensive**: Must store the entire dataset.|
|Non-parametric (makes no assumptions about data shape).|**Sensitive to irrelevant features**: Distance is affected by background or lighting more than semantics.|

### 7.4 Perceptual vs. Pixel Similarity

The k-NN fails on images because it relies on **Pixel Similarity**, which does not equal **Perceptual (Semantic) Similarity**.

- **The Shift Problem**: Shifting an image by just a few pixels can result in a massive L2 distance, even though the image is identical to a human.
    
- **The Tint/Crop Problem**: Changing the overall color tint or cropping an image can create a smaller L2 distance to a completely different object than to the original object.