Image segmentation is the process of partitioning a digital image into multiple segments (sets of pixels). The goal is to simplify or change the representation of an image into something that is more meaningful and easier to analyze.

## 1. Problem Formulation

- **Input**: An image $I \in \mathbb{R}^{R \times C \times 3}$ (Height $\times$ Width $\times$ Channels).
    
- **Goal**: Estimate a partition $\{R_i\}$ of the image domain $X$ such that:
    
    1. $\bigcup R_i = X$ (Every pixel is assigned to a region).
        
    2. $R_i \cap R_j = \emptyset$ for $i \neq j$ (Regions are disjoint; no pixel belongs to two regions).
        
- **Output**: A **Segmentation Mask** where each pixel value corresponds to a class ID.
    

---

## 2. Supervised vs. Unsupervised Segmentation

Segmentation can be approached in two ways:

1. **Unsupervised Segmentation (Clustering)**:
    
    - **Logic**: Group pixels based on low-level features like color similarity, texture, or proximity without knowing what the objects are.
    - **Example**: SLIC (Simple Linear Iterative Clustering) or "Superpixels."
    
2. **Supervised (Semantic) Segmentation**:
    
    - **Logic**: Every pixel is assigned a predefined class label (e.g., "Person," "Road," "Sky").
        
    - **Requirement**: Requires "Dense Annotations" where humans have labeled every pixel in the training set.
        

---

## 3. Semantic vs. Instance Segmentation

It is critical to distinguish between these two "Supervised" tasks:

- **Semantic Segmentation**: All objects of the same class are treated as a single entity. If there are five cars, they all get the "Car" label and appear as one continuous blob.
    
- **Instance Segmentation**: Distinguishes between individual objects. It would label "Car 1," "Car 2," etc., giving them distinct boundaries and IDs.
    

---

## 4. The "Vanilla" Solution: Patch-based Classification

Before advanced architectures like U-Net existed, the "Vanilla" approach treated segmentation as a massive classification problem.

### The Algorithm

1. Define a grid of points across the image.
    
2. For each point, crop a small **patch** (e.g., $28 \times 28$) centered on that pixel.
    
3. Feed each patch into a standard **Classification CNN** (like AlexNet or VGG).
    
4. The network predicts a single class for that patch, which is then assigned to the central pixel.
    
5. Repeat for all pixels to build the final mask.
    

### Conditions & Requirements

- **Annotations**: Requires dense, pixel-level ground truth.
    
- **Processing**: Extremely slow because the network must run a forward pass for every single pixel (or grid point) in the image.
    

### Pros and Cons

- **Pros**: You can use a standard pre-trained classification FEN.
    
- **Cons**:
    
    - **Inefficiency**: Huge computational redundancy (overlapping patches share 99% of the same pixels but are processed separately).
        
    - **Context Loss**: The patch size limits the "view." A $28 \times 28$ patch might only see "gray texture" and not realize it's part of a "Road" because it can't see the car or the sidewalk.