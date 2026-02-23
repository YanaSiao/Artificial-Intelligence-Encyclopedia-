## Global Average Pooling (GAP)

To use CAM, the network must use GAP instead of Flattening.

- **Process**: It takes the average value of each $H \times W$ feature map.
    
- **Result**: If the last FEN layer has 512 channels of $7 \times 7$, GAP turns it into a $1 \times 1 \times 512$ vector.
    
- **Advantage**: It drastically reduces parameters and preserves a spatial link between the feature maps and the output weights.
    

## Class Activation Maps (CAM)

**CAM** is a technique for **Weakly Supervised Localization**. It allows the model to tell us _where_ it is looking to make a prediction, using only class labels (no bounding boxes needed during training).

### The Architecture

1. **FEN**: Produces feature maps $A_k$ (where $k$ is the channel index).
    
2. **GAP Layer**: Averages each map to a single value $F_k$.
    
3. **Softmax Layer**: A single Dense layer with weights $w_k^c$ connecting the feature $k$ to the class $c$.
    ![[Pasted image 20260219211728.png]]

### The Algorithm

To find the importance of each pixel for Class $c$:

1. Identify the weights $w_1^c, w_2^c, \dots, w_n^c$ that connect the GAP output to the specific class neuron.
    
2. Compute a weighted sum of the original feature maps:
    
    $$M_c = \sum_{k} w_k^c A_k$$
    
3. **Upsample** the resulting map $M_c$ to the original image size to see the "heatmap."
    

### Why and When to use it?

- **Why**: It provides "Explainability." It proves the model is looking at the "Cat" to classify it as a cat, and not just the background.
    
- **When**:
    - When you need **localization** but only have **classification labels** (Data Scarcity of bounding boxes).
        
    - In medical imaging to highlight exactly where a tumor might be.
        
    - To debug "clever Hans" effects (where a model learns the wrong correlation).

- CAM can be included in any pre-trained network, as long as all the **FC layers** at the end are **removed**.
- Computing CAMs from a pre-trained model requires training the **new output layer.**
- The FC used for CAM is simple, with few neurons and **no hidden layers**: 
  Classification performance might drop (in VGG, removing the feature classifier means removing >90% of parameters).
- CAM resolution (localization **accuracy**) can be improved by «anticipating» GAP to larger convolutional feature maps (but this reduces the semantic information within these layers).
- **GAP**: encourages the identification of the **entire object**(as long as all the parts of the values in the activation map concur with the classification).
- **GMP** (Global Max Pooling) instead of GAP promotes **specific discriminative features**, since weights aggregating the activations are trained only from the maximum over the channel.

## Weakly Supervised Learning (WSL)

**What is it?** WSL is a training paradigm where the model is trained on "coarse" or "cheap" labels (image-level labels like _"This image contains a dog"_) but is expected to perform a "fine" task (like _locating the dog_).

- **The Logic**: If a model correctly classifies an image, it must have found the relevant features. WSL simply "asks" the model where those features are.
    
- **Requirements**:
    
    - A model that preserves spatial information until the very end (avoiding multiple Dense layers).
        
    - A mechanism to project the "importance" of deep features back onto the input image.
        
- **When to use**: When you have millions of image labels but no budget to manually draw bounding boxes or pixel-masks for every object.

![[Pasted image 20260219212840.png]]

---

## From Pixels to Regions: Localization Tools

### A. Saliency Maps (Backprop to Image)

Saliency maps tell us which **pixels** the model is most sensitive to.

- **Algorithm**: Compute the gradient of the class score ($y_c$) with respect to the input image pixels ($I$).
    
    - $Saliency = \frac{\partial y_c}{\partial I}$
        
- **Meaning**: If I change this pixel slightly, how much does it change the model's confidence in the "Dog" class?
    
- **Pros**: Very high resolution (pixel-level).
    
- **Cons**: Often "noisy" and can highlight edges rather than the whole object.
    ![[Pasted image 20260219212952.png]]

### B. Grad-CAM (Gradient-weighted Class Activation Mapping)

Grad-CAM is a generalization of CAM. **Requirement**: Unlike CAM, it does **not** require a GAP layer; it can work with almost any CNN.

- **Algorithm**:
    
    1. Pick a target class (e.g., "Dog").
    2. Compute the gradient of the score for that class with respect to the feature maps ($A$) of the last convolutional layer.
    3. Average those gradients to find the "importance" ($\alpha$) of each channel.
    4. The heatmap is the weighted sum of the feature maps: $L_{Grad-CAM} = ReLU(\sum \alpha_k A_k)$.
    
- **Why use it**: It is much more flexible than CAM and provides a clear heatmap of the object's location.
    ![[Pasted image 20260219213202.png]]

---

## Advanced Approaches: Improving Resolution

The biggest problem with CAM and Grad-CAM is that the heatmaps are **blurry** because the last feature maps are very small (e.g., $7 \times 7$).

### A. Augmented Grad-CAM / Guided Grad-CAM

- **Guided Grad-CAM**: Combines the high-resolution detail of **Saliency Maps** with the class-specific localization of **Grad-CAM**.
    
- **Result**: You get a visualization that is both sharp (shows the cat's whiskers) and specific (only shows the cat, not the background).
    ![[Pasted image 20260219213257.png]]

### B. The Super-Resolution Approach

In advanced WSL, researchers use "Super Resolution" or **Multi-scale** techniques to sharpen heatmaps:

1. **Iterative Erasing**: The model finds the most important part of the object, "erases" it, and is forced to find the next most important part. This helps the model see the _whole_ object rather than just the most discriminative part (like just the face).
    
2. **Sub-pixel CNNs**: Using upsampling layers (like those in Super-Resolution networks) to learn how to upscale the $7 \times 7$ heatmap to the original $224 \times 224$ size more intelligently than standard Bilinear interpolation.
    

---

## Summary Table: Explanation Methods

|**Method**|**Level of Detail**|**Requirements**|**Best For...**|
|---|---|---|---|
|**Occlusion**|Coarse (Boxy)|None|Identifying which _part_ of an image is key.|
|**Saliency Maps**|Fine (Pixels)|Gradients|Seeing edges and textures.|
|**CAM**|Medium (Blob)|GAP Layer|Localizing objects with specific architectures.|
|**Grad-CAM**|Medium (Blob)|Gradients|Localizing objects in _any_ CNN.|
|**Guided Grad-CAM**|Fine & Specific|Gradients|High-quality visualization for reports.|