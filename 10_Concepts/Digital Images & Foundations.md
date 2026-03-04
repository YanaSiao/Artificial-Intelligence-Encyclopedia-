## 1. Digital Image Representation

In computer vision, a digital image is represented as a **three-dimensional array (tensor)**.

- **Structure**: An image $I \in \mathbb{R}^{R \times C \times 3}$ is defined by its height ($R$ for rows), width ($C$ for columns), and color channels.
    
- **RGB Color Space**: Most color images use the Red, Green, and Blue (RGB) model. Each pixel is an RGB triplet $[R, G, B]$ representing the intensity of each color.
    
- **Bit Depth and Range**: Typically, each color channel is encoded using **8 bits**. This results in pixel intensity values ranging from **0 to 255**.
    
    - $0$: Minimal intensity (black).
    - $255$: Maximum intensity (pure color).   

## 2. Tensors and Data Loading

When working with neural networks, images are treated as tensors.

- **In-Memory vs. Disk**: While images are stored on disk in compressed formats (like JPEG), they must be **uncompressed** when loaded into memory for processing. This makes the memory footprint in RAM significantly larger than the file size on disk.
    
- **Unfolding (Flattening)**: To feed an image into a standard Multi-Layer Perceptron (MLP), it is often "unrolled" column-wise into a long vector $x \in \mathbb{R}^d$, where $d = R \times C \times 3$.
    - _Example_: A CIFAR-10 image ($32 \times 32 \times 3$) results in an input vector of $3,072$ values.

## 3. Video Data

Videos are fundamentally **sequences of images**, known as frames.

- **Representation**: A video $V$ with $T$ frames is represented as a 4D tensor: $V \in \mathbb{R}^{R \times C \times 3 \times T}$.
    
- **Memory Challenges**: Video data dimensions increase exponentially compared to static images.
    - **Full HD Frame (1080p)**: $1080 \times 1920 \times 3 \approx 6 \text{ MB}$ (uncompressed).
    - **1 Second of Video (24 fps)**: $\approx 150 \text{ MB}$.
    
- **Redundancy**: Visual data is highly redundant, which is why compression is vital for storage. However, during training, neural networks typically require the raw, uncompressed values.

## 4. Foundational Operations: Spatial Filtering

Before moving to CNNs, it is important to understand how local information is processed via **[[Spatial Filtering & Correlation]]**.

- **Local Neighborhood ($U$)**: Transformations are applied to a pixel based on the values of its neighbors.
    
- **Correlation**: A local linear filter where the output is a weighted sum of the pixels in neighborhood $U$.
    
    - **Filter/Kernel ($w$)**: A small matrix of weights that defines the operation.
    - **Mechanism**: The kernel "slides" over the image, performing element-wise multiplication and summing the results to produce a new pixel value in the output image.