The **Feature Extraction Network (FEN)** is the core of a CNN, responsible for transforming an input image into a sequence of volumes. As the data progresses deeper into the FEN, the spatial resolution (height and width) typically decreases, while the number of channels (feature maps) increases.
## 1. Trainable Parameters

Unlike Dense (MLP) layers where every input connects to every output, Convolutional layers use **Sparse Connectivity** and **Weight Sharing** to drastically reduce the number of parameters.

- **Filters (Kernels)**: The parameters of a convolutional layer are the weights within its filters.
- **Weight Sharing**: The same filter is slid across the entire spatial extent of the input, meaning the same weights are reused at every position.
- **Bias**: A single bias value is typically associated with each filter and is shared across all neurons in that filter's output channel.
### Parameter Calculation Formula

For a layer with $N_F$ filters of size $K \times K$ and an input with $C$ channels:

- **Weights**: $(K \times K \times C) \times N_F$.
- **Biases**: $N_F$ (one per filter).
- **Total Parameters**: $[(K \times K \times C) + 1] \times N_F$.
  
## 2. The Convolutional Layer

The fundamental building block of a CNN. Unlike a dense layer that looks at every pixel, a convolutional layer focuses on **local patches**.

- **The Operation**: A filter (kernel) slides over the input. At each position, it computes a linear combination (dot product) between its weights and the input pixels across **all channels**.
    
- **Purpose**: To detect specific patterns (edges, textures, shapes).
    
- **Weight Sharing**: The same filter is used for the whole image, which drastically reduces the number of parameters and provides **Translation Equivariance**.
    ![[Pasted image 20260217224253.png]]


### A Special Case of Dense

Mathematically, a Convolution is a linear operation. This means any Convolutional layer can be represented as a **Fully Connected (FC) layer** with a very specific weight matrix structure.

| **Feature**        | **Dense (MLP) Layer**                                                 | **Convolutional Layer**                                                                   |
| ------------------ | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Connectivity**   | **Global**: Every output neuron is connected to _every_ input neuron. | **Local**: Every output neuron is connected only to a small local patch of the input.     |
| **Parameters**     | **Unique**: Every connection has its own independent weight.          | **Shared**: The same set of weights (the kernel) is reused across the whole image.        |
| **Weight Matrix**  | **Dense**: Usually full of non-zero values.                           | **Sparse & Structured**: Mostly zeros (Sparse) with a block-diagonal/circulant structure. |
| **Inductive Bias** | None (Generic).                                                       | **Locality & Translation Equivariance**: Assumes nearby pixels are related.               |

### The Weight Matrix $W$ Structure

If we "unroll" (flatten) an input image into a 1D vector, the Convolutional layer's weight matrix $W$ would exhibit these properties:

1. **Sparsity (Local Connectivity)**: Most entries are **zero**. A specific output neuron only "looks" at a few input neurons (those in its $K \times K$ patch). This creates a **Block Diagonal** appearance.
    
2. **Parameter Sharing**: The non-zero weights in the matrix are not unique. The same values appear repeatedly, just shifted to different positions. This is known as a **Toeplitz** or **Circulant** matrix structure.
    
3. **Bias Vector $b$**: While a Dense layer has a unique bias for every output neuron, a Conv layer has a **block-wise constant** bias. All neurons in the same output channel share a single bias value.

### Why use Conv instead of Dense?

1. **Statistical Efficiency**: By sharing weights, we need to learn far fewer parameters, which reduces the risk of overfitting (Generalization).
    
2. **Memory Efficiency**: We only store the small kernel ($3 \times 3$) instead of a massive matrix.
    
3. **Translation Equivariance**: If an object moves in the image, its feature representation moves accordingly because the same filter is applied everywhere.

### Receptive field
The **Receptive Field (RF)** is the region in the input space that a particular CNN feature is looking at.

- **Local to Global**: In the first layer, a neuron has a small RF (equal to the kernel size $K \times K$).
    
- **Hierarchical Growth**: As we go deeper, the RF grows. A neuron in Layer 2 "sees" a patch of Layer 1, which in turn "sees" a larger patch of the original image.
    
- **Influencers**: The RF increases significantly with **Stride > 1**, **Pooling**, and **Dilation**.

## 3. Pooling Layers

Pooling layers are used to reduce the spatial dimensions ($H \times W$) of the feature maps. They have no trainable parameters.

- **Max Pooling**: Picks the maximum value in the window. This is the most common type as it effectively "activates" if a feature is present anywhere in the window, providing **Translation Invariance**.
    
- **Average Pooling**: Calculates the average of the window. It is often used at the very end of the FEN (Global Average Pooling) but is less common in middle layers as it "smooths out" features.
    
- **Purpose**: Reduces computational load, controls overfitting, and helps the network "see" larger areas of the image (increases the receptive field).

## 4. Activation Layers (ReLU)

Convolutions are linear operations. To learn complex patterns, we must inject non-linearity after every convolutional layer.

- **ReLU (Rectified Linear Unit)**: The standard choice. It computes $f(x) = \max(0, x)$.
    
- **Purpose**: It "extinguishes" negative activations (setting them to zero) and keeps positive ones. This helps the network learn which features are present vs. absent.
    
- **Training Benefit**: ReLU helps avoid the vanishing gradient problem, making it much faster to train deep networks compared to Sigmoid or Tanh.

## 5. Hyperparameters: Stride, Padding, and Dilation

### Stride

Stride is the "step size" the filter takes as it slides.

- **Bigger Stride**: The output size **decreases** (downsampling). It speeds up the network and increases the receptive field faster, but you risk "skipping" over small, fine-grained details.
    
- **Smaller Stride (e.g., 1)**: The output size stays larger. It preserves more spatial information and overlap but is computationally more expensive.
    

[Image showing the difference between stride 1 and stride 2 in a convolutional layer]

### Padding

Padding involves adding "fake" pixels (usually zeros) around the border of the input.

- **Bigger Padding**: Preserves the spatial dimensions ($H \times W$) of the input. It ensures that pixels at the **edges** are treated with the same "importance" as central pixels.
    
- **Smaller/No Padding ("Valid")**: The output size **shrinks** significantly after each layer. Information at the borders is effectively "lost" because the filter cannot center itself on edge pixels.

### Dilation

- **Dilation > 1**: Introduces "holes" between filter weights. This allows the filter to cover a much larger area (wider Receptive Field) without adding any extra trainable parameters.


| **Hyperparameter** | **Change** | **Effect on Output Size** | **Effect on Receptive Field** | **Effect on Information**       |
| ------------------ | ---------- | ------------------------- | ----------------------------- | ------------------------------- |
| **Stride**         | Increase   | **Decrease**              | Increase                      | Less detail (Downsampling)      |
| **Padding**        | Increase   | **Increase**              | No change                     | Better border preservation      |
| **Pooling Size**   | Increase   | **Decrease**              | Increase                      | Higher Invariance / Less detail |
| **Dilation**       | Increase   | No change                 | **Increase**                  | Captures more global context    |

## 7. The Receptive Field

The **Receptive Field** is the specific region in the input image that a particular output neuron "sees".

- **Hierarchical Growth**: Deeper neurons have a wider receptive field because they aggregate information from multiple neurons in previous layers.
- **Expansion factors**: Max-pooling, convolutions, and strides > 1 all contribute to increasing the receptive field as we move deeper into the network.

### The Additive Influence: Convolutional Layers ($K$)

When you stack convolutional layers with **Stride S = 1**, the receptive field grows linearly. Each new layer adds $(K - 1)$ to the current receptive field.

- **Initial Jump**: $J_{in} = 1$
- **New Jump**: $J_{out} = J_{in} \times S$ (where $S$ is the stride of the current layer).

- **Formula**: $RF_{new} = RF_{old} + (K - 1) \times J_{prev}$
    
- **Example**: Starting from $1 \times 1$ (a single pixel):
    
    - One $3 \times 3$ Conv: $1 + (3 - 1) = 3 \times 3$
    - Two $3 \times 3$ Convs: $3 + (3 - 1) = 5 \times 5$
    - Three $3 \times 3$ Convs: $5 + (3 - 1) = 7 \times 7$
- Example: Suppose you have a sequence: **Input $\rightarrow$ Conv1 ($K=3, S=2$) $\rightarrow$ Conv2 ($K=3, S=1$)**

	- **Start**: $RF = 1$, $J = 1$.
	- **After Conv1**:
	    
	    - $RF = 1 + (3 - 1) \times 1 = \mathbf{3}$
	    - $J = 1 \times 2 = \mathbf{2}$ (The stride "2" is stored here for the next layer).
	    
	- **After Conv2**:
	    
	    - $RF = 3 + (3 - 1) \times 2 = \mathbf{7}$
	    - (In a stride-1 world, this would only be 5. The stride in the _previous_ layer made this layer's kernels "reach" further).

### The Multiplicative Influence: Stride ($S$) and Pooling

The **Stride** of a layer (whether in a Convolution or a Pooling layer) acts as a **multiplier for all subsequent growth**. Once you downsample the feature map, the filters in the next layer "jump" over larger distances in the original image.

- **The "Jump" ($J$)**: This tracks the total stride accumulated by the network.
    
- **Formula**: $RF_{new} = RF_{old} + (K - 1) \times J_{prev}$
    
- **Logic**: A $3 \times 3$ filter in a layer that follows a $2 \times 2$ Max Pool (stride 2) no longer adds 2 pixels to the RF; it adds $2 \times 2 = 4$ pixels.

|**Factor**|**Effect on Receptive Field**|**Purpose**|
|---|---|---|
|**Kernel Size ($K$)**|**Linear Growth**: Larger kernels increase the RF more per layer.|Captures more local context.|
|**Stride ($S > 1$)**|**Exponential Growth**: Multiplies the impact of all future layers.|Reaches a "global" view of the image much faster.|
|**Pooling**|**Same as Stride**: Reduces resolution while doubling the effective "jump" of next layers.|Provides translation invariance and increases RF.|
|**Dilation ($D > 1$)**|**Artificial Expansion**: Effectively makes a $3 \times 3$ kernel act like a $5 \times 5$ or $7 \times 7$ without adding parameters.|Captures wide context while maintaining high resolution.|