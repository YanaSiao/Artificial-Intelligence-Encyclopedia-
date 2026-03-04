## 1. Local (Spatial) Transformations

Unlike global operations that look at the entire image (like a simple MLP input), **Spatial Transformations** process an image by mixing pixels locally within a small region called a **neighborhood ($U$)**.

- **Mechanism**: The output value at a specific pixel $(r, c)$ is a function of the intensity values of its neighbors $I(r+u, c+v)$, where $(u, v)$ defines the displacement within the neighborhood $U$.
    
- **Space Invariance**: These transformations are typically "space-invariant," meaning the exact same function or filter is applied to every pixel in the image, regardless of its position.

## 2. Correlation: The Local Linear Filter

A **Linear Transformation** in this context implies that the output is a linear combination of the pixels in the neighborhood $U$.

- **The Filter/Kernel ($w$)**: This operation is entirely defined by a small matrix of weights $\{w(u, v)\}$, commonly referred to as a **filter** or **kernel**.
    
- **The Formula**: The correlation of a filter $w$ and an image $I$ is calculated as the sum of point-wise products:    $$I \otimes w(r, c) = \sum_{u, v \in U} w(u, v) \cdot I(r+u, c+v)$$
- **Process**: The filter "slides" over each pixel of the input image, performing an element-wise product and summing the result to produce a single value in the output image.
    
## 3. Implementation and Extensions

- **Computational Logic**: Implementation requires nested loops: two for the image dimensions $(r, c)$ and two for the filter dimensions $(u, v)$ to accumulate the products.
    
- **RGB and Grayscale**:
    - **Grayscale**: The formula applies directly to a single intensity plane.
    - **RGB**: Correlation is computed **channel-wise** and then summed across all three color planes (Red, Green, Blue) to produce the final output.
        
- [[Linear Classifiers & Template Matching| Template Matching]]: Correlation can be used to find a specific pattern (template) in an image. However, a "perfect match" isn't the only thing that yields a high value; bright areas in the image can also produce high correlation scores, making **normalization** essential for effective template matching.
![[Pasted image 20260217174043.png]]

## 4. Key Properties & Concepts

To fully understand these operations in a modern deep learning context, consider these additional foundational concepts:

- **Padding**: To ensure the output image has the same dimensions as the input, "padding" (adding extra border pixels, often zeros) is used to allow the kernel to center on edge pixels.
    
- **Stride**: The "step size" the filter takes as it moves across the image. A stride $> 1$ results in a downsampled output.
    
- **Correlation vs. Convolution**: Mathematically, **Convolution** is identical to correlation except the kernel is flipped (rotated 180°) before the operation. In deep learning libraries, the term "convolution" is often used even if the underlying operation is technically correlation, as the network learns the weights $w$ anyway.

## 5. Linear Classifiers as Global Correlation

A standard linear classifier (1-layer NN) can be re-interpreted through the lens of correlation:

- **Weights as Templates**: The weights $W[i, :]$ learned by a classifier for a specific class (e.g., "car") can be reshaped back into an image.
    
- **Classification Score**: The final score for a class is essentially the correlation between the input image and the learned class template.
    
- **Limitations**: Simple linear correlation/template matching is sensitive to transformations. For example, if a dataset contains horses facing both left and right, the learned "horse template" might look like a **two-headed horse** to capture both possibilities.